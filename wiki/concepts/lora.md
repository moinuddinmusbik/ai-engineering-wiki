---
title: "LoRA"
type: concept
created: 2026-04-27
updated: 2026-04-27
tags: [lora, qlora, peft, finetuning, low-rank, adapters]
sources: [ai-engineering-ch7.md]
---

# LoRA (Low-Rank Adaptation)

LoRA is the dominant parameter-efficient fine-tuning ([[PEFT|Finetuning]]) technique for large language models. It makes [[Finetuning]] of billion-parameter models practical on consumer-grade hardware by updating only a tiny fraction of parameters while preserving full model expressiveness.

## Core Idea

During standard finetuning, all weight matrices W in the model are updated. LoRA instead **freezes the original weights** and learns a low-rank decomposition of the weight update:

```
W' = W + ΔW
   = W + (α/r) × B × A
```

Where:
- **W**: original frozen weight matrix (n × m)
- **A**: low-rank matrix (n × r), initialized randomly
- **B**: low-rank matrix (r × m), initialized to zero (so ΔW starts at zero)
- **r**: rank hyperparameter (typically 4–64)
- **α**: scaling factor (often set equal to r)

Only A and B are updated during training. W is never modified.

## Why It Works

Any weight update ΔW can be expressed as a product of two matrices if ΔW is low-rank (or approximately low-rank). The hypothesis — empirically validated — is that the **weight updates required for task adaptation are intrinsically low-rank**, even though the full weight matrix is not.

## Parameter Efficiency

For GPT-3 (175B parameters), LoRA at rank 4 uses approximately **0.0027% of full finetuning parameters**. For a 7B-parameter model, typical LoRA configurations use ~20–40M trainable parameters versus 7B for full finetuning.

The parameter count per adapted layer:
- Full finetuning: n × m parameters
- LoRA: (n × r) + (r × m) = r × (n + m) parameters

For a 4096×4096 layer at rank 8: full = 16.7M, LoRA = 65K (0.39%).

## Rank Selection

| Rank (r) | Use Case |
|---|---|
| 4–8 | Simple style/format adaptation; very memory-constrained |
| 16–32 | General task adaptation; good balance |
| 64+ | Complex capability changes; more expressiveness |

Higher rank → more parameters → more expressiveness, but also more memory and risk of overfitting on small datasets.

## Which Layers to Adapt

LoRA is typically applied to:
- Attention weight matrices (Q, K, V projections)
- Output projection
- Optionally: MLP layers (feed-forward)

Adapting all attention matrices generally outperforms adapting only Q and V.

## Serving LoRA Adapters

Two production patterns:

### Merged Adapters (No Latency)
After training, merge A and B back into W:
```
W_merged = W + (α/r) × BA
```
The merged model has identical inference performance to the original. No architectural change, no latency cost. Downside: one merged model per task/persona.

### Separate Adapters (Multi-LoRA Serving)
Keep the base model frozen; load adapters dynamically at inference time. A single base model serves many customers/tasks, each with their own LoRA adapter. **Apple uses this on-device** — one base model, multiple task adapters loaded as needed. The adapter swap overhead is small (adapter is tiny compared to base model).

## QLoRA

QLoRA combines quantization with LoRA to push finetuning to extreme hardware constraints:

1. **Quantize base model to 4-bit NF4** (Normal Float 4): a data type optimized for normally-distributed weights. Not INT4 — NF4 has better numeric properties for neural network weights.
2. **Dequantize to BF16 during forward pass**: NF4 → BF16 on the fly for computation.
3. **Apply LoRA in BF16**: adapters trained in full 16-bit precision.
4. **Paged optimizers**: use NVIDIA unified memory to page optimizer states to CPU RAM when GPU memory is insufficient, preventing OOM errors during long sequences.

**Result**: A 65B-parameter model can be finetuned on a **single 48 GB GPU** (e.g., A6000 or A100). Without QLoRA, this would require ~8× 80 GB A100s.

## LoRA vs. Full Finetuning

| Aspect | Full Finetuning | LoRA |
|---|---|---|
| Memory | Very high (weights + gradients + optimizer) | Low (only A, B + their optimizer states) |
| Quality | Upper bound | ~Matches for most tasks; may lag on complex adaptation |
| Serving | One model | Merged (same) or separate adapters (multi-tenant) |
| Risk | Catastrophic forgetting | Lower (base weights preserved) |
| Speed | Slower (more parameters to update) | Faster (fewer gradients) |

## Connections

- [[Finetuning]] — LoRA is the dominant PEFT technique within the finetuning family
- [[Quantization]] — QLoRA combines quantization with LoRA for extreme efficiency
- [[Model Merging]] — merged LoRA adapters can be treated as model soups
- [[Inference Optimization]] — separate adapter serving is an inference-time concern
