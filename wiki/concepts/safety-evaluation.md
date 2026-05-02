---
title: "Safety Evaluation"
type: concept
created: 2026-04-10
updated: 2026-04-10
tags: [evaluation, safety, toxicity, bias]
sources: [ai-engineering.pdf]
---

# Safety Evaluation

Safety evaluation measures whether a model's outputs can cause harm to users and society. Safety is an umbrella term covering all types of toxicity and biases.

## Harm Categories

1. **Inappropriate language** — profanity, explicit content
2. **Harmful recommendations** — tutorials for dangerous activities, encouraging self-destructive behavior
3. **Hate speech** — racist, sexist, homophobic, and other discriminatory language
4. **Violence** — threats, graphic detail
5. **Stereotypes** — e.g., always using female names for nurses, male names for CEOs
6. **Political/religious bias** — models generating content supporting only one ideology

## Political Biases in Models

Studies (Feng et al., 2023; Motoki et al., 2023; Hartman et al., 2023) have shown measurable political biases:
- [[OpenAI]]'s GPT-4: more left-wing and libertarian-leaning
- [[Meta]]'s Llama: more authoritarian
- These biases are a function of training data and post-training alignment choices

## Evaluation Approaches

### General-Purpose AI Judges
GPT, Claude, and Gemini can detect many harmful outputs when prompted properly. Model providers also expose their moderation tools externally ([[Anthropic]] has a tutorial on using Claude for content moderation).

### Specialized Toxicity Classifiers
Much smaller, faster, and cheaper than general-purpose judges:
- Facebook's hate speech detection model
- Skolkovo Institute's toxicity classifier
- Perspective API
- Language-specific models (Danish, Vietnamese, etc.)
- [[Meta]]'s Llama Guard — safety-focused model

### Moderation Endpoints
[[OpenAI]]'s content moderation endpoint provides a taxonomy and automated detection.

## Benchmarks

- **RealToxicityPrompts** (Gehman et al., 2020): 100,000 naturally occurring prompts likely to elicit toxic outputs
- **BOLD** (Bias in Open-ended Language Generation Dataset) (Dhamala et al., 2021): Measures bias in generation

## Connections

- [[Factual Consistency]] is technically a subset (factual inconsistency can cause harm)
- [[Post-Training]] (RLHF/DPO) is the primary mechanism for making models safer
- [[AI as a Judge]] can evaluate safety in production as a guardrail
- Part of the [[Evaluation-Driven Development]] criteria framework
- Ch.5 discusses more ways AI can be unsafe and how to make systems more robust

## Sources

- [[AI Engineering — Ch.4: Evaluate AI Systems|AI Engineering Ch.4]]
