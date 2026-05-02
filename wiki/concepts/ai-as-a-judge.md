---
title: "AI as a Judge"
type: concept
created: 2026-04-10
updated: 2026-04-10
tags: [evaluation, subjective, llm-judge, biases]
sources: [ai-engineering.pdf]
---

# AI as a Judge

AI as a judge (also: LLM as a judge) is the approach of using AI to evaluate AI responses. An AI judge is not just a model — it's a **system** that includes a model, a prompt, and sampling parameters. Changing any of these changes the judge.

## Why AI Judges?

- **Fast, cheap, flexible** compared to human evaluators
- Can work **without reference data** (usable in production)
- Can evaluate based on **any criteria**: correctness, toxicity, coherence, factual consistency, roleplaying fidelity, etc.
- Can **explain their decisions** (useful for auditing)
- GPT-4's agreement with humans reached **85%**, exceeding inter-human agreement (81%) on MT-Bench (Zheng et al., 2023)
- **58%** of evaluations on LangChain's platform were done by AI judges (2023)

## How to Use

Three fundamental approaches:
1. **Score a response** alone (given the question) — pointwise scoring
2. **Compare to reference** — alternative to hand-designed similarity measurements
3. **Compare two responses** — determine which is better (useful for preference data and [[Comparative Evaluation]])

### Prompting an AI Judge

A judge prompt should specify:
1. **The task** (e.g., evaluate relevance)
2. **The criteria** with detailed instructions
3. **The scoring system**: classification (good/bad), discrete numerical (1–5), or continuous (0–1)
   - Language models work better with classification than numerical scoring
   - Wider discrete ranges → worse performance
   - Include examples of each score level with justifications

## Limitations

### Inconsistency
- Same judge, same input → can output different scores across runs
- Including evaluation examples increases GPT-4 consistency from 65% to 77.5% (but quadruples cost)
- High consistency ≠ high accuracy (can consistently make the same mistakes)

### Criteria Ambiguity
- No standardization across tools: MLflow, Ragas, and LlamaIndex all define "faithfulness" differently (scoring 1–5, 0/1, and YES/NO respectively)
- Scores from different tools are **not comparable**
- Changing the judge prompt (even fixing a typo) can change evaluation results over time

### Increased Costs and Latency
- Using GPT-4 for both generation and evaluation approximately doubles API costs
- Evaluating three criteria = 4× API calls
- Mitigations: use weaker judge models, spot-check (evaluate a subset)
- In production, evaluation adds latency before returning responses to users

### Biases
| Bias | Description | Severity |
|------|-------------|----------|
| **Self-bias** | Models favor their own responses (GPT-4: +10% win rate; Claude-v1: +25%) | Significant |
| **First-position bias** | Favors the first option in pairwise comparisons (opposite of human recency bias) | Mitigable via reordering |
| **Verbosity bias** | Prefers longer responses regardless of quality; GPT-4 and Claude-1 prefer ~100-word responses with factual errors over ~50-word correct responses | Decreasing with stronger models |

## Specialized Judges

General-purpose judges are flexible but small, specialized judges can be more reliable for specific tasks:

- **Reward models**: Score (prompt, response) pairs. Example: Cappy (Google, 360M parameters, scores 0–1)
- **Reference-based judges**: Evaluate against reference responses. Examples: BLEURT (similarity score), Prometheus (quality score 1–5 given rubric)
- **Preference models**: Predict which of two responses users prefer. Examples: PandaLM, JudgeLM. Essential for generating preference data for [[Post-Training]] alignment.

## Key Principle

> "Do not trust any AI judge if you can't see the model and the prompt used for the judge."

## Connections

- The main alternative to [[Exact Evaluation]] for open-ended tasks
- Essential for evaluating [[Factual Consistency]], [[Safety Evaluation]], and [[Instruction Following]]
- Preference models feed into [[Post-Training]] (RLHF/DPO)
- [[Comparative Evaluation]] often uses AI judges as evaluators
- Judge quality depends on [[Sampling]] parameters

## Sources

- [[AI Engineering — Ch.3: Evaluation Methodology|AI Engineering Ch.3]]
- [[AI Engineering — Ch.4: Evaluate AI Systems|AI Engineering Ch.4]]
