---
title: "LMSYS"
type: entity
created: 2026-04-10
updated: 2026-04-10
tags: [organization, evaluation, leaderboard, chatbot-arena]
sources: [ai-engineering.pdf]
---

# LMSYS

LMSYS (Large Model Systems Organization) is a research organization best known for creating **Chatbot Arena**, the premier [[Comparative Evaluation]] platform for foundation models.

## Chatbot Arena

- Users enter prompts and receive responses from two **anonymous** models
- Users vote for the preferred response; model names revealed only after voting
- Originally used Elo ratings; switched to **Bradley–Terry** algorithm (found Elo too sensitive to evaluator/prompt order)
- As of January 2024: 57 models evaluated using 244,000 comparisons
- AlpacaEval found near-perfect (0.98) correlation with human preference

## Key Contributions

- Published analysis of **one million conversations** from Vicuna demo and Chatbot Arena (Zheng et al., 2023), revealing the top 10 most common instruction types (roleplaying ranked 8th)
- Among 33,000 published prompts, 0.55% were just "hello"/"hi" — highlighting the standardization challenge
- Demonstrated that GPT-4's agreement with humans (85%) exceeds inter-human agreement (81%) on MT-Bench

## Relevance to Wiki Themes

LMSYS represents the state of the art in **crowdsourced model evaluation**. Chatbot Arena's comparative approach is considered more trustworthy than benchmark-based leaderboards because it's harder to game and captures actual human preference. However, it has limitations: quadratic scaling, no standardization of prompts, and inability to measure absolute (vs. relative) performance.

## Connections

- [[Comparative Evaluation]] — LMSYS is the primary implementer
- [[AI as a Judge]] — LMSYS research on judge biases (self-bias, position bias, verbosity bias)
- [[Model Selection]] — Chatbot Arena rankings used as one signal in model selection workflows

## Sources

- [[AI Engineering — Ch.3: Evaluation Methodology|AI Engineering Ch.3]]
- [[AI Engineering — Ch.4: Evaluate AI Systems|AI Engineering Ch.4]]
