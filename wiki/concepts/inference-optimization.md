---
title: "Inference Optimization"
type: concept
created: 2026-04-27
updated: 2026-04-27
tags: [inference, optimization, kv-cache, speculative-decoding, flash-attention, batching, parallelism, goodput]
sources: [ai-engineering-ch9.md]
---

# Inference Optimization

Inference optimization is the discipline of making trained models faster, cheaper, and more scalable to serve in production. It is arguably more impactful than training optimization for most AI engineering teams, since **inference accounts for up to 90% of ML costs** in production.

## The Fundamental Bottleneck

Autoregressive LLM inference has two distinct phases with different bottlenecks:

| Phase | Description | Bottleneck |
|---|---|---|
| **Prefill** | Process the entire input prompt in parallel | Compute-bound (FLOPs) |
| **Decode** | Generate one token at a time, autoregressively | Memory bandwidth-bound |

The decode phase is the primary target for optimization: the model must load all weights from GPU HBM (high-bandwidth memory) for every single token generated. With billions of parameters, this is enormous per-token memory traffic.

Production systems increasingly **decouple prefill and decode** onto separate hardware (e.g., DistServe), optimizing each phase independently.

## Performance Metrics

| Metric | Definition | What It Measures |
|---|---|---|
| **TTFT** | Time to first token | Prefill latency; user-perceived responsiveness |
| **TPOT** | Time per output token | Decode throughput; generation speed |
| **Throughput** | Tokens/second or requests/second | System capacity |
| **Goodput** | SLO-satisfying requests/second | Business-relevant efficiency |

**Goodput** is the most important business metric: a system running at 95% GPU utilization but dropping 30% of requests outside their latency SLO has low goodput despite high throughput.

**Utilization metrics**:
- `nvidia-smi` GPU utilization: whether a GPU kernel is running. Often misleadingly high.
- **MFU** (Model FLOP/s Utilization): actual FLOPs vs peak hardware FLOPs. Better for compute-bound phases.
- **MBU** (Model Bandwidth Utilization): actual memory bandwidth used vs peak HBM bandwidth. Better for memory-bound phases.

## Memory Hierarchy

Understanding optimization requires understanding where data lives:

| Tier | Bandwidth | Capacity | Use |
|---|---|---|---|
| CPU DRAM | 25–50 GB/s | Hundreds of GB | Model storage when offloaded |
| GPU HBM | 256 GB/s – 1.5 TB/s | 40–80 GB | Model weights during inference |
| GPU SRAM | >10 TB/s | ~40 MB | Active compute (registers, cache) |

Every optimization is ultimately about moving computation from low-bandwidth tiers to high-bandwidth tiers, or reducing total memory traffic.

## AI Accelerators

Inference runs on specialized hardware:
- **GPUs**: thousands of CUDA cores for parallel computation. NVIDIA H100, A100 are production standards.
- **TPUs**: Google's Tensor Processing Units; matrix multiply accelerators
- **AWS Inferentia**: inference-optimized chips, cheaper per token than GPUs for some workloads
- **Apple Neural Engine**: on-device inference acceleration for consumer hardware

## Model-Level Optimizations

### Quantization
Reduce numerical precision of weights/activations (INT8, INT4). Directly reduces memory footprint and memory bandwidth required per token. See [[Quantization]].

### Distillation
Train a smaller student to mimic a larger teacher. Reduces model size while preserving capability. See [[Data Synthesis]].

### Pruning
Remove weights below a magnitude threshold (unstructured pruning) or entire attention heads/layers (structured pruning). Creates sparse models. Less common in production than quantization — hardware support for sparse compute is still maturing.

### Speculative Decoding
The key insight: the decode phase is serial (one token at a time), but verification is parallelizable.

1. **Draft model** (small, fast) generates K candidate tokens
2. **Target model** (large, accurate) verifies all K tokens in a single parallel forward pass
3. Accept tokens up to the first mismatch; the target model generates one corrected token at the mismatch point
4. Repeat

Result: multiple tokens accepted per target model forward pass → **~2× speedup** for Chinchilla-70B with no quality loss (accepted tokens are provably equivalent to target model sampling).

The draft model must be fast and share the same vocabulary. Common approach: use a smaller version of the same model family.

**Inference with reference**: a simpler variant for scenarios where output overlaps with input (e.g., RAG with long retrieved passages). When the model would output tokens already present in the input, copy them directly. Achieves ~2× speedup for these scenarios.

### Parallel Decoding
Generate multiple candidate continuations simultaneously:
- **Medusa**: adds multiple decoding heads to the model, each predicting a future token. Tree-based attention evaluates all head combinations. Achieves ~1.9× speedup.
- **Lookahead / Jacobi decoding**: iterative refinement of a draft sequence using the full model in a fixed-point iteration.

