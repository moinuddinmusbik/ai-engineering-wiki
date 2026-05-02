---
title: "Foundation Models"
type: concept
created: 2026-04-07
updated: 2026-04-10
tags: [core-concept, llm, multimodal, models]
sources: [ai-engineering.pdf]
---

# Foundation Models

Large-scale, general-purpose AI models trained on massive datasets that serve as the *foundation* for building AI applications. The term encompasses both [[Large Language Models]] (text-only) and large multimodal models (text + images + video + audio).

## Evolution

```
Language Models (1950s–)
  → Large Language Models (2018–, via self-supervision)
    → Foundation Models (2021–, multimodal)
      → AI Engineering (building on top of them)
```

## Key Properties

- **General-purpose**: Unlike task-specific models, a single foundation model can handle translation, coding, summarization, reasoning, and more.
- **Scale**: Measured in parameters — from 117M (GPT-1, 2018) to 100B+ (current "large" threshold). Size is relative and shifts over time.
- **Multimodal**: Modern foundation models process text, images, video, audio, and more. GPT-4V and Claude 3 understand both images and text.
- **Self-supervised**: Trained without manual labels by inferring structure from the data itself (see [[Self-Supervision]]).
- **Adaptable**: Can be customized via [[Prompt Engineering]], [[RAG]], or [[Finetuning]].

## From Task-Specific to General-Purpose

Previously, AI research was siloed by modality: NLP (text), computer vision (images), speech (audio). Foundation models unify these under one architecture. A model trained with multimodal data generates the next token conditioned on both text and image tokens.

## How They're Built (Ch.2)

Four major design decisions:
1. **Training Data** — Quality, language coverage, domain coverage. See [[Self-Supervision]].
2. **Architecture** — Dominated by [[Transformer Architecture]]. Alternatives: [[State Space Models]], [[RWKV]], [[Jamba]].
3. **Scale** — Parameters × training tokens × FLOPs. Governed by [[Chinchilla Scaling Law]].
4. **[[Post-Training]]** — SFT + preference finetuning (RLHF/DPO) to align with human preferences.

## Key Models Mentioned

- **GPT series** ([[OpenAI]]): GPT-1 → GPT-2 → GPT-4 → GPT-4V
- **CLIP** ([[OpenAI]]): Language-image embedding model, trained on 400M image-text pairs via natural language supervision
- **BERT**: Masked language model by Google (2018) — bidirectional, used for classification tasks
- **Llama series** ([[Meta]]): Open-weight, 7B–405B params, prioritized usability
- **Gemini** (Google), **Claude** ([[Anthropic]]), **Mixtral** (Mistral), **Mamba** (SSM-based)
- **AlphaFold** ([[DeepMind]]): Domain-specific, protein structure prediction

## Evaluation Challenges (Ch.3–4)

Evaluating foundation models is uniquely difficult compared to traditional ML models:

1. **The smarter the model, the harder to evaluate** — few people can verify PhD-level math solutions
2. **Open-ended outputs** undermine ground-truth-based evaluation — too many valid responses to enumerate
3. **Black-box nature** — model internals (architecture, training data) are often hidden
4. **Benchmark saturation** — GLUE saturated in one year; MMLU largely replaced by MMLU-Pro within four years
5. **Expanded evaluation scope** — must discover new capabilities, not just measure known ones

Key evaluation approaches: [[Perplexity]] (language modeling proxy), [[Exact Evaluation]] (functional correctness, similarity), [[AI as a Judge]] (subjective quality), and [[Comparative Evaluation]] (relative ranking via Chatbot Arena).

[[Model Selection]] between foundation models spans hard attributes (licenses, privacy) and soft attributes (accuracy, toxicity), with the build-vs-buy decision adding further complexity.

## Sources

- [[AI Engineering — Ch.1: Introduction to Building AI Applications with Foundation Models|AI Engineering Ch.1]]
- [[AI Engineering — Ch.2: Understanding Foundation Models|AI Engineering Ch.2]]
- [[AI Engineering — Ch.3: Evaluation Methodology|AI Engineering Ch.3]]
- [[AI Engineering — Ch.4: Evaluate AI Systems|AI Engineering Ch.4]]
