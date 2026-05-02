---
title: "AI Engineering Stack"
type: concept
created: 2026-04-07
updated: 2026-04-07
tags: [architecture, stack, infrastructure]
sources: [ai-engineering.pdf]
---

# AI Engineering Stack

The three-layer architecture for building AI applications, as defined by [[Chip Huyen]].

## Layer 1: Application Development (Top)

The layer with the most activity since 2023. Responsibilities:

- **Evaluation** — Model selection, benchmarking, deployment readiness, production monitoring. Harder with open-ended outputs.
- **[[Prompt Engineering]]** — Getting models to express desired behaviors from input alone. Includes context construction and memory management.
- **AI Interface** — Building interfaces for end users: web apps, browser extensions, chatbot integrations, voice assistants, VSCode plugins.

## Layer 2: Model Development (Middle)

Traditionally the domain of [[ML Engineering]]. Responsibilities:

- **Modeling & Training** — Architecture selection, pre-training, [[Finetuning]], post-training. ML knowledge now "nice-to-have" rather than required.
- **Dataset Engineering** — Data curation, deduplication, tokenization, quality control. More important as models become commodities and data becomes the differentiator.
- **Inference Optimization** — Making models faster and cheaper. Even more critical with autoregressive models generating tokens sequentially (10ms/token = 1s for 100 tokens).

## Layer 3: Infrastructure (Bottom)

Least changed by the foundation model revolution. Responsibilities:

- Model serving
- Compute and resource management
- Monitoring and observability

## How They Changed with Foundation Models

| Layer | Traditional ML | AI Engineering |
|-------|---------------|----------------|
| Application | Less emphasis on interfaces | Interfaces are critical differentiators |
| Model | Train from scratch | Adapt existing models |
| Infrastructure | Same core needs | Scale requirements increased |

## The Build-or-Iterate Shift

- **Traditional ML**: Data → Model → Product (model-first)
- **AI Engineering**: Product → Model → Data (product-first, iterate fast)

Full-stack engineers who can quickly prototype have an advantage in this new paradigm.

## Sources

- [[AI Engineering — Ch.1: Introduction to Building AI Applications with Foundation Models|AI Engineering Ch.1]]