## KV Cache

The single most impactful optimization in production LLM serving.

**The problem**: in attention, every new token must attend to all previous tokens, requiring key-value (KV) vectors for every past position. Without caching, these are recomputed from scratch on every decoding step.

**The solution**: store (cache) the key and value vectors after they are first computed. On subsequent steps, load from cache rather than recomputing.

**Memory cost formula**:
```
KV cache memory = 2 × B × S × L × H × M bytes
```
Where: B = batch size, S = sequence length, L = layers, H = attention heads, M = bytes per element.

**Example**: Llama 2 13B with batch=32, seq=2048 → **54 GB** of KV cache. This can exceed model weight memory.

### KV Cache Optimizations

| Technique | Description | Impact |
|---|---|---|
| **PagedAttention** (vLLM) | Pages KV cache like OS virtual memory; eliminates fragmentation | Near-optimal GPU memory utilization |
| **Multi-Query Attention (MQA)** | All heads share a single K and V | Large cache reduction |
| **Grouped-Query Attention (GQA)** | Groups of heads share K and V (compromise between MHA and MQA) | Significant cache reduction with small quality trade-off |
| **Cross-Layer Attention** | Share KV between adjacent layers | Reduces cache per token |
| **KV Cache Quantization** | Store KV in INT8 or INT4 | Halves/quarters cache memory |
| **Prompt Caching** | Cache KV for repeated prompt prefixes (server-side) | Eliminates prefill cost for repeated prefixes |

**Character.AI case**: reduced KV cache by **20×** by combining three techniques (MQA, cross-layer sharing, and quantization).

**Prompt caching (Anthropic)**: for a 100K-token cached prompt, achieves up to **90% cost savings** and **79% latency reduction**. The KV pairs for the prompt prefix are computed once and reused across requests.

## Attention Optimization

### FlashAttention
Standard attention computes Q×K^T, applies softmax, then multiplies by V — with multiple HBM reads/writes. FlashAttention **fuses these into a single GPU kernel** using tiling techniques that keep intermediate results in fast SRAM rather than writing to HBM.

**Benefits**: less HBM bandwidth → faster computation, enables longer sequences within same memory budget, supports gradients (enabling training use too).

### Kernel Optimization
Custom GPU kernels outperform generic PyTorch operations by:
- **Vectorization**: process multiple elements per instruction
- **Parallelization**: maximize thread occupancy
- **Loop tiling**: reuse data in fast cache between iterations
- **Operator fusion**: combine multiple operations into one kernel (fewer HBM reads/writes)

Tools: `torch.compile`, XLA (Google), TensorRT (NVIDIA).

## Service-Level Optimizations

### Batching Strategies

| Strategy | Description | Efficiency |
|---|---|---|
| **Static batching** | Fixed batch; wait for all requests to complete | Low GPU utilization |
| **Dynamic batching** | Collect requests into batches with a time window | Better utilization |
| **Continuous (in-flight) batching** | New requests join the batch mid-generation; completed sequences leave immediately | Highest GPU utilization |

Continuous batching (pioneered by Orca, adopted by vLLM) is now the production standard. It eliminates the "waiting for the slowest response" problem of static batching.

### Prefill/Decode Decoupling (DistServe)
Prefill is compute-bound; decode is memory bandwidth-bound. They have different hardware optima. DistServe runs them on separate GPU pools, routing prefill-complete requests to decode servers. Improves both TTFT and throughput.

### Prompt Caching
See KV Cache section above. Dramatically reduces cost for applications with repeated system prompts or few-shot examples.

## Parallelism Strategies

| Strategy | Splits On | Use Case |
|---|---|---|
| **Replica** | Requests (different GPUs handle different requests) | Scale requests/second |
| **Tensor parallelism** | Weight matrices (split across GPUs within a layer) | Reduce per-GPU memory for large models |
| **Pipeline parallelism** | Layers (each GPU handles a range of layers) | Very large models; adds inter-GPU latency |
| **Context parallelism** | Sequence positions (split long sequences) | Very long contexts |
| **Sequence parallelism** | Sequence dimension during attention | Similar to context parallelism |

Hybrid parallelism (combining multiple strategies) is used for the largest models (GPT-4 scale).

## Connections

- [[Quantization]] — primary model-level inference optimization
- [[Data Synthesis]] — distillation produces smaller models for inference
- [[KV Cache]] — see KV Cache section; key concept referenced throughout
- [[RAG]] — prompt caching reduces inference cost for RAG's long contexts
- [[AI Engineering Architecture]] — inference optimization is the performance layer of production systems
- [[Foundation Models]] — inference optimization determines whether foundation models are economically viable in production
