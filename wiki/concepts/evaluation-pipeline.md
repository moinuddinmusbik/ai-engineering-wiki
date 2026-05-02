---
title: "Evaluation Pipeline"
type: concept
created: 2026-04-10
updated: 2026-04-10
tags: [evaluation, pipeline, methodology, production]
sources: [ai-engineering.pdf]
---

# Evaluation Pipeline

An evaluation pipeline is a systematic, repeatable process for evaluating AI applications. The success of an AI application often hinges on the ability to differentiate good outcomes from bad — this requires a pipeline you can **rely upon**.

## Design Steps

### Step 1: Evaluate All Components

Real-world AI applications have multiple components. Evaluate **both** the end-to-end output and each component's intermediate output independently.

Example — extracting a person's employer from a resume PDF:
1. PDF → text extraction (evaluate with similarity to ground truth text)
2. Text → employer extraction (evaluate with accuracy given correct text)

If you only evaluate end-to-end, you can't pinpoint where failures occur.

### Multi-Turn Evaluation

Two levels for conversational applications:
- **Per-turn evaluation**: Quality of each individual output
- **Per-task evaluation**: Whether the system completes the full task, and in how many turns
  - Task-based evaluation is more important (what users care about) but harder (how do you define task boundaries?)
  - BIG-bench's `twenty_questions` benchmark is an example of task-based evaluation

### Step 2: Create Evaluation Guidelines

The most important step. An ambiguous guideline leads to ambiguous, misleading scores.

**Define criteria**: What makes a good response? LinkedIn found that defining this was their first hurdle in deploying generative AI. A "correct" response isn't always "good" (e.g., "You are a terrible fit" is correct but unhelpful for a job assessment tool).

LangChain's State of AI 2023: users average **2.3 criteria** per application (e.g., relevance + factual consistency + safety).

**Create scoring rubrics with examples**: For each criterion, define the scoring system (binary, 1–5, continuous) and provide examples at each level with justifications. Validate rubrics with humans — if humans find them hard to follow, refine until unambiguous.

### Step 3: Choose Evaluation Methods

Combine approaches from the evaluation methodology toolkit:
- [[Exact Evaluation]] for tasks with deterministic correctness
- [[AI as a Judge]] for subjective quality criteria
- [[Comparative Evaluation]] for relative ranking
- Human evaluation for sanity checks and ground truth

## Key Principles

1. **Don't trust public benchmarks alone** — they're likely contaminated and don't represent your application
2. **Custom benchmarks are essential** — curate prompts and metrics specific to your use case
3. **Evaluation guidelines can be reused** for training data annotation (Ch.8)
4. **The pipeline should be stable** — changing the evaluation method makes it hard to track application improvement over time

## Connections

- Operationalizes [[Evaluation-Driven Development]]
- Combines [[Exact Evaluation]], [[AI as a Judge]], and [[Comparative Evaluation]]
- Guides [[Model Selection]] (Step 3 of the 4-step workflow)
- Evaluation rubrics feed back into [[Post-Training]] data annotation
- Part of the broader [[AI Engineering Stack]]

## Sources

- [[AI Engineering — Ch.4: Evaluate AI Systems|AI Engineering Ch.4]]
