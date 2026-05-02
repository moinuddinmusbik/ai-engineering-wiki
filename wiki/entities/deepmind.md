---
title: "DeepMind"
type: entity
created: 2026-04-07
updated: 2026-04-10
tags: [organization, research, google, scaling]
sources: [ai-engineering.pdf]
---

# DeepMind

Google's AI research lab. Major contributor to scaling research, alignment, and specialized AI models.

## Key Contributions (from wiki sources)

- **[[Chinchilla Scaling Law]]** — "Training Compute-Optimal Large Language Models" (2022). Trained 400 models to derive the optimal relationship between model size and training tokens.
- **AlphaFold** — Domain-specific model for protein structure prediction, trained on ~100K known protein structures.
- **Self-delusion hypothesis** — Ortega et al. (2021) proposed that models hallucinate because they can't distinguish their own outputs from given facts.
- **Test time compute research** — Snell et al. (2024) showed that scaling inference compute can be more efficient than scaling model parameters.
- **Gopher** — 280B parameter model. Used heuristic dialogue filtering from internet for SFT data.

## Key Contributions (from Ch.3–4)

- **SAFE** (Search-Augmented Factuality Evaluator, Wei et al., 2024) — Decomposes responses into individual statements, uses Google Search to verify each. A key technique for [[Factual Consistency]] evaluation.
- **Evaluation investment gap** — Balduzzi et al. noted that "developing evaluations has received little systematic attention compared to developing algorithms."

## Mentioned In

- [[AI Engineering — Ch.2: Understanding Foundation Models|AI Engineering Ch.2]]
- [[AI Engineering — Ch.3: Evaluation Methodology|AI Engineering Ch.3]]
- [[AI Engineering — Ch.4: Evaluate AI Systems|AI Engineering Ch.4]]
