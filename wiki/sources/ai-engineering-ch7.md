---
title: "AI Engineering — Ch.7: Fine-Tuning"
type: source
created: 2026-04-27
updated: 2026-04-27
tags: [finetuning, lora, quantization, model-merging, peft, transfer-learning]
sources: [ai-engineering.pdf]
---

# AI Engineering — Ch.7: Fine-Tuning

**Book:** *AI Engineering* by Chip Huyen (O'Reilly, 2024)
**Chapter:** 7 — Fine-Tuning
**Pages:** ~331–386

---

## Summary

Chapter 7 covers the full spectrum of adapting pre-trained models to specific tasks through fine-tuning. It begins by framing fine-tuning within the broader context of [[Model Adaptation]] — the third pillar alongside [[Prompt Engineering]] and [[RAG]]. The chapter then dives deep into the engineering challenges of actually running fine-tuning at scale: memory bottlenecks, numerical representation formats, and the family of parameter-efficient techniques that make fine-tuning practical on commodity hardware.

The central thesis on when to fine-tune is crisp: **"Fine-tuning is for form, RAG is for facts."** Fine-tuning is the right tool when the model exhibits behavioral problems — wrong output format, wrong tone, wrong task structure — not when it simply lacks knowledge. If the problem is missing or stale information, [[RAG]] is the correct intervention.

A key cautionary data point: when Bloomberg built [[BloombergGPT]], a domain-specific 50B-parameter model trained on 363B finance tokens, it was outperformed by GPT-4 on many financial tasks. General models with better training can outperform purpose-built domain models. This underscores the importance of exhausting prompt engineering and RAG before investing in fine-tuning.

The chapter's treatment of memory arithmetic is thorough. Training is far more memory-intensive than inference — during backpropagation, the system must store not just weights but also activations (for the backward pass), gradients, and optimizer states. The Adam optimizer stores two additional values per parameter (first and second moment estimates), tripling the per-parameter memory footprint beyond the weights alone. A 7B-parameter model in FP32 requires ~28 GB just for weights, but full fine-tuning demands multiples more for the full training state.

Parameter-efficient fine-tuning ([[LoRA]] and its variants) resolves this by updating only a small fraction of parameters. [[LoRA]] specifically decomposes the weight update matrix into two low-rank matrices: W' = W + (α/r)×AB, where A and B together hold perhaps 0.003% of the original parameter count. QLoRA extends this by storing the base model in 4-bit NF4 format, enabling 65B-parameter models to be fine-tuned on a single 48 GB GPU.

The chapter closes with [[Model Merging]] — an emerging technique for combining capabilities from multiple fine-tuned checkpoints without retraining. Methods range from linear interpolation (model soups, task arithmetic) to geometric interpolation (SLERP) to layer stacking (frankenmerging, MoE creation).

---

## Key Takeaways

- **When to fine-tune**: behavioral/form problems → fine-tune; knowledge/information problems → RAG. Fine-tuning cannot reliably inject new factual knowledge.
- **Fine-tuning types**: supervised fine-tuning (SFT), preference fine-tuning (RLHF/DPO), continued pre-training, infilling. Most production fine-tuning is SFT.
- **Memory bottleneck**: backpropagation requires storing activations for the entire forward pass. For training: weights + activations + gradients + optimizer states (Adam adds 2× per-parameter overhead).
- **Numerical representations**: FP32 (4B/param), FP16/BF16 (2B/param, BF16 preferred for training due to wider dynamic range), TF32 (19-bit hybrid), INT8, INT4. Mixed precision training typically uses FP16/BF16 for compute and FP32 for optimizer state.
- **Quantization**: Post-training quantization (PTQ) is the most common approach. QAT (quantization-aware training) is more accurate but costlier.
- **PEFT landscape**: Two families — adapter-based (LoRA dominant) and soft prompt-based (prefix-tuning, P-Tuning, prompt tuning).
- **LoRA**: rank r typically 4–64; for GPT-3 scale, LoRA uses 0.0027% of full fine-tuning parameters. At inference, adapters can be merged into base weights (no latency cost) or kept separate for multi-LoRA serving.
- **QLoRA**: 4-bit NF4 base model + LoRA adapters + paged optimizers → 65B model on one 48 GB GPU.
- **Model merging**: Combining multiple fine-tuned checkpoints (model soups, task vectors, SLERP, TIES/DARE for pruning, frankenmerging, MoE creation). Avoids catastrophic forgetting and enables capability combination.
- **Practical path**: Try prompt engineering first → RAG if knowledge gap → fine-tuning if behavior gap → full pre-training only if truly necessary.

---

## Notable Quotes

> "Finetuning is for form, RAG is for facts."

> "When Bloomberg built BloombergGPT, a 50B-parameter model trained on 363B tokens of financial data, it was still outperformed by GPT-4 on many financial tasks."

---

## Pages Created / Updated

**Created:**
- [[Finetuning]] — when to finetune, types, memory math, finetuning tactics
- [[LoRA]] — low-rank adaptation, QLoRA, multi-LoRA serving
- [[Model Merging]] — summing methods, layer stacking, concatenation
- [[Quantization]] — numerical representations, PTQ, QAT, mixed precision

**Updated:**
- [[Model Adaptation]] — added finetuning depth from Ch.7
