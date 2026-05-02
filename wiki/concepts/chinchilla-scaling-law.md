---
title: "Chinchilla Scaling Law"
type: concept
created: 2026-04-07
updated: 2026-04-07
tags: [scaling, training, compute, model-size]
sources: [ai-engineering.pdf]
---

# Chinchilla Scaling Law

A rule for building compute-optimal models, proposed in "Training Compute-Optimal Large Language Models" (DeepMind, 2022). Given a fixed compute budget, it determines the optimal model size and dataset size.

## The Rule

> For compute-optimal training, the number of training tokens should be approximately **20x the number of model parameters**. Model size and training tokens should be scaled equally — doubling one means doubling the other.

Example: A 3B-parameter model needs ~60B training tokens.

## How It Was Derived

DeepMind trained **400 language models** ranging from 70M to 16B parameters on 5 to 500 billion tokens. They mapped the relationships between model size, dataset size, compute budget, and training loss.

## Key Implications

- Many early models were **undertrained** relative to their size. GPT-3-175B was trained on only 300B tokens (1.7x params, not 20x).
- **Chinchilla** (70B params, 1.4T tokens) outperformed the much larger Gopher (280B params, 300B tokens) because it was trained on more data per parameter.
- Llama models deliberately chose **smaller than optimal** sizes for better usability and cheaper inference — a practical tradeoff.
- The law assumes data acquisition cost is much cheaper than compute cost. A modified calculation exists for when data is expensive.

## Limitations

- Developed for **dense models** — adapting for MoE/sparse models is active research.
- Doesn't account for synthetic/AI-generated training data.
- Optimizes for *training* compute, not *inference* compute. Sardana et al. (2023) modified it to account for inference demand.

## Three Numbers That Signal Scale

1. **Parameters** — proxy for learning capacity
2. **Training tokens** — proxy for how much was learned
3. **FLOPs** — proxy for training cost

## Sources

- [[AI Engineering — Ch.2: Understanding Foundation Models|AI Engineering Ch.2]]
