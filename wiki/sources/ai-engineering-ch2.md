---
title: "AI Engineering — Ch.2: Understanding Foundation Models"
type: source
created: 2026-04-07
updated: 2026-04-07
tags: [ai-engineering, foundation-models, transformer, training, sampling, chip-huyen]
sources: [ai-engineering.pdf]
---

# Ch.2: Understanding Foundation Models

**Book:** AI Engineering by [[Chip Huyen]] (O'Reilly, 2024)
**Pages:** 73–135

## Summary

This chapter dives into how [[Foundation Models]] are built, covering the four major design decisions: training data, model architecture, model size, and post-training alignment. It also covers [[Sampling]], an underrated concept that explains many AI behaviors including hallucination and inconsistency.

The chapter is structured as: Training Data → Modeling (architecture + size) → Post-Training (SFT + preference finetuning) → Sampling (strategies + structured outputs + probabilistic nature).

## Key Takeaways

### Training Data
- **Data quality matters as much as quantity.** Common Crawl is used in most models but contains fake news, propaganda, and low-quality content. A 1.3B model trained on 7B tokens of high-quality coding data outperformed much larger models.
- **English dominance:** 45.88% of Common Crawl is English, 8x more than Russian (2nd place). Languages like Punjabi (231x underrepresented), Swahili (115x), and Bengali (37x) are severely underrepresented.
- **Multilingual performance gap:** GPT-4 performs 3x better on math in English vs Armenian/Farsi. Fails completely in Burmese and Amharic. Burmese requires 10x more tokens than English for the same content.
- **Domain-specific models** (AlphaFold, BioNeMo, Med-PaLM2) outperform general models on specialized tasks. Data for these domains is expensive and scarce.
- **Data is running out.** Training dataset growth rate exceeds new data creation rate. 28% of C4 sources are now restricted. Proprietary data becomes competitive advantage.

### Model Architecture
- **[[Transformer Architecture]]** dominates since 2017, solving seq2seq's two problems: information bottleneck (decoder only sees final hidden state) and sequential processing (slow for long inputs).
- **[[Attention Mechanism]]** is the key innovation — lets models weigh importance of different input tokens via Key/Query/Value vectors. Multi-headed attention enables attending to different token groups simultaneously.
- **Transformer block** = Attention module + MLP module. Number of blocks = number of layers. Model size determined by: model dimension, number of blocks, feedforward dimension, vocabulary size.
- **Prefill vs Decode**: Input tokens processed in parallel (prefill), output tokens generated sequentially (decode). This asymmetry drives many optimization techniques.
- **Alternative architectures gaining traction:** [[RWKV]] (parallelizable RNN), [[State Space Models]] (Mamba — linear-time scaling, million-length sequences), [[Jamba]] (hybrid transformer-Mamba, 52B params on single 80GB GPU).

### Model Size & Scaling
- **Three numbers signal scale:** number of parameters (learning capacity), training tokens (how much learned), FLOPs (training cost).
- **[[Chinchilla Scaling Law]]**: Optimal training tokens ≈ 20x model parameters. Double model → double data. Proposed by DeepMind (2022) from training 400 models (70M–16B params).
- **Compute cost example:** Training GPT-3-175B on 256 H100s at 70% utilization costs ~$4.1M.
- **Mixture-of-Experts (MoE):** Sparse models where only a subset of parameters is active per token. Mixtral 8x7B has 46.7B total params but only 12.9B active per token.
- **[[Inverse Scaling]]:** Larger/more-aligned models can sometimes perform *worse* (e.g., expressing specific political/religious views). Inverse Scaling Prize found failures in memorization and strong-prior tasks.
- **Scaling bottlenecks:** Running out of internet data (training data growth > new data creation). Electricity — data centers at 1–2% global consumption, projected 4–20% by 2030.

### Post-Training
- **Two-step process:** [[Supervised Finetuning]] (SFT) → [[Preference Finetuning]] (RLHF/DPO)
- **SFT** teaches the model to *converse* (not just complete). Uses (prompt, response) demonstration data. InstructGPT: 13,000 pairs at ~$10 each = $130,000. Labelers are highly educated (~90% college degree, 33% master's).
- **Preference finetuning** teaches the model *what kind of conversations to have*. Uses comparison data: (prompt, winning_response, losing_response). RLHF trains a reward model, then optimizes the foundation model against it. DPO is simpler (Meta switched from RLHF to DPO for Llama 3).
- **The Shoggoth metaphor:** Pre-training = untamed monster, SFT = socially acceptable, preference finetuning = smiley face.
- Post-training uses only ~2% of compute (InstructGPT), but unlocks capabilities already latent in the pre-trained model.
- **"Best of N" strategy:** Some companies (Stitch Fix, Grab) skip RL entirely — generate multiple outputs, pick the highest-scored by reward model.

### Sampling
- **Fundamentals:** Model outputs logit vector → softmax → probability distribution → sample next token.
- **[[Temperature]]**: Higher = more creative/diverse (redistributes probability to rare tokens). Lower = more consistent/predictable. 0 = greedy (always pick highest logit). 0.7 common for creative tasks.
- **[[Top-k Sampling]]**: Only consider top-k logits. Reduces computation. k typically 50–500.
- **[[Top-p Sampling]]** (nucleus): Dynamic selection — sum probabilities until reaching p (typically 0.9–0.95). More contextually appropriate than top-k.
- **[[Test Time Compute]]**: Generate multiple outputs, pick the best. Using a verifier gives the same boost as 30x model size increase. Optimal up to ~400 samples (OpenAI), log-linear improvement up to 10,000 (Stanford).
- **[[Structured Outputs]]**: Five approaches — prompting, post-processing, test time compute, constrained sampling (filter invalid logits), finetuning. LinkedIn's defensive YAML parser: 90% → 99.99%.

### The Probabilistic Nature
- **[[Inconsistency]]**: Same input → different outputs. Slightly different input → drastically different outputs. Even fixing temperature/seed/top-p doesn't guarantee 100% consistency (hardware differences matter).
- **[[Hallucination]]**: Two hypotheses:
  1. **Self-delusion** (DeepMind, Ortega 2021): Model can't distinguish its own outputs from given facts. Snowballing — initial wrong assumption leads to more hallucinations.
  2. **Knowledge mismatch** (Leo Gao, OpenAI): SFT trains model to mimic labeler responses using knowledge the model doesn't have → teaches model to hallucinate.
- RLHF results are mixed: helped overall but actually worsened hallucination for InstructGPT.

## Notable Data Points

- Llama training data: 1.4T tokens (v1) → 2T (v2) → 15T (v3)
- RedPajama-v2: 30 trillion tokens (≈ 450M books)
- InstructGPT: 98% compute for pre-training, 2% for post-training
- Comparison labeling: $3.50 per comparison (Llama 2), $25 per written response
- RLHF labeler agreement: ~73% (InstructGPT)
- Llama 2-7B: 32 transformer blocks, 4096 model dim, 32K vocab, 4K context
- Llama 3-405B: 126 transformer blocks, 16384 model dim, 128K vocab, 128K context

## Connections

- Deepens [[Foundation Models]] and [[Language Models]] from Ch.1
- Training data quality connects to [[Model Adaptation]] — garbage in, garbage out
- Post-training section is foundation for Ch.7 ([[Finetuning]])
- Sampling section is foundation for Ch.5 ([[Prompt Engineering]]) and Ch.9 (Inference Optimization)
- Scaling law connects to the [[AI Engineering Stack]] cost considerations
