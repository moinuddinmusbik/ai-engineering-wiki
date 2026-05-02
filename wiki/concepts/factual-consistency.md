---
title: "Factual Consistency"
type: concept
created: 2026-04-10
updated: 2026-04-10
tags: [evaluation, hallucination, factuality, rag]
sources: [ai-engineering.pdf]
---

# Factual Consistency

Factual consistency measures whether a model's output is supported by facts — either from an explicitly provided context or from open knowledge. It is the most pressing generation quality metric and a primary concern for any application where [[Hallucination]] is unacceptable.

## Two Settings

### Local Factual Consistency
Output evaluated **against a provided context**. Factually consistent if supported by the context.

- Important for: summarization, customer support chatbots, business analysis, [[RAG]] systems
- Easier to verify because the context is explicit

### Global Factual Consistency
Output evaluated **against open knowledge**. Factually correct if aligned with commonly accepted facts.

- Important for: general chatbots, fact-checking, market research
- Much harder — requires determining what "the facts" are, navigating misinformation, and avoiding absence-of-evidence fallacies

## Evaluation Techniques

### AI as a Judge (Direct)
The most straightforward approach. Prompt an AI judge to evaluate factual consistency of a summary against its source document. Liu et al. (2023) and Luo et al. (2023) showed GPT-3.5/GPT-4 outperform previous methods. TruthfulQA's finetuned GPT-judge achieves 90–96% accuracy at predicting human truthfulness judgments.

### Self-Verification (SelfCheckGPT)
Manakul et al. (2023): Given response R, generate N additional responses. If the additional responses disagree with R, R is likely hallucinated. Expensive — requires many AI queries per evaluation.

### Knowledge-Augmented Verification (SAFE)
[[DeepMind]]'s Search-Augmented Factuality Evaluator (Wei et al., 2024):
1. Decompose response into individual statements
2. Make each statement self-contained
3. Generate fact-checking queries → send to Google Search
4. Use AI to verify each statement against search results

### Textual Entailment (NLI)
Frame factual consistency as a classification task:
- **Entailment** → factually consistent
- **Contradiction** → factually inconsistent
- **Neutral** → can't be determined

Specialized scorers (e.g., DeBERTa-v3-base-mnli-fever-anli, 184M parameters) trained on 764K annotated (hypothesis, premise) pairs.

## Benchmarks

- **TruthfulQA**: 817 questions across 38 categories (health, law, finance, politics) that humans commonly answer incorrectly due to false beliefs. Includes GPT-judge for automatic evaluation. Human expert baseline: 94%.

## Design Considerations

When designing factual consistency metrics:
- Analyze which query types the model is most likely to hallucinate on (niche knowledge, queries about things that don't exist)
- Focus benchmarks on these vulnerable areas
- For RAG systems, factual consistency against retrieved context is a crucial evaluation criterion

## Connections

- Directly addresses [[Hallucination]] — factual inconsistency is a primary manifestation
- Central evaluation criterion for [[RAG]] systems (Ch.6)
- [[AI as a Judge]] is the most common evaluation approach
- Part of the [[Evaluation-Driven Development]] criteria framework
- [[Safety Evaluation]] is technically a superset (factual inconsistency can cause harm)

## Sources

- [[AI Engineering — Ch.4: Evaluate AI Systems|AI Engineering Ch.4]]
