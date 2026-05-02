---
title: "Post-Training"
type: concept
created: 2026-04-07
updated: 2026-04-07
tags: [training, alignment, sft, rlhf, dpo]
sources: [ai-engineering.pdf]
---

# Post-Training

The process of training a model *after* pre-training to make it safe, useful, and conversational. Addresses two problems: (1) pre-trained models optimize for completion, not conversation; (2) models trained on internet data can output toxic/wrong content.

## The Shoggoth Metaphor

1. **Pre-training** → Untamed monster (capable but unsafe, trained on raw internet)
2. **[[Supervised Finetuning]]** → Socially acceptable (learns to converse)
3. **[[Preference Finetuning]]** → Smiley face (aligned with human values)

## Step 1: Supervised Finetuning (SFT)

Trains the model on (prompt, response) **demonstration data** to teach it *how to converse*.

- Without SFT, "How to make pizza" might produce follow-up questions instead of instructions — valid completion but wrong behavior.
- Requires diverse task coverage: QA, summarization, translation, etc.
- **Labelers must be highly skilled**: InstructGPT used 90% college-educated, 33% with master's. Each response costs ~$10–$25.
- InstructGPT: 13,000 demonstration pairs. Cost: ~$130,000+.
- Alternatives: volunteer data (LAION: 13,500 volunteers, 10,000 conversations, but skewed 90% male), heuristic filtering from internet (DeepMind/Gopher), AI-generated synthetic data.

## Step 2: Preference Finetuning

Teaches the model *what kind of conversations to have* — aligned with human preferences.

### RLHF (Reinforcement Learning from Human Feedback)
1. **Train a reward model** on comparison data: (prompt, winning_response, losing_response). Comparisons are easier and cheaper than absolute scoring ($3.50 vs $25 per sample).
2. **Optimize the foundation model** to maximize reward model scores (using PPO).

Inter-labeler agreement: ~73% (InstructGPT). Highlights the impossibility of capturing universal human preference.

### DPO (Direct Preference Optimization)
Simpler alternative to RLHF — skips the separate reward model. Meta switched from RLHF (Llama 2) to DPO (Llama 3) to reduce complexity.

### Best-of-N Strategy
Some companies (Stitch Fix, Grab) skip RL entirely: generate N outputs, score with reward model, pick the best. Simple and effective.

## Resource Allocation

Post-training uses only **~2% of total compute** (InstructGPT). Pre-training dominates at 98%. Post-training is about *unlocking* capabilities already latent in the pre-trained model.

> "Some people compare pre-training to reading to acquire knowledge, while post-training is like learning how to use that knowledge."

## Open Questions

- RLHF improved InstructGPT overall but actually *worsened* hallucination.
- More alignment training can lead to models expressing specific political/religious views ([[Inverse Scaling]]).
- If we had better pre-training data, we might not need SFT and preference finetuning at all.

## Sources

- [[AI Engineering — Ch.2: Understanding Foundation Models|AI Engineering Ch.2]]
