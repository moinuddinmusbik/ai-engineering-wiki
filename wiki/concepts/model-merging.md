---
title: "Model Merging"
type: concept
created: 2026-04-27
updated: 2026-04-27
tags: [model-merging, model-soups, task-vectors, frankenmerging, moe, slerp, ties, dare]
sources: [ai-engineering-ch7.md]
---

# Model Merging

Model merging is the practice of combining multiple separately-trained model checkpoints into a single model that inherits capabilities from all of them — without any additional training. It has emerged as a practical alternative to multi-task training and an antidote to catastrophic forgetting.

The intuition: if two models are fine-tuned from the same base checkpoint, their weight spaces occupy nearby regions. Interpolating between them yields a model that often performs better than either on both tasks.

## Why Merge?

- **Combine capabilities**: a model finetuned for coding + a model finetuned for reasoning → a model good at both
- **Avoid catastrophic forgetting**: sequential finetuning destroys previous task performance; merging preserves it
- **No retraining required**: merging is a post-hoc operation on existing checkpoints
- **Community recipes**: the open-source community (Hugging Face, MergeKit) has developed numerous successful merge recipes

## Method Families

### 1. Summing (Weight Interpolation)

**Model Soups (Linear Combination)**
Average or linearly interpolate the weights of two or more checkpoints:
```
W_merged = λ₁W₁ + λ₂W₂ + ... + λₙWₙ
```
Works best when models share the same base and architecture. Simple but surprisingly effective.

**Task Vectors / Task Arithmetic**
A task vector is the difference between a finetuned model and its base:
```
τ = W_finetuned - W_base
```
Task vectors can be added or subtracted:
- Add two task vectors to combine capabilities
- Subtract a task vector to remove a capability (e.g., subtract "toxic content" ability)
- Scale task vectors (larger coefficient = stronger influence)

**SLERP (Spherical Linear Interpolation)**
Interpolates along the geodesic (shortest path) on a hypersphere rather than in Euclidean space. Better preserves vector norms during interpolation. Often used for merging two models (not scalable to many models):
```
W = sin((1-t)θ)/sin(θ) × W₁ + sin(tθ)/sin(θ) × W₂
```
where θ is the angle between W₁ and W₂, t ∈ [0,1].

**TIES (Trim, Elect Sign, Merge)**
Handles conflict when merging many task vectors:
1. **Trim**: prune small-magnitude changes in each task vector (keep top-k% by magnitude)
2. **Elect sign**: for each parameter, determine the dominant sign direction across task vectors
3. **Merge**: average only the task vectors that agree with the elected sign

**DARE (Drop And REscale)**
Randomly drop (zero out) a fraction of task vector parameters, then rescale the remaining ones. Reduces interference between task vectors. Often combined with TIES.

### 2. Layer Stacking

**Frankenmerging**
Take layers from different finetuned models and stack them directly into a new model. Example: take the first 16 layers from Model A and the last 16 layers from Model B. The resulting model has an unusual capability mix that sometimes produces surprising emergent behaviors.

**MoE Creation (Mixture of Experts)**
Convert multiple dense finetuned models into a sparse Mixture of Experts architecture. Each expert is a full finetuned model; a router determines which expert(s) handle each token. Combines capabilities without degrading individual model performance (each expert is preserved intact).

**Depthwise Scaling / SOLAR**
Stack a model's own layers to create a deeper model. SOLAR 10.7B was created by duplicating and stacking layers of a base model, then finetuning the result. The resulting model outperforms the original despite having the same parameter count (just deeper).

### 3. Concatenation

Concatenate weight matrices dimension-wise. More niche; used in specific architectural experiments. Increases model size.

## Practical Considerations

- Merging works best when models **share the same base** (same architecture, same initialization). Merging two independently pre-trained models is much harder.
- **Merging ≠ ensemble**: a merged model runs a single forward pass; an ensemble runs N forward passes. Merged models are much cheaper at inference.
- **MergeKit** (Hugging Face) is the standard open-source toolkit for model merging experiments.
- Quality is not guaranteed — the merged model may be worse than either constituent. Evaluation is essential after merging.
- Merging is particularly popular in the **open-source community** where compute is scarce: "why train when you can merge?"

## Connections

- [[Finetuning]] — model merging is applied to finetuned checkpoints
- [[LoRA]] — merged LoRA adapters are a special case of model summing
- [[Model Adaptation]] — merging is an emerging fourth adaptation strategy
- [[Inference Optimization]] — MoE creation affects inference architecture
