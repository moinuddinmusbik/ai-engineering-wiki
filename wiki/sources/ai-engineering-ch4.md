---
title: "AI Engineering — Ch.4: Evaluate AI Systems"
type: source
created: 2026-04-10
updated: 2026-04-10
tags: [evaluation, model-selection, benchmarks, factual-consistency, safety, instruction-following]
sources: [ai-engineering.pdf]
---

# AI Engineering — Ch.4: Evaluate AI Systems

**Source:** *AI Engineering* by [[Chip Huyen]], O'Reilly 2024, Chapter 4 (pp. 183–232).

## Summary

Where Chapter 3 covered evaluation *methods*, Chapter 4 covers how to use those methods to evaluate models *for your application*. It is structured in three parts: evaluation criteria, model selection, and evaluation pipeline design.

### Part 1: Evaluation Criteria

Huyen introduces [[Evaluation-Driven Development]] — defining evaluation criteria before building, inspired by test-driven development. She identifies four buckets of criteria:

1. **Domain-specific capability** — evaluated via MCQs and benchmarks (MMLU, HumanEval, BIRD-SQL). MCQs are popular because they're easy to create and verify, but they test classification ability (differentiating good from bad) rather than generation ability.

2. **Generation capability** — the chapter focuses on [[Factual Consistency]], the most pressing issue. Local factual consistency checks outputs against provided context; global factual consistency checks against open knowledge. Techniques include AI as a judge, self-verification (SelfCheckGPT), knowledge-augmented verification (SAFE by [[DeepMind]]), and textual entailment (NLI). TruthfulQA is the key benchmark (817 questions across 38 categories).

3. **[[Safety Evaluation]]** — covers six harm categories: inappropriate language, harmful recommendations, hate speech, violence, stereotypes, and political/religious bias. Tools range from general-purpose AI judges to specialized toxicity classifiers (Perspective API, Meta's Llama Guard). Studies show models have measurable political biases (GPT-4 leans left-libertarian; Llama leans authoritarian). Key benchmarks: RealToxicityPrompts, BOLD.

4. **[[Instruction Following]]** — IFEval identifies 25 automatically verifiable instruction types (keyword inclusion, length constraints, JSON format, etc.). INFOBench takes a broader view, including content constraints, linguistic guidelines, and style rules, verified via yes/no decomposition questions. Roleplaying evaluation (RoleLLM, CharacterEval) is also covered as a common real-world instruction type.

5. **Cost and latency** — metrics include time to first token, time per token, time per query. Pareto optimization across quality, latency, and cost is essential.

### Part 2: Model Selection

The [[Model Selection]] workflow has four steps: (1) filter by hard attributes, (2) narrow via public benchmarks, (3) run private experiments, (4) monitor in production. Hard attributes (licenses, data privacy, model size) vs. soft attributes (accuracy, toxicity — improvable via adaptation).

The **build vs. buy** analysis is thorough, covering seven axes: data privacy (Samsung ChatGPT leak), data lineage/copyright (evolving IP laws), performance (gap closing but incentives favor proprietary models keeping their best behind paywalls), functionality (logprobs, finetuning, structured outputs), API cost vs. engineering cost, control/transparency (model versioning risks, rate limits), and on-device deployment.

Open source terminology is clarified: **open weight** (weights public, data hidden) vs. **open model** (weights + data public). Licensing is complex — Llama licenses restrict >700M MAU and distillation; Apache 2.0 is the most permissive (Gemma, Mistral-7B).

### Part 3: Evaluation Pipeline Design

The chapter closes with practical pipeline design: evaluate all components independently (not just end-to-end), create clear evaluation guidelines with scoring rubrics, and use both per-turn and per-task evaluation. Benchmarks become saturated rapidly (GLUE → SuperGLUE → MMLU → MMLU-Pro) and are prone to data contamination. Detection methods include n-gram overlapping and perplexity analysis.

Public leaderboards ([[Hugging Face]] Open LLM Leaderboard, Stanford HELM, BIG-bench) are useful for initial filtering but can't replace custom evaluation. Benchmark correlation matters — strongly correlated benchmarks exaggerate biases. Stanford spent ~$80K–$100K to evaluate 30 models on full HELM.

## Key Takeaways

- **Evaluation-driven development**: define how you'll evaluate before you build. Evaluation is the biggest bottleneck to AI adoption.
- **Factual consistency** has two settings: local (against context) and global (against open knowledge). SelfCheckGPT and SAFE represent self-verification and knowledge-augmented approaches respectively.
- **Safety** encompasses six categories; models have measurable political biases that vary by provider.
- **IFEval** provides 25 automatically verifiable instruction types — a practical template for custom instruction-following benchmarks.
- **Model selection** should distinguish hard attributes (deal-breakers) from soft attributes (improvable). The build-vs-buy decision spans seven axes.
- **Open source ≠ open model**: most "open source" models are open weight only, with hidden training data.
- **Data contamination** is pervasive and often unintentional; public benchmarks should always be supplemented with private evaluation.
- **Benchmark saturation** is accelerating — benchmarks are replaced within 1–2 years.

## Notable Quotes

> "I believe that evaluation is the biggest bottleneck to AI adoption. Being able to build reliable evaluation pipelines will unlock many new applications."

> "An application that is deployed but can't be evaluated is worse [than one never deployed]. It costs to maintain, but if you want to take it down, it might cost even more."

## Pages Created or Updated

- **Concepts:** [[Evaluation-Driven Development]], [[Factual Consistency]], [[Safety Evaluation]], [[Instruction Following]], [[Model Selection]], [[Evaluation Pipeline]]
- **Entities:** [[Hugging Face]]
- **Updated:** [[Hallucination]], [[Foundation Models]], [[DeepMind]]
