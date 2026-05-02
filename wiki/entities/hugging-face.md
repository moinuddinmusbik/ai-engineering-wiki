---
title: "Hugging Face"
type: entity
created: 2026-04-10
updated: 2026-04-10
tags: [organization, platform, leaderboard, open-source]
sources: [ai-engineering.pdf]
---

# Hugging Face

Hugging Face is a leading AI platform and community hub, best known for hosting open source models, datasets, and evaluation infrastructure.

## Open LLM Leaderboard

Hugging Face's public leaderboard ranks open source models using aggregated benchmark scores:

**Version 1 (2023):** Six benchmarks, averaged:
1. ARC-C — grade school science reasoning
2. MMLU — knowledge across 57 subjects
3. HellaSwag — commonsense sentence completion
4. TruthfulQA — factual accuracy
5. WinoGrande — commonsense reasoning
6. GSM-8K — grade school math

**Version 2 (June 2024):** Entirely new, more challenging benchmarks after saturation:
- MATH lvl 5 (replacing GSM-8K)
- MMLU-PRO (replacing MMLU)
- GPQA — graduate-level Q&A
- MuSR — multistep reasoning
- BBH (BIG-bench Hard) — reasoning
- IFEval — [[Instruction Following]]

## Key Contributions

- Released BigCode model under BigCode Open RAIL-M v1 license
- Plots standard deviations of model performance to detect data contamination outliers
- Published benchmark correlation analysis when launching new leaderboard
- Community-responsive: explained benchmark selection guided by models' commonly used benchmarks

## Relevance to Wiki Themes

Hugging Face is central to the open source AI ecosystem. Their leaderboard illustrates both the value and limitations of public benchmarks: small benchmark sets can't capture the full range of [[Foundation Models]] capabilities, benchmarks saturate within 1–2 years, and averaging treats all benchmarks equally despite different difficulty levels.

## Connections

- [[Model Selection]] — the Open LLM Leaderboard is a key tool for narrowing model candidates
- [[Comparative Evaluation]] — HELM uses "mean win rate" as an alternative to averaging
- [[Foundation Models]] — Hugging Face is a primary distribution platform

## Sources

- [[AI Engineering — Ch.4: Evaluate AI Systems|AI Engineering Ch.4]]
