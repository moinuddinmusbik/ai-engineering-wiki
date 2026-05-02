---
title: "AI Engineering"
type: concept
created: 2026-04-07
updated: 2026-04-07
tags: [discipline, core-concept, foundation-models]
sources: [ai-engineering.pdf]
---

# AI Engineering

The process of building applications on top of readily available [[Foundation Models]], as opposed to training models from scratch.

## Definition

AI engineering emerged as a distinct discipline from [[ML Engineering]] due to three converging factors:

1. **General-purpose AI capabilities** — Foundation models can do more tasks, enabling more applications.
2. **Increased AI investment** — ChatGPT's success triggered massive capital flow (~$200B globally by 2025).
3. **Low entry barrier** — Model-as-a-service APIs let anyone build AI apps without ML infrastructure.

## Key Differences from ML Engineering

| Aspect | ML Engineering | AI Engineering |
|--------|---------------|----------------|
| Focus | Model training | Model adaptation & evaluation |
| Starting point | Data → Model → Product | Product → Model → Data |
| Knowledge required | Deep ML expertise | ML nice-to-have, product skills critical |
| Outputs | Close-ended (classification) | Open-ended (generation) |
| Evaluation | Compare against ground truth | Much harder — open-ended outputs |

## Three Model Adaptation Techniques

1. **[[Prompt Engineering]]** — Adapt via instructions and context, no weight changes. Easiest to start.
2. **[[RAG]]** — Supplement prompts with retrieved external knowledge.
3. **[[Finetuning]]** — Update model weights. Most powerful, most resource-intensive.

## The AI Engineering Stack

Three layers (see [[AI Engineering Stack]]):
- **Application Development**: prompts, evaluation, AI interfaces
- **Model Development**: training, dataset engineering, inference optimization
- **Infrastructure**: serving, compute management, monitoring

## Connections

- The discipline this entire wiki is organized around.
- Evolved from [[ML Engineering]], now closer to full-stack development.
- [[Chip Huyen]] defines and maps this field in her book.

## Sources

- [[AI Engineering — Ch.1: Introduction to Building AI Applications with Foundation Models|AI Engineering Ch.1]]
