---
title: "Evaluation-Driven Development"
type: concept
created: 2026-04-10
updated: 2026-04-10
tags: [evaluation, methodology, development-process]
sources: [ai-engineering.pdf]
---

# Evaluation-Driven Development

Evaluation-driven development means **defining evaluation criteria before building** an AI application. The name is inspired by test-driven development (TDD) in software engineering.

## Rationale

Sensible business decisions are made based on returns on investment, not hype. Applications should demonstrate value to be deployed. The most common enterprise AI applications in production are those with **clear evaluation criteria**:

- **Recommender systems** — evaluated by engagement/purchase-through rates
- **Fraud detection** — evaluated by money saved from prevented frauds
- **Coding tools** — evaluated by functional correctness (unlike most generation tasks)
- **Classification tasks** (intent classification, sentiment analysis) — much easier to evaluate than open-ended tasks

## The Lamppost Problem

Focusing only on applications whose outcomes can be measured is like "looking for the lost key under the lamppost." Many potentially game-changing applications are missed because there's no easy way to evaluate them.

> "I believe that evaluation is the biggest bottleneck to AI adoption. Being able to build reliable evaluation pipelines will unlock many new applications." — [[Chip Huyen]]

## Evaluation Criteria Buckets

An AI application should start with a list of criteria specific to the application, organized into four buckets:

1. **Domain-specific capability** — Can the model do the required task? (e.g., understand legal contracts)
2. **Generation capability** — How coherent/faithful are the outputs? (see [[Factual Consistency]])
3. **Instruction-following capability** — Does the output match the requested format? (see [[Instruction Following]])
4. **Cost and latency** — How much does it cost and how long does it take?

## Connections

- Drives the structure of the [[Evaluation Pipeline]]
- [[Model Selection]] follows from well-defined evaluation criteria
- [[AI as a Judge]] and [[Exact Evaluation]] are the two main approaches for operationalizing criteria

## Sources

- [[AI Engineering — Ch.4: Evaluate AI Systems|AI Engineering Ch.4]]
