---
title: "Chain-of-Thought"
type: concept
created: 2026-04-15
updated: 2026-04-15
tags: [prompting, reasoning, technique]
sources: [ai-engineering.pdf]
---

# Chain-of-Thought

Chain-of-thought (CoT) prompting encourages a model to "think step by step" before arriving at an answer, nudging it toward systematic problem solving. Among the first prompting techniques proven to work well across models and tasks.

## Origin

Introduced by Wei et al. (2022) in "Chain-of-Thought Prompting Elicits Reasoning in Large Language Models" — almost a year before ChatGPT launched.

## Variants

| Variant | Description |
|---------|------------|
| **Zero-shot CoT** | Simply add "think step by step" or "explain your rationale" to the prompt |
| **Zero-shot CoT (structured)** | Specify the exact steps the model should follow |
| **Few-shot CoT** | Include examples that demonstrate step-by-step reasoning |
| **Self-critique** | Ask the model to check/evaluate its own output (also known as self-eval) |

## Effectiveness

- Improved LaMDA, GPT-3, and PaLM on math benchmarks (MAWPS, SVAMP, GSM-8K)
- LinkedIn found CoT **reduces hallucinations**
- Works across model families and tasks, making it one of the most reliable prompting techniques

## Trade-offs

- **Increased latency**: Model performs multiple intermediate steps before the user sees the first output token
- **Increased cost**: More output tokens generated (model APIs charge per token)
- **Unpredictable step count**: If the model invents its own steps, the resulting chain can be lengthy and expensive
- Combined with prompt decomposition, these costs compound

## Connections

- Core technique of [[Prompt Engineering]]
- Related to test-time compute / inference scaling (Ch.2) — more compute at inference → better results
- Self-critique variant connects to [[AI as a Judge]] (self-evaluation)
- [[Sampling]] parameters (temperature) affect CoT quality
- Reducing [[Hallucination]] is a key benefit

## Sources

- [[AI Engineering — Ch.5: Prompt Engineering|AI Engineering Ch.5]]
