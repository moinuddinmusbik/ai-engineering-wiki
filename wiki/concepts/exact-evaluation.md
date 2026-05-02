---
title: "Exact Evaluation"
type: concept
created: 2026-04-10
updated: 2026-04-10
tags: [evaluation, metrics, similarity, functional-correctness]
sources: [ai-engineering.pdf]
---

# Exact Evaluation

Exact evaluation produces judgment without ambiguity — the score is deterministic and reproducible. It contrasts with subjective evaluation (e.g., [[AI as a Judge]]), where scores depend on the evaluator.

## Two Approaches

### 1. Functional Correctness

Evaluating whether a system performs its intended functionality. The ultimate metric for any application, but not always straightforward to measure.

**Code generation** is the flagship use case — code is validated by running unit tests:
- **pass@k**: Generate k code samples; a problem is "solved" if any sample passes all test cases. The fraction of solved problems is the score.
- Benchmarks: OpenAI's HumanEval, Google's MBPP, text-to-SQL benchmarks (Spider, BIRD-SQL, WikiSQL)
- BIRD-SQL also evaluates efficiency (runtime comparison with ground truth)

Also applicable to game bots (measured by score) and optimization tasks (measured by objective improvement).

### 2. Similarity Measurements Against Reference Data

When functional correctness can't be automated, compare AI outputs against reference (ground truth) responses. Four methods, from strictest to most flexible:

#### Exact Match
- Binary: does the output match a reference exactly?
- Works for short, precise answers ("What's 2+3?" → "5")
- Variations: accept any output *containing* the reference (but can cause false positives)

#### Lexical Similarity
- Measures token overlap between generated and reference texts
- **N-gram similarity**: Compare overlapping sequences of n tokens
- **Fuzzy matching**: Edit distance (deletion, insertion, substitution)
- Common metrics: **BLEU**, **ROUGE**, METEOR++, TER, CIDEr
- Declining in use since foundation models — OpenAI found BLEU scores similar for correct and incorrect code on HumanEval
- Requires comprehensive reference sets; correct answers missing from references cause false negatives (Adept's Fuyu example)

#### Semantic Similarity
- Measures whether texts have the same *meaning*, not just the same words
- Requires converting texts to [[Embeddings]], then computing **cosine similarity**
- "What's up?" and "How are you?" are lexically different but semantically close
- Metrics: BERTScore, MoverScore
- Less dependent on comprehensive reference sets than lexical similarity
- Quality depends on the embedding algorithm; compute-intensive

## Key Insight

> Similarity measurements are useful far beyond evaluation: retrieval/search, ranking, clustering, anomaly detection, and data deduplication. These techniques recur throughout the AI engineering stack.

## Connections

- [[Perplexity]] is exact but measures language modeling performance, not task performance
- [[AI as a Judge]] is subjective — useful when exact evaluation can't capture quality
- [[Embeddings]] are the foundation of semantic similarity
- [[Evaluation Pipeline]] combines exact and subjective methods

## Sources

- [[AI Engineering — Ch.3: Evaluation Methodology|AI Engineering Ch.3]]
