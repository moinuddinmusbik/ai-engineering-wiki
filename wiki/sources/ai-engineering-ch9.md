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

---

## References (89 links)

All hyperlinks embedded by the author in this chapter, extracted from the book PDF. See also the [complete URL reference](../analyses/ai-engineering-urls.md) across all chapters.


| URL | Name / Link Text | Description |
|-----|-----------------|-------------|
| https://oreil.ly/M_aGR | Williams et al., 2009 | Profiling tools like NVIDIA Nsight will show you a roofline chart to tell you whether |
| https://oreil.ly/K3j6t | arithmetic | Profiling tools like NVIDIA Nsight will show you a roofline chart to tell you whether your workload is compute-bound or memory bandwidth-bound, as shown in |
| https://oreil.ly/jD5Pj | “Lufthansa Delays Chatbot’s Responses to Make It More ‘Human’” | See “Lufthansa Delays Chatbot’s Responses to Make It More ‘Human’” (Ry Crozier, iTnews, May 2017). |
| https://oreil.ly/zHsb8 | NVIDIA | 10 An experiment by Anyscale shows that 100 input tokens have approximately the same impact on the overall latency as a single output token. |
| https://en.wikipedia.org/wiki/Goodput | goodput | Instead, some teams focus on goodput, a metric adapted from networking for LLM applications. |
| https://oreil.ly/ludJ2 | nvidia-smi | The official NVIDIA tool for monitoring GPU usage is nvidia-smi—SMI stands for System Management Interface. |
| https://oreil.ly/tOOOD | (Databricks, 2024 | Image from “LLM Training and Inference with Intel Gaudi 2 AI Accelerators” (Databricks, 2024). |
| https://en.wikipedia.org/wiki/Perceptrons_(book) | Perceptrons: An Introduction to Computational Geometry | Their exact quote was: “Virtually nothing is known about the computational capabilities of this latter kind of machine. |
| https://oreil.ly/mRNCP | rename the GPU | wasn’t sufficient compute power to dispute their argument, which was then cited by many people as a key reason for the drying up of AI funding in the 1970s. |
| https://oreil.ly/iK0tN | interview | reason for the drying up of AI funding in the 1970s. |
| https://oreil.ly/Yv4V7 | Krizhevsky et al., | The revival of interest in deep learning in 2012 was also closely tied to compute. |
| https://en.wikipedia.org/wiki/Graphics_processing_unit | GPUs | Compared to thousands of CPUs, a couple of GPUs were a |
| https://oreil.ly/Xpwco | Google released just a few | Compared to thousands of CPUs, a couple of GPUs were a lot more accessible to PhD students and researchers, setting off the deep learning research boom. |
| https://arxiv.org/abs/2007.00072 | “Data Movement Is All You Need: A Case Study on | 15 Matrix multiplication, affectionately known as matmul, is estimated to account for more than 90% of all float‐ ing point operations in a neural network, acco |
| https://arxiv.org/abs/1802.04799 | “Scalable MatMul-free Language | 15 Matrix multiplication, affectionately known as matmul, is estimated to account for more than 90% of all float‐ ing point operations in a neural network, acco |
| https://en.wikipedia.org/wiki/List_of_AMD_graphics_processing_units | Advanced Micro Devices (AMD)’s newer generations of | it imposes challenges on memory design and power consumption. |
| https://en.wikipedia.org/wiki/Tensor_Processing_Unit | Tensor Processing Unit | While many chips can handle both training and inference, one big theme emerging is |
| https://oreil.ly/oDQOk | Intel’s Habana Gaudi | While many chips can handle both training and inference, one big theme emerging is |
| https://oreil.ly/6ySTY | Graphcore’s | While many chips can handle both training and inference, one big theme emerging is |
| https://oreil.ly/R7gXn | Groq’s Language Processing Unit | While many chips can handle both training and inference, one big theme emerging is specialized chips for inference. |
| https://oreil.ly/ACIty | Cerebras’ | While many chips can handle both training and inference, one big theme emerging is specialized chips for inference. |
| https://en.wikipedia.org/wiki/List_of_quantum_processors | Quant Processing Unit | While many chips can handle both training and inference, one big theme emerging is specialized chips for inference. |
| https://oreil.ly/qSpMK | Desislavov et al. (2023) | A survey by Desislavov et al. |
| https://oreil.ly/y45q6 | run fast on | For example, the transformer was originally designed by Google to run fast on TPUs and only later optimized on GPUs. |
| https://en.wikipedia.org/wiki/Neural_Engine | Neural Engine | Examples of such chips include the Apple Neural Engine, AWS Inferentia, and MTIA (Meta Training and Inference Accelerator). |
| https://oreil.ly/42LSB | AWS Inferentia | Examples of such chips include the Apple Neural Engine, AWS Inferentia, and MTIA (Meta Training and Inference Accelerator). |
| https://oreil.ly/XH2bh | MTIA | Examples of such chips include the Apple Neural Engine, AWS Inferentia, and MTIA (Meta Training and Inference Accelerator). |
| https://oreil.ly/m8daG | Google’s Edge | Chips designed for edge computing, like Google’s Edge TPU and the NVIDIA Jetson Xavier, are also typically geared toward inference. |
| https://oreil.ly/PRZSQ | NVIDIA Jetson Xavier | Chips designed for edge computing, like Google’s Edge TPU and the NVIDIA Jetson Xavier, are also typically geared toward inference. |
| https://oreil.ly/bNAOG | NVIDIA H100 | Table 9-2 shows the FLOP/s specs for different precision formats for NVIDIA H100 SXM chips. |
| https://en.wikipedia.org/wiki/GDDR_SDRAM | GDDR | Understanding Inference Optimization \| 423 |
| https://en.wikipedia.org/wiki/DDR_SDRAM | DDR SDRAM | To be more specific, CPUs typically use DDR SDRAM (Double Data Rate Synchro‐ nous Dynamic Random-Access Memory), which has a 2D structure. |
| https://en.wikipedia.org/wiki/High_Bandwidth_Memory | HBM | GPUs, particu‐ larly high-end ones, often use HBM (high-bandwidth memory), which has a 3D stacked structure.17 An accelerator’s memory is measured by its size a |
| https://en.wikipedia.org/wiki/CUDA | CUDA | The latter is AMD’s open source alternative to NVIDIA’s proprietary CUDA. |
| https://github.com/triton-lang/triton | OpenAI’s Triton | Flow don’t yet allow fine-grained control of memory access. |
| https://github.com/ROCm/ROCm | ROCm | researchers and engineers to become interested in GPU programming languages such as CUDA (originally Compute Unified Device Architecture), OpenAI’s Triton, and |
| https://oreil.ly/5vRsP | 54 billion | A GPU can have billions of tran‐ sistors—an NVIDIA A100 has 54 billion transistors, while an NVIDIA H100 has 80 billion. |
| https://en.wikipedia.org/wiki/Hopper_(microarchitecture) | Hopper (microarchitecture) | A GPU can have billions of tran‐ sistors—an NVIDIA A100 has 54 billion transistors, while an NVIDIA H100 has 80 billion. |
| https://oreil.ly/RqY-3 | environment | Chip energy consumption threatens to have a staggering impact on the environment, increasing the pressure on companies to invest in technologies for green data |
| https://en.wikipedia.org/wiki/Green_data_center | green data centers | Chip energy consumption threatens to have a staggering impact on the environment, increasing the pressure on companies to invest in technologies for green data |
| https://oreil.ly/5hFSF | Cerebras (2024) | The experiment was conducted by Cerebras (2024). |
| https://arxiv.org/abs/1810.05270 | Liu et al., 2018 | Pruning can help discover promising model architectures (Liu et al., 2018). |
| https://arxiv.org/abs/1710.01878 | Zhu et al., 2017 | These pruned architectures, smaller than the pre-pruned architectures, can also be trained from scratch (Zhu et al., 2017). |
| https://oreil.ly/qwlHE | Frankle and Carbin (2019) | from scratch (Zhu et al., 2017). |
| https://oreil.ly/QYdG8 | Kadous et al., 2023 | In an experi‐ ment, Anyscale found that a single output token can have the same impact on latency as 100 input tokens (Kadous et al., 2023). |
| https://arxiv.org/abs/1811.03115 | (Stern et al., 2018 | The image is from “Blockwise Parallel Decoding for Deep Autoregressive Models” (Stern et al., 2018). |
| https://arxiv.org/abs/2302.01318 | Chen et al., 2023 | For example, to speed up the decoding process of Chinchilla-70B, DeepMind trained a 4B-parameter draft model of the same architecture (Chen et al., 2023). |
| https://arxiv.org/abs/2211.17192 | Laviathan et al., 2022 | A similar speed-up was achieved for T5-XXL (Laviathan et al., 2022). |
| https://oreil.ly/IaPOB | 50 lines of | For example, it’s possible to do so in 50 lines of code in PyTorch. |
| https://oreil.ly/uzg1s | vLLM | It’s been incorporated into popular inference frameworks such as vLLM, TensorRT-LLM, and llama.cpp. |
| https://github.com/NVIDIA/TensorRT-LLM | TensorRT-LLM | It’s been incorporated into popular inference frameworks such as vLLM, TensorRT-LLM, and llama.cpp. |
| https://github.com/ggerganov/llama.cpp/pull/2926 | llama.cpp | It’s been incorporated into popular inference frameworks such as vLLM, TensorRT-LLM, and llama.cpp. |
| https://arxiv.org/abs/2304.04487 | Yang et al., 2023 | In “Inference with Reference: Lossless Acceleration of Large Language Models” (Yang et al., 2023), this technique helps achieve two times generation speedup in |
| https://arxiv.org/abs/2402.02057 | Fu et al., 2024 | The parallel tokens can be generated by the same decoder, as in Lookahead decoding (Fu et al., 2024), or by different decoding heads, as in Medusa (Cai et al., |
| https://arxiv.org/abs/2401.10774 | Cai et al., 2024 | The parallel tokens can be generated by the same decoder, as in Lookahead decoding (Fu et al., 2024), or by different decoding heads, as in Medusa (Cai et al., |
| https://oreil.ly/FWYf5 | Eassa et al., | NVIDIA claimed Medusa helped boost Llama 3.1 token generation by up to 1.9× on their HGX H200 GPUs (Eassa et al., 2024). |
| https://en.wikipedia.org/wiki/Jacobi_method | Jacobi method | Lookahead decoding uses the Jacobi method21 to verify the gen‐ erated tokens, which works as follows: 1. |
| https://arxiv.org/abs/2211.05102 | (Pope et al., 2022) | A Google paper calculated that for a 500B+ model with multi-head attention, batch size 512, and context length 2048, the KV cache totals 3TB (Pope et al., 2022) |
| https://arxiv.org/abs/2004.05150v2 | Beltagy et al., 2020 | For example, when generating a new token, instead of attending to all previous tokens, local windowed attention attends only to a fixed size window of nearby to |
| https://arxiv.org/abs/2405.12981?ref=research.character.ai | Brandon et al., 2024 | Both cross-layer attention (Brandon et al., 2024) and multi-query attention (Shazeer, 2019) reduce the memory footprint of the KV cache by reducing the number o |
| https://arxiv.org/abs/1911.02150?ref=research.character.ai | Shazeer, | Both cross-layer attention (Brandon et al., 2024) and multi-query attention (Shazeer, 2019) reduce the memory footprint of the KV cache by reducing the number o |
| https://arxiv.org/abs/2305.13245 | Ainslie et al., 2023 | Grouped-query attention (Ainslie et al., 2023) is a generalization of multi-query atten‐ tion. |
| https://oreil.ly/nLt6A | 180 messages | Character.AI, an AI chatbot application, shares that their average conversation has a dialogue history of 180 messages (2024). |
| https://github.com/vllm-project/vllm | vLLM | for applications with long context. |
| https://arxiv.org/abs/2309.06180 | Kwon et al., 2023 | Other techniques include KV cache quantization (Hooper et al., 2024; Kang et al., 2024), adaptive KV cache compression (Ge et al., 2023), and selective KV cache |
| https://arxiv.org/abs/2401.18079 | Hooper et al., 2024 | Other techniques include KV cache quantization (Hooper et al., 2024; Kang et al., 2024), adaptive KV cache compression (Ge et al., 2023), and selective KV cache |
| https://arxiv.org/abs/2403.05527 | Kang et al., | Other techniques include KV cache quantization (Hooper et al., 2024; Kang et al., 2024), adaptive KV cache compression (Ge et al., 2023), and selective KV cache |
| https://arxiv.org/abs/2310.01801 | Ge et al., 2023 | Other techniques include KV cache quantization (Hooper et al., 2024; Kang et al., 2024), adaptive KV cache compression (Ge et al., 2023), and selective KV cache |
| https://oreil.ly/ixtBl | Liu | Other techniques include KV cache quantization (Hooper et al., 2024; Kang et al., 2024), adaptive KV cache compression (Ge et al., 2023), and selective KV cache |
| https://github.com/Dao-AILab/flash-attention | FlashAt‐ | One of the most well-known kernels optimized for attention computation is FlashAt‐ tention (Dao et al., 2022). |
| https://arxiv.org/abs/2407.08608 | Shah et al., | Later on, FlashAttention-3 was introduced for H100 GPUs (Shah et al., 2024). |
| https://oreil.ly/_5Nqa | PyTorch, 2023 | Inference Optimization Case Study from PyTorch Figure 9-14 shows how much throughput improvement the PyTorch team could give to Llama-7B through the following o |
| https://github.com/apache/tvm | Apache TVM | Compilers can be standalone tools, such as Apache TVM and MLIR (Multi-Level Intermediate Representation) or integrated into ML and inference frameworks, like to |
| https://mlir.llvm.org | MLIR | Compilers can be standalone tools, such as Apache TVM and MLIR (Multi-Level Intermediate Representation) or integrated into ML and inference frameworks, like to |
| https://oreil.ly/6bjVM | torch.compile | AI compa‐ nies might have their own compilers, with their proprietary kernels designed to speed |
| https://en.wikipedia.org/wiki/Accelerated_Linear_Algebra | XLA | AI compa‐ nies might have their own compilers, with their proprietary kernels designed to speed |
| https://github.com/openxla/xla | OpenXLA | AI compa‐ nies might have their own compilers, with their proprietary kernels designed to speed up their own workloads.24 |
| https://github.com/NVIDIA/TensorRT | TensorRT | AI compa‐ nies might have their own compilers, with their proprietary kernels designed to speed up their own workloads.24 I f S i O ti i ti |
| https://oreil.ly/SJ7Mb | oreil.ly | It works by selectively batching operations that don’t cause the generation of one response to hold up another, as introduced in the paper Orca (Yu et al., 2022 |
| https://oreil.ly/DlIPs | in-flight batching | Continuous batching, also called in-flight batching, is visualized in Figure 9-16. |
| https://arxiv.org/html/2401.09670v1 | Zhong et al., 2024 | One common optimization technique for inference servers is to disaggregate prefill and decode. |
| https://arxiv.org/abs/2401.11181 | Hu et al., 2024 | One common optimization technique for inference servers is to disaggregate prefill and decode. |
| https://en.wikipedia.org/wiki/NVLink | NVLink | prefill instances to decode instances, the paper shows communication overhead is not substantial in modern GPU clusters with high-bandwidth connections such as |
| https://oreil.ly/eMQ_P | “Llama Inference at Meta” | Inference Optimization \| 443 |
| https://github.com/ggerganov/llama.cpp/blob/master/examples/main/README.md#prompt-caching | prompt caching | Its documentation is limited, but my guess from reading the code is that in a long conversation, it caches the previous messages and processes only the newest m |
| https://oreil.ly/Pd6Pk | Gim et al. | Since its introduction in November 2023 by Gim et al., the prompt cache has been rapidly incorporated into model APIs. |
| https://oreil.ly/pIHkL | functionality | Since its introduction in November 2023 by Gim et al., the prompt cache has been rapidly incorporated into model APIs. |
| https://oreil.ly/8rtsF | prompt caching | Anthropic offers prompt caching that promises up to 90% cost savings (the longer the cached context, the higher the savings) and up to 75% latency reduction. |
| https://oreil.ly/On2-B | context parallelism | In context parallelism, the input sequence itself is split across different devices to be processed separately. |
