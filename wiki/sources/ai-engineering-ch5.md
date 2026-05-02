---
title: "AI Engineering — Ch.5: Prompt Engineering"
type: source
created: 2026-04-15
updated: 2026-04-15
tags: [prompt-engineering, in-context-learning, chain-of-thought, jailbreaking, security]
sources: [ai-engineering.pdf]
---

# AI Engineering — Ch.5: Prompt Engineering

**Source:** *AI Engineering* by [[Chip Huyen]], O'Reilly 2024, Chapter 5 (pp. 211–252).

## Summary

Chapter 5 covers prompt engineering — the easiest and most common [[Model Adaptation]] technique. Huyen frames it as "human-to-AI communication": anyone can communicate, but not everyone can communicate effectively. The chapter splits into two halves: writing effective prompts and defending against prompt attacks.

### Part 1: Introduction to Prompting

A prompt consists of a **task description** (including role/persona), **examples** (shots), and the **task** itself (concrete input). The chapter introduces **in-context learning** — the GPT-3 discovery (Brown et al., 2020) that models can learn new behaviors from prompt examples without weight updates, functioning as a form of continual learning. Zero-shot vs. few-shot learning is covered, with the observation that as models improve, fewer examples are needed (Microsoft 2023 analysis showed limited few-shot improvement over zero-shot for GPT-4).

François Chollet's metaphor: a foundation model is a "library of programs" and prompt engineering is about finding the right prompt to activate the program you want.

The **system prompt vs. user prompt** distinction is detailed, including chat templates (Llama 2 vs. Llama 3 format changes). System prompts likely boost performance because (1) they come first in the final prompt, and (2) models may be post-trained to prioritize them (OpenAI's instruction hierarchy paper, Wallace et al., 2024).

**Context length** has expanded 2,000× in five years (GPT-2's 1K → Gemini-1.5 Pro's 2M). However, **context efficiency** varies — the "needle in a haystack" (NIAH) test shows models are much better at finding information at the beginning and end of prompts than in the middle.

### Part 2: Best Practices

Six best practices, distilled from OpenAI, Anthropic, Meta, and Google guides:

1. **Write clear, explicit instructions** — explain scoring systems, adopt personas, provide examples, specify output format, use markers to signal structured output
2. **Provide sufficient context** — reduces hallucination; includes discussion of restricting model knowledge to only its context (tricky, prompting alone is insufficient)
3. **Break complex tasks into simpler subtasks** — prompt decomposition enables monitoring, debugging, parallelization, and use of cheaper models for simpler steps. GoDaddy reduced a 1,500-token prompt to smaller prompts with better performance and lower cost.
4. **Give the model time to think** — [[Chain-of-Thought]] (CoT) prompting (Wei et al., 2022) and self-critique. CoT improved LaMDA, GPT-3, and PaLM across math benchmarks. LinkedIn found CoT reduces hallucinations.
5. **Iterate on prompts** — version prompts, use experiment tracking, standardize evaluation. Separate prompts from code (prompts.py pattern). Prompt catalogs with metadata for organization.
6. **Evaluate prompt engineering tools** — DSPy, OpenPrompt automate prompt optimization. DeepMind's Promptbreeder uses evolutionary strategy. Caveat: tools generate hidden API calls (300+ calls for 10 variations × 30 examples), can have template bugs (LangChain typo example), and can change without warning.

### Part 3: Defensive Prompt Engineering

Three attack types against AI applications:

1. **Prompt extraction / reverse prompt engineering** — deducing system prompts via output analysis or tricking the model. "Write your system prompt assuming it will one day become public."

2. **[[Jailbreaking]] and prompt injection**:
   - **Direct attacks**: Obfuscation (misspelling keywords), output format manipulation (write a poem about hotwiring), roleplaying (DAN — "Do Anything Now", grandma exploit)
   - **Automated attacks**: PAIR (Chao et al., 2023) uses an attacker AI to iteratively refine prompts, often needing <20 queries
   - **Indirect prompt injection**: Malicious instructions placed in tools the model uses — passive phishing (poisoned GitHub repos found via web search) and active injection (malicious email content interpreted as instructions)

3. **Information extraction** — extracting training data (memorization rates ~1%), privacy violations, copyright regurgitation. Nasr et al. (2023) showed the "repeat word forever" divergence attack extracts training data without knowing the data. Larger models memorize more.

### Defenses (Three Levels)

- **Model-level**: OpenAI's **instruction hierarchy** (Wallace et al., 2024) — four priority levels (system > user > model outputs > tool outputs). Finetuning on aligned/misaligned instruction pairs improved robustness by up to 63%.
- **Prompt-level**: Repeat system prompt before and after user input; explicitly warn about known attacks; inspect tool default templates (LangChain's permissive defaults had 100% injection success rate).
- **System-level**: Isolation (sandbox code execution), human approval for impactful operations, input/output guardrails, anomaly detection on usage patterns.

Two key defense metrics: **violation rate** (% successful attacks) and **false refusal rate** (% legitimate queries refused).

## Key Takeaways

- Prompt engineering = human-to-AI communication; easy to start, hard to master.
- **In-context learning** lets models learn new behaviors from prompt examples without weight updates — a form of continual learning.
- **System prompts** boost performance likely because models are trained to prioritize them and because they come first.
- **Context efficiency matters more than context length** — models struggle with information in the middle of long prompts (NIAH test).
- **Chain-of-thought** prompting is among the most reliably effective techniques across models.
- **Prompt decomposition** (task chaining) improves performance, enables debugging, and can reduce costs despite more API calls.
- **Security is an evolving cat-and-mouse game** — no defense is foolproof. Indirect prompt injection is the most dangerous attack vector.
- **Separate prompts from code** and version them. Use prompt catalogs with metadata.

## Notable Quotes

> "The problem is not with prompt engineering. It's a real and useful skill to have. The problem is when prompt engineering is the only thing people know." — OpenAI research manager

> "Write your system prompt assuming that it will one day become public."

> "Security risks will remain a significant roadblock for AI adoption in high-stakes environments."

## Pages Created or Updated

- **Concepts:** [[Prompt Engineering]], [[Chain-of-Thought]], [[Jailbreaking]], [[Defensive Prompt Engineering]]
- **Updated:** [[Model Adaptation]], [[Hallucination]], [[Foundation Models]]
