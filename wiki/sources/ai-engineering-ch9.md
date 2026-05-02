---
title: "AI Engineering — Ch.9: Inference Optimization"
type: source
created: 2026-04-27
updated: 2026-04-27
tags: [inference, optimization, kv-cache, speculative-decoding, batching, parallelism, flash-attention]
sources: [ai-engineering.pdf]
---

# AI Engineering — Ch.9: Inference Optimization

**Book:** *AI Engineering* by Chip Huyen (O'Reilly, 2024)
**Chapter:** 9 — Inference Optimization
**Pages:** ~429–473

---

## Summary

Chapter 9 is the most technically dense chapter in the book, covering every layer of the inference optimization stack. Its framing insight: inference is fundamentally different from training — it is **memory bandwidth-bound** rather than compute-bound, making the memory hierarchy the primary design constraint.

The chapter starts by distinguishing the two phases of autoregressive generation: **prefill** (processing the prompt in parallel, compute-bound) and **decode** (generating tokens one-by-one, memory bandwidth-bound). Production systems increasingly decouple these phases onto separate hardware (e.g., DistServe) to optimize each independently.

Performance metrics get precise definitions: TTFT (time to first token, measures prefill latency), TPOT (time per output token, measures decode throughput), throughput (tokens/second, requests/second), and **goodput** — the fraction of requests that meet their SLO, the most business-relevant metric. GPU utilization via `nvidia-smi` is misleading; Model FLOP/s Utilization (MFU) and Model Bandwidth Utilization (MBU) are better proxies for actual efficiency.

The memory hierarchy section grounds all subsequent optimization discussion: CPU DRAM (25–50 GB/s bandwidth), GPU HBM (256 GB/s–1.5 TB/s), GPU SRAM (>10 TB/s, ~40 MB). Every optimization technique is ultimately about moving computation into higher-bandwidth memory tiers.

Model-level optimizations include quantization (Ch.7), distillation (Ch.8), pruning (sparse models, less production-proven), and **speculative decoding**: a small draft model generates K candidate tokens, the target model verifies all K in a single parallel forward pass. For Chinchilla-70B, this achieves ~2× speedup. A simpler variant, **inference with reference**, copies tokens directly from the input when output overlaps with input (e.g., in RAG contexts with repeated passages).

**KV cache** is the single most impactful optimization in production. Autoregressive attention recomputes key-value pairs for every past token on every decoding step; caching these vectors eliminates the redundant computation. Memory cost: 2×B×S×L×H×M bytes. Llama 2 13B with batch=32 and seq=2048 requires 54 GB of KV cache alone. Optimizations: PagedAttention (vLLM — eliminates fragmentation by paging KV cache like OS virtual memory), multi-query attention, grouped-query attention, cross-layer attention sharing, KV cache quantization. Character.AI reduced their KV cache by 20× using three combined techniques.

**FlashAttention** fuses the attention computation into a single GPU kernel, dramatically reducing HBM reads/writes and enabling much longer sequences within the same memory budget.

Service-level optimizations: **continuous batching** (in-flight batching) allows new requests to join a batch mid-generation, dramatically improving GPU utilization versus static batching. **Prompt caching** (Anthropic) stores computed KV pairs for repeated prompt prefixes — up to 90% cost savings and 79% latency reduction for 100K-token cached prompts. Parallelism strategies — replica, tensor (intra-operator), pipeline (inter-layer), context, sequence — allow scaling across multiple GPUs/nodes.

---

## Key Takeaways

- **Prefill vs decode**: compute-bound vs memory bandwidth-bound. Production systems decouple them (DistServe).
- **Goodput** = SLO-satisfying requests/second. More meaningful than raw throughput for business decisions.
- **MFU/MBU** are better utilization metrics than `nvidia-smi`'s GPU utilization (which just measures kernel scheduling).
- **Inference costs up to 90% of ML spend** — optimization here has outsized ROI vs. training optimization.
- **Speculative decoding**: draft model + parallel verification → ~2× speedup for large models.
- **KV cache**: single biggest production win. PagedAttention (vLLM) eliminates fragmentation. Multi-query/grouped-query attention reduce cache size. Character.AI achieved 20× reduction.
- **FlashAttention**: kernel fusion for attention → less HBM traffic, longer context, faster training and inference.
- **Continuous batching**: new requests join mid-generation. Much better GPU utilization than static batching.
- **Prompt caching**: Anthropic's implementation saves up to 90% cost and 79% latency for long repeated prefixes.
- **Parallelism**: replica (embarrassingly parallel, different requests), tensor (split weight matrices across GPUs), pipeline (split layers across GPUs), context/sequence (split long sequences).

---

## Notable Quotes

> "Inference can account for up to 90% of ML costs in production."

> "Goodput is the number of requests per second that meet their SLO — the most business-relevant throughput metric."

---

## Pages Created / Updated

**Created:**
- [[Inference Optimization]] — metrics, accelerators, model optimization, KV cache, speculative decoding, batching, parallelism

**Updated:**
- [[Quantization]] — added inference-time quantization context
- [[RAG]] — added prompt caching relevance
