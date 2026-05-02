---
title: "Sampling"
type: concept
created: 2026-04-07
updated: 2026-04-07
tags: [inference, core-concept, temperature, probabilistic]
sources: [ai-engineering.pdf]
---

# Sampling

How a model chooses its output from all possible options. One of the most underrated concepts in AI — it explains hallucinations, inconsistency, and many seemingly baffling AI behaviors.

## Fundamentals

1. Model computes a **logit vector** (one logit per token in vocabulary)
2. **Softmax** converts logits to probabilities: p_i = e^(x_i) / sum(e^(x_j))
3. Model **samples** the next token according to this probability distribution

**Greedy sampling** = always pick highest probability token. Deterministic but boring.

## Sampling Strategies

### Temperature
Adjusts logits before softmax. Logits are divided by temperature T.

| Temperature | Effect |
|------------|--------|
| T < 1 | More deterministic, model favors high-probability tokens |
| T = 1 | No adjustment (default) |
| T > 1 | More creative, rare tokens get higher probability |
| T → 0 | Greedy sampling (always pick the most likely token) |

Recommended: **0.7 for creative tasks**. Providers typically limit T to [0, 2].

### Top-k
Only consider the top-k logits. k typically 50–500. Reduces computation but fixed regardless of context.

### Top-p (Nucleus Sampling)
Dynamic selection — sum probabilities in descending order until reaching p (typically 0.9–0.95). More contextually appropriate than top-k. For "yes or no?" → considers 2 tokens. For open questions → considers many.

### Min-p
Set minimum probability threshold a token must reach to be considered.

## Logprobs

Probabilities on log scale — avoids underflow with large vocabularies. Useful for classification, evaluation, and understanding model behavior. Many providers limit or don't expose logprobs (security concern — makes model replication easier).

## [[Test Time Compute]]

Generate multiple outputs, pick the best one. Selection methods:
- Highest average logprob
- Reward model / verifier scoring
- Most common answer (self-consistency)
- Application-specific heuristics

Key finding: A verifier gives the **same performance boost as 30x model size increase**.

## [[Structured Outputs]]

Five approaches, from lightweight to heavy:
1. **Prompting** — instruct the format (not guaranteed)
2. **Post-processing** — fix common mistakes (LinkedIn: 90% → 99.99% valid YAML)
3. **Test time compute** — regenerate until valid
4. **Constrained sampling** — filter logits to only valid tokens per grammar rules
5. **Finetuning** — train on correctly formatted examples (most effective)

## Sources

- [[AI Engineering — Ch.2: Understanding Foundation Models|AI Engineering Ch.2]]
