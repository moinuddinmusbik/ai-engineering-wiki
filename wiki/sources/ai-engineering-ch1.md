---
title: "AI Engineering — Ch.1: Introduction to Building AI Applications with Foundation Models"
type: source
created: 2026-04-07
updated: 2026-04-07
tags: [ai-engineering, foundation-models, llm, overview, chip-huyen]
sources: [ai-engineering.pdf]
---

# Ch.1: Introduction to Building AI Applications with Foundation Models

**Book:** AI Engineering by [[Chip Huyen]] (O'Reilly, 2024)
**Pages:** 25–71

## Summary

This chapter frames the emergence of **[[AI Engineering]]** as a new discipline born from the convergence of three forces: increasingly capable [[Foundation Models]], massive investment in AI, and dramatically lower barriers to building AI applications via model-as-a-service APIs.

Huyen traces the evolution from early [[Language Models]] through [[Large Language Models]] (enabled by [[Self-Supervision]]) to multimodal [[Foundation Models]], and finally to the current era where engineers build applications *on top of* these models rather than training them from scratch.

The chapter surveys eight categories of AI use cases — [[Coding with AI]], image/video production, writing, education, [[Conversational Bots]], [[Information Aggregation]], data organization, and [[Workflow Automation]] — with data from 205 open-source GitHub projects and 50+ enterprise interviews.

It closes with a detailed comparison of the **[[AI Engineering Stack]]** (three layers: application development, model development, infrastructure) and how AI engineering differs from traditional [[ML Engineering]].

## Key Takeaways

- **Scale defines post-2020 AI.** Models are consuming nontrivial portions of global electricity and we're at risk of running out of public internet data to train them.
- **Self-supervision was the unlock.** Language models can infer labels from input data itself, removing the data-labeling bottleneck and enabling massive scale-up.
- **Foundation models are general-purpose.** Unlike task-specific models, a single foundation model can do sentiment analysis, translation, coding, and more. Adapt via [[Prompt Engineering]], [[RAG]], or [[Finetuning]].
- **The "last mile" problem is real.** Going 0→60 is easy (weekend demo), 60→100 is exceedingly hard. LinkedIn took 1 month for 80%, then 4 more months to reach 95%.
- **AI engineering ≠ ML engineering.** AI engineering is less about model training, more about *adapting and evaluating* models. Evaluation is harder because outputs are open-ended.
- **The AI stack has three layers:** Application Development (prompts, eval, interfaces), Model Development (training, datasets, inference optimization), Infrastructure (serving, compute, monitoring).
- **Full-stack engineers have an edge** in the new paradigm — the ability to quickly prototype and iterate is more valuable than deep ML knowledge for many use cases.
- **Maintenance is a moving target.** The AI space moves so fast that the best option today may be the worst tomorrow. Cost, regulation, and IP risks add complexity.

## Notable Data Points

- GitHub Copilot crossed $100M ARR within 2 years of launch.
- Midjourney hit $200M ARR at 1.5 years old (late 2023).
- Goldman Sachs estimated AI investment approaching $200B globally by 2025.
- McKinsey: AI makes developers 2x productive for documentation, 25–50% for code generation, minimal improvement for highly complex tasks.
- Eloundou et al. (2023): occupations with ~100% AI exposure include interpreters, tax preparers, web designers, writers.

## Notable Quotes

> "The journey from 0 to 60 is easy, whereas progressing from 60 to 100 becomes exceedingly challenging." — Ding et al. (UltraChat, 2023)

> "AI engineering is just software engineering with AI models thrown in the stack." — Anton Bacaj

## Connections

- Sets up the rest of the book: Ch.2 (foundation models), Ch.3–4 (evaluation), Ch.5 (prompt engineering), Ch.6 (RAG & agents), Ch.7 (finetuning), Ch.8 (datasets), Ch.9 (inference), Ch.10 (architecture & feedback).
- The [[Model Adaptation]] trichotomy (prompt engineering / RAG / finetuning) is the central organizing principle of the book.
