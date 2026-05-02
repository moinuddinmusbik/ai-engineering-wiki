---
title: "Finetuning"
type: concept
created: 2026-04-27
updated: 2026-04-27
tags: [finetuning, sft, rlhf, dpo, peft, transfer-learning, model-adaptation]
sources: [ai-engineering-ch7.md, ai-engineering-ch8.md]
---

# Finetuning

Finetuning is the third and most powerful [[Model Adaptation]] technique, after [[Prompt Engineering]] and [[RAG]]. It involves updating the weights of a pre-trained foundation model on a task-specific dataset to change the model's behavior, output style, or capability profile.

## The Core Decision: Fine-tune or Not?

The most important question before finetuning is **whether finetuning is actually the right tool**:

> **"Finetuning is for form, RAG is for facts."**

| Problem Type | Right Tool |
|---|---|
| Wrong output format | Fine-tune |
| Wrong tone or style | Fine-tune |
| Wrong task structure | Fine-tune |
| Missing / stale knowledge | [[RAG]] |
| Requires real-time data | [[RAG]] |

Finetuning cannot reliably inject new factual knowledge into a model. If a model gives wrong answers because it lacks information, finetuning on that information will not reliably fix it — the model may learn surface patterns without grounding the facts in its weights. [[RAG]] is the correct intervention for knowledge gaps.

**The progression path**: prompt engineering → RAG → finetuning → full pre-training (only if truly necessary). Each step is more expensive and should only be taken when the previous is insufficient.

**Cautionary example**: [[BloombergGPT]] — a 50B-parameter model trained on 363B financial tokens — was outperformed by GPT-4 on many financial tasks. General models with superior training often beat purpose-built domain models. Exhaust prompt engineering and RAG first.

## Types of Finetuning

### Supervised Finetuning (SFT)
The most common form. Train on (input, output) pairs representing the target behavior. The loss is cross-entropy on the output tokens. SFT teaches the model *what* to output.

### Preference Finetuning
Trains the model to prefer outputs ranked higher by human or AI feedback:
- **RLHF** (Reinforcement Learning from Human Feedback): reward model trained on human preferences; PPO used to optimize policy against reward model.
- **DPO** (Direct Preference Optimization): directly optimizes on (preferred, rejected) pairs without a separate reward model. Simpler and more stable than RLHF.

### Continued Pre-training
Further pre-training on domain-specific text in a self-supervised manner (next-token prediction). Used to add domain knowledge before SFT. Expensive but effective when the base model has poor domain coverage.

### Infilling / Fill-in-the-Middle
Training the model to complete text given surrounding context (not just a prefix). Essential for code completion tools like [[GitHub Copilot]].

## Memory Bottleneck

Training is far more memory-intensive than inference. During backpropagation:

| Component | Memory Cost |
|---|---|
| Model weights | N × M bytes (N params, M bytes/param) |
| Activations | Stored for entire forward pass (backward needs them) |
| Gradients | Same size as weights |
| Optimizer states | Adam: 2× weights (first + second moment estimates) |

**Total training memory ≈ 4× inference memory** (rough rule of thumb for Adam + FP16 mixed precision).

**Inference memory** (rough): N × M × 1.2 (weights + small overhead)
**Training memory**: weights + activations + gradients + optimizer states

For a 7B parameter model in FP32 (4 bytes/param):
- Weights: ~28 GB
- Full training state: ~112+ GB

This is why full finetuning of large models requires multi-GPU clusters.

## Numerical Representations

| Format | Bytes/param | Notes |
|---|---|---|
| FP32 | 4 | Full precision; standard for optimizer states |
| FP16 | 2 | Half precision; less dynamic range than BF16 |
| BF16 | 2 | Brain Float 16; same range as FP32, preferred for training |
| TF32 | ~2.4 (19-bit) | NVIDIA A100+; automatic mixed precision |
| INT8 | 1 | Integer quantization; inference-time mostly |
| INT4 | 0.5 | Aggressive quantization; used in QLoRA |

**Mixed precision training**: compute in FP16/BF16 for speed, maintain FP32 optimizer state for stability.

## Parameter-Efficient Fine-Tuning (PEFT)

PEFT techniques update only a small subset of parameters, making finetuning practical on consumer-grade hardware.

Two families:

### Adapter-Based
Add small trainable modules to a frozen base model. [[LoRA]] is the dominant approach — low-rank matrix decomposition of weight updates.

### Soft Prompt-Based
Add trainable "virtual tokens" to the input or hidden layers:
- **Prefix-tuning**: trainable vectors prepended to every layer's key-value pairs
- **P-Tuning**: trainable continuous prompt tokens in the input
- **Prompt tuning**: trainable soft tokens prepended to the input only

LoRA (adapter-based) consistently dominates in the community — per HuggingFace PEFT GitHub issues, it's by far the most-used method.

## Finetuning Tactics

**Hyperparameter guidance:**
- Learning rate: 1e-7 to 1e-3 (lower for small datasets; higher when adapting aggressively)
- Batch size: as large as memory allows; use gradient accumulation to simulate larger batches
- Gradient accumulation: simulate large batch by accumulating gradients over N steps before updating
- **Prompt loss masking**: when training on (instruction, response) pairs, often mask loss on the instruction portion (only compute loss on response tokens). Prevents the model from "wasting" capacity memorizing input formatting.

**Frameworks:** LLaMA-Factory, unsloth, PEFT (HuggingFace), Axolotl, LitGPT.

**Distillation path**: rather than finetuning on human-labeled data, use teacher model outputs. Alpaca finetuned a 7B model on 52K examples generated by GPT-3 (175B). See [[Data Synthesis]].

## Connections

- [[LoRA]] — the dominant PEFT technique; deep dive on low-rank adaptation
- [[Quantization]] — reduces memory for both inference and finetuning (QLoRA)
- [[Model Merging]] — combining finetuned checkpoints post-training
- [[Dataset Engineering]] — what data goes into finetuning
- [[Data Synthesis]] — generating training data via teacher models
- [[RAG]] — the alternative to finetuning for knowledge augmentation
- [[Model Adaptation]] — broader context: prompt engineering → RAG → finetuning
- [[Post-Training]] — finetuning as post-training from the model development perspective
