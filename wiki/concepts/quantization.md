---
title: "Quantization"
type: concept
created: 2026-04-27
updated: 2026-04-27
tags: [quantization, compression, inference, finetuning, numerical-precision, qlora]
sources: [ai-engineering-ch7.md, ai-engineering-ch9.md]
---

# Quantization

Quantization reduces the numerical precision of model weights and/or activations from high-precision formats (FP32, FP16) to lower-precision formats (INT8, INT4). It is one of the primary techniques for making large models practical for inference and finetuning within memory and latency budgets.

## Why It Matters

Model weights dominate GPU memory usage. A 70B-parameter model in FP32 requires 280 GB of memory — more than any single GPU. Quantizing to INT8 halves this to 140 GB; INT4 to 70 GB. Beyond memory, lower-precision formats execute faster on hardware that has dedicated low-precision compute units (INT8 tensor cores, etc.).

## Numerical Representation Formats

| Format | Bytes/param | Exponent bits | Mantissa bits | Notes |
|---|---|---|---|---|
| FP32 | 4 | 8 | 23 | Full precision; standard for training |
| FP16 | 2 | 5 | 10 | Narrower dynamic range; can overflow |
| BF16 | 2 | 8 | 7 | Same exponent range as FP32; preferred for training |
| TF32 | ~2.4 (19-bit) | 8 | 10 | NVIDIA A100+ tensor cores; auto mixed precision |
| INT8 | 1 | — | — | Integer; 256 discrete values |
| INT4 | 0.5 | — | — | 16 discrete values; aggressive; QLoRA uses NF4 variant |
| NF4 | 0.5 | — | — | Normal Float 4; designed for normally-distributed weights |

**BF16 vs FP16**: BF16 has 8 exponent bits (same as FP32), giving it the same dynamic range. FP16 has only 5 exponent bits — it can overflow during training when gradients are large. BF16 is the preferred half-precision format for training.

**BitNet b1.58**: A research direction where weights are constrained to {-1, 0, +1} — 1.58 bits (log₂(3)). Claims near-FP16 quality at a fraction of the memory. Early results promising but not yet production mainstream.

## Quantization Methods

### Post-Training Quantization (PTQ)
Quantize a trained model after the fact, with no retraining. The most practical approach — fast, requires no labeled data (just a small calibration set for some methods). Most production quantization is PTQ.

**Approaches**:
- **Naive rounding**: round weights to nearest quantized value. Simple but can degrade quality.
- **Calibration-based**: run a small dataset through the model, observe activation ranges, set quantization scales accordingly (e.g., GPTQ, AWQ).
- **Per-channel vs per-tensor**: per-channel (different scale per output neuron) is more accurate but slightly more complex.

### Quantization-Aware Training (QAT)
Simulate quantization during training with "fake quantization" — maintain FP32 weights internally but simulate integer rounding in the forward pass. The model learns to be robust to quantization. More accurate than PTQ but requires access to the training pipeline and dataset. Used by large labs that can afford the overhead.

### Mixed Precision Training
Not strictly quantization, but related: use FP16/BF16 for forward and backward pass compute, maintain FP32 "master weights" for optimizer state updates. Achieves ~2× memory reduction for compute while preserving training stability.

**QLoRA** is the most impactful mixed-precision scheme for finetuning:
- Base model stored in 4-bit NF4
- Dequantize to BF16 for the forward pass computation
- [[LoRA]] adapters trained in BF16
- Paged optimizers handle OOM during long sequences

## Quantization and Inference

Quantization at inference time reduces:
1. **Memory**: fits larger models on fewer/smaller GPUs
2. **Memory bandwidth**: fewer bytes moved per weight load → faster decode (memory bandwidth-bound phase)
3. **Compute latency**: INT8 tensor cores often execute faster than FP16

The bandwidth reduction matters particularly for the decode phase of autoregressive generation, which is memory bandwidth-bound (see [[Inference Optimization]]).

## Quality Trade-offs

| Precision | Quality Impact |
|---|---|
| FP32 → BF16/FP16 | Negligible for most tasks |
| FP16 → INT8 | Minor; usually acceptable |
| INT8 → INT4 | Noticeable; depends on model and calibration method |
| INT4 → 1-bit | Significant (current research frontier) |

Quality degradation is task-dependent. Reasoning-intensive tasks tend to degrade faster than retrieval-intensive tasks under aggressive quantization.

## Connections

- [[Finetuning]] — QLoRA enables finetuning 65B models on a single GPU
- [[LoRA]] — QLoRA = LoRA + 4-bit base model quantization
- [[Inference Optimization]] — quantization is one of the primary model-level inference optimizations
- [[AI Engineering Stack]] — quantization decisions sit at the model development layer
