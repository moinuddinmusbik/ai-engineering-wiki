---
title: "AI Engineering — Ch.3: Evaluation Methodology"
type: source
created: 2026-04-10
updated: 2026-04-10
tags: [evaluation, metrics, ai-judge, perplexity, comparative-evaluation]
sources: [ai-engineering.pdf]
---

# AI Engineering — Ch.3: Evaluation Methodology

**Source:** *AI Engineering* by [[Chip Huyen]], O'Reilly 2024, Chapter 3 (pp. 137–180).

## Summary

Chapter 3 tackles the methodology of evaluating foundation models — a task that has become the biggest bottleneck to bringing AI applications to production. Huyen opens with a stark framing: without systematic evaluation, the risks of AI may outweigh its benefits, as illustrated by real-world failures (AI-encouraged suicide, hallucinated legal evidence, Air Canada chatbot damages).

The chapter begins with **language modeling metrics** — [[Perplexity]], cross entropy, bits-per-character (BPC), and bits-per-byte (BPB) — which remain the foundational measures for guiding training and finetuning. These four metrics are interconvertible and measure how well a model predicts its training data. Perplexity has useful secondary applications: detecting data contamination, identifying abnormal texts, and serving as a proxy for downstream capability.

The core of the chapter is a taxonomy of evaluation approaches split into **exact evaluation** and **subjective evaluation**. [[Exact Evaluation]] covers functional correctness (e.g., pass@k for code generation via HumanEval/MBPP) and similarity measurements against reference data — exact match, lexical similarity (BLEU, ROUGE), and semantic similarity via [[Embeddings]] and cosine distance. An important aside introduces embeddings in depth: BERT, CLIP, joint multimodal embedding spaces, and their role in semantic textual similarity metrics like BERTScore.

The chapter then pivots to [[AI as a Judge]] — the rising star of subjective evaluation. AI judges are fast, flexible, and relatively cheap; studies show GPT-4's agreement with humans (85%) exceeds inter-human agreement (81%). Huyen covers how to prompt AI judges (scoring systems, rubrics, examples), then catalogs their limitations: **inconsistency** (probabilistic outputs), **criteria ambiguity** (no standardization across tools), **increased costs and latency**, and **biases** (self-bias, first-position bias, verbosity bias). She surveys specialized judges: reward models (Cappy), reference-based judges (BLEURT, Prometheus), and preference models (PandaLM, JudgeLM).

The chapter closes with [[Comparative Evaluation]] — ranking models via head-to-head matchups rather than absolute scores. Inspired by game rating systems, this approach uses algorithms like Elo and Bradley–Terry. LMSYS's Chatbot Arena is the premier example. Huyen discusses scalability bottlenecks (quadratic model pairs), lack of standardization in crowdsourced comparisons, and the fundamental limitation that comparative evaluation reveals relative but not absolute performance.

## Key Takeaways

- **Evaluation is the biggest bottleneck** to AI adoption; investment in evaluation infrastructure lags far behind modeling and training.
- **Perplexity** is the default language modeling metric; lower = better prediction. Useful beyond training: data contamination detection, anomaly detection, deduplication.
- **Exact evaluation** (functional correctness + similarity measurements) produces unambiguous scores but requires reference data and works best for close-ended tasks.
- **BLEU/ROUGE** scores are declining in use since foundation models; higher lexical similarity doesn't always mean better responses (OpenAI found BLEU scores similar for correct and incorrect code).
- **Embeddings** are the backbone of semantic similarity; CLIP pioneered joint text–image embedding spaces.
- **AI as a judge** is the most common evaluation method in production (58% of evaluations on LangChain's platform). A judge = model + prompt; changing either changes the judge.
- **AI judge biases**: self-bias (GPT-4 favors itself +10%, Claude-v1 +25%), first-position bias, verbosity bias (preferring longer responses even with factual errors).
- **Comparative evaluation** captures human preference and is harder to game than benchmarks, but doesn't tell you if a model is *good enough*, only if it's *better*.

## Notable Quotes

> "Evals are surprisingly often all you need." — Greg Brockman, OpenAI (December 2023)

> "Do not trust any AI judge if you can't see the model and the prompt used for the judge."

> "A benchmark stops being useful as soon as it becomes public."

## Pages Created or Updated

- **Concepts:** [[Perplexity]], [[Exact Evaluation]], [[Embeddings]], [[AI as a Judge]], [[Comparative Evaluation]]
- **Entities:** [[LMSYS]]
- **Updated:** [[Hallucination]], [[Foundation Models]]
