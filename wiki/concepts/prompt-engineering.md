---
title: "Prompt Engineering"
type: concept
created: 2026-04-15
updated: 2026-04-15
tags: [adaptation, prompting, in-context-learning, best-practices]
sources: [ai-engineering.pdf]
---

# Prompt Engineering

The process of crafting instructions to get a model to generate desired outcomes. The easiest and most common [[Model Adaptation]] technique — it guides behavior without changing model weights. You should make the most out of prompting before moving to more resource-intensive techniques like finetuning.

> "Anyone can communicate, but not everyone can communicate effectively."

## Anatomy of a Prompt

1. **Task description** — what you want the model to do, including role/persona and output format
2. **Examples** (shots) — demonstrations of the desired behavior
3. **The task** — the concrete input to process

## In-Context Learning

Introduced by GPT-3 (Brown et al., 2020): models can learn new behaviors from prompt examples without weight updates. This is a form of **continual learning** — models can incorporate new information (e.g., updated documentation) at inference time.

- **Zero-shot**: No examples, just instructions
- **Few-shot**: k examples provided (k-shot learning)
- As models improve, fewer examples are needed (GPT-4 shows limited improvement from few-shot over zero-shot)
- François Chollet's metaphor: a foundation model is a "library of programs"; prompt engineering finds the right prompt to activate the desired program

## System Prompt vs. User Prompt

- **System prompt**: Task description / developer instructions (processed first)
- **User prompt**: End-user input / the concrete task
- Under the hood, they're concatenated via a **chat template** (model-specific, e.g., Llama 2 vs. Llama 3 templates)
- System prompts likely boost performance because: (1) they come first, and (2) models may be post-trained to prioritize them (OpenAI's instruction hierarchy)

> Template mismatches are a common source of silent failures. Always verify the chat template.

## Context Length and Efficiency

- Context length expanded 2,000× in 5 years (1K → 2M tokens)
- **Context efficiency** matters more than length — the **needle in a haystack** (NIAH) test shows models are much better at finding information at the beginning and end than the middle
- RULER benchmark evaluates long-context processing capability

## Best Practices

1. **Write clear, explicit instructions** — adopt personas, specify output format, use markers for structured output
2. **Provide sufficient context** — reduces [[Hallucination]]; context construction tools (retrieval, web search) discussed in Ch.6
3. **Break complex tasks into subtasks** — prompt decomposition enables monitoring, debugging, parallelization, cheaper models for simpler steps
4. **Give the model time to think** — [[Chain-of-Thought]] and self-critique
5. **Iterate systematically** — version prompts, track experiments, separate prompts from code
6. **Evaluate prompt engineering tools** — inspect hidden API calls, verify templates, understand costs

## Prompt Organization

- **Separate prompts from code** (e.g., `prompts.py` imported by `application.py`)
- Benefits: reusability, independent testing, readability, collaboration with domain experts
- **Prompt catalogs** with metadata (model, date, application, creator) for search and versioning
- `.prompt` file formats: Firebase Dotprompt, Humanloop, Continue Dev

## Prompt Engineering Tools

- **Automated optimization**: DSPy (Khattab et al., 2023), OpenPrompt — specify I/O format, metrics, and data; tools find optimal prompts automatically
- **AI-powered**: Models can write and improve prompts for you; [[DeepMind]]'s Promptbreeder uses evolutionary mutation; Stanford's TextGrad uses gradient-based optimization
- **Structured output tools**: Guidance, Outlines, Instructor
- **Caveat**: Hidden API call costs (10 variations × 30 examples = 300+ calls), template bugs, tools can change without warning

## Connections

- One of three [[Model Adaptation]] techniques (alongside RAG and finetuning)
- Relies on [[Instruction Following]] capability (Ch.4)
- [[Chain-of-Thought]] is the most impactful prompting technique
- Security risks covered in [[Jailbreaking]] and [[Defensive Prompt Engineering]]
- [[Evaluation Pipeline]] should evaluate prompts systematically

## Sources

- [[AI Engineering — Ch.5: Prompt Engineering|AI Engineering Ch.5]]
