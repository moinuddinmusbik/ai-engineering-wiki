---
title: "Dataset Engineering"
type: concept
created: 2026-04-27
updated: 2026-04-27
tags: [dataset-engineering, data-quality, data-curation, data-flywheel, annotation, llama3]
sources: [ai-engineering-ch8.md]
---

# Dataset Engineering

Dataset engineering is the discipline of designing, sourcing, curating, and validating training datasets for AI models. It reflects a **data-centric AI** philosophy: as models converge in architecture and training methodology, data quality and diversity become the primary differentiating factors in model performance.

> "Garbage in, garbage out" — applies at every training phase (pre-training, SFT, preference finetuning).

## Data-Centric vs Model-Centric AI

| Approach | Focus | When to Use |
|---|---|---|
| Model-centric | Improve architecture, training, hyperparameters | When data is plentiful and clean |
| Data-centric | Improve data quality, coverage, annotation | When model improvements plateau |

Modern AI engineering is predominantly data-centric: the major lever for improving GPT-4 to GPT-4-turbo, Llama 2 to Llama 3, etc., is better data, not better architecture.

## Data Quality Criteria

Six criteria define high-quality training data:

1. **Relevant**: matches the domain, task, and use case of the intended application
2. **Aligned**: reflects the intended model behavior (tone, format, values)
3. **Consistent**: no contradictions within or across examples
4. **Correctly formatted**: matches expected input/output schema
5. **Sufficiently unique**: no excessive duplication; near-duplicates inflate effective dataset size artificially
6. **Compliant**: licensing allows the intended use; privacy requirements met (no PII violations)

## Coverage and Diversity

Orthogonal to per-example quality. A dataset of high-quality examples can still be systematically biased if it:
- Covers only a subset of topic areas
- Represents only certain demographics or languages
- Contains only easy or only hard examples
- Has imbalanced class/task distribution

Coverage gaps become model gaps. For code models: if the training data has little Rust, the model will be poor at Rust regardless of overall quality.

## Llama 3 Data Mix

Illustrates how data composition shifts across training phases:

| Phase | General Knowledge | Domain-Specific | Notes |
|---|---|---|---|
| Pre-training | ~50% | ~50% | Broad coverage; massive scale |
| SFT | ~52.66% | ~47.34% | More balanced; quality over quantity |
| Preference finetuning | ~81.99% | ~18.01% | General alignment dominates |

The increasing share of general knowledge through alignment stages reflects the goal of making the model broadly helpful and harmless, not narrowly specialized.

## Data Acquisition

### Public Datasets
Common Crawl, Wikipedia, GitHub, Stack Exchange, arXiv, etc. Large scale but noisy; require significant filtering and deduplication. For SFT: FLAN, ShareGPT, OpenAssistant, and similar instruction-tuning datasets.

### Human Annotation
**Gold standard** for quality but expensive and slow. Annotation guidelines are the most important artifact — they define what "correct" means. Annotator agreement (inter-rater reliability) is a key quality metric.

### The Data Flywheel
**Application data is the ideal training source**: real user queries, model outputs, and implicit feedback (did the user accept the response? Did they retry?) are perfectly aligned with the deployment distribution. The **data flywheel** is the virtuous cycle:

```
Deploy application → Collect real user interactions → 
Label/filter best examples → Finetune model → Redeploy
```

Organizations that close this loop have a compounding competitive advantage. Every user interaction makes the model better, which attracts more users, which generates more data.

## Data Augmentation

Cheap techniques to expand dataset size and diversity, especially for small datasets:

- **Word replacement**: synonyms, antonyms, related terms (WordNet-based)
- **Perturbation**: add noise, typos, or paraphrasing to increase robustness
- **Back-translation**: translate to another language, then back (produces paraphrases)
- **Paraphrasing**: use a model to generate alternative phrasings of the same content

Augmentation works best when the target domain has limited data. Less useful when data is already abundant.

## Deduplication

Deduplication is one of the highest-impact preprocessing steps. Near-duplicate examples:
- Inflate apparent dataset size
- Cause the model to overfit on repeated patterns
- Distort frequency-based learning (memorization of duplicates vs. generalization)

Exact deduplication (hash-based) is fast. Fuzzy/semantic deduplication (MinHash, Jaccard similarity) catches near-duplicates. Llama 3 applied aggressive deduplication across their pre-training corpus.

## Connections

- [[Data Synthesis]] — generating training data programmatically or via AI models
- [[Finetuning]] — dataset engineering determines the quality of finetuning inputs
- [[Evaluation Pipeline]] — data quality assessment uses the same tools as model evaluation
- [[RAG]] — the retrieved documents in RAG are analogous to training data; quality matters similarly
