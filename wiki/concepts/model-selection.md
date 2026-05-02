---
title: "Model Selection"
type: concept
created: 2026-04-10
updated: 2026-04-10
tags: [evaluation, deployment, open-source, api, benchmarks]
sources: [ai-engineering.pdf]
---

# Model Selection

Model selection is the process of choosing the right [[Foundation Models|foundation model]] for your application. You don't care about which model is the best overall — you care about which model is the best **for your use case**.

## The Four-Step Workflow

1. **Filter by hard attributes** — Remove models whose non-negotiable properties don't fit (licenses, data privacy, model size)
2. **Narrow via public benchmarks** — Use leaderboard rankings and benchmark scores to identify promising candidates
3. **Run private experiments** — Evaluate shortlisted models with your own [[Evaluation Pipeline]] and data
4. **Monitor in production** — Detect failure, collect feedback, iterate (Ch.10)

These steps are **iterative** — new information can change earlier decisions.

## Hard vs. Soft Attributes

- **Hard attributes**: Impossible or impractical to change (licenses, training data, model size, your own privacy policies). Can dramatically reduce the candidate pool.
- **Soft attributes**: Improvable through adaptation (accuracy, toxicity, factual consistency). Tricky to estimate improvement potential — a model at 20% accuracy might reach 70% with task decomposition, or remain unusable after weeks of tweaking.

## Build vs. Buy: Seven Axes

The evergreen question: use commercial model APIs or host open source models yourself?

| Axis | Model APIs | Self-Hosting |
|------|-----------|--------------|
| **Data privacy** | Risk of leaks (Samsung/ChatGPT incident); providers may use your data for training (Zoom policy change) | Data stays internal |
| **Data lineage** | Contracts can protect from copyright risk; but training data is opaque | Open data allows inspection but thorough review of TB-scale data is impractical |
| **Performance** | Strongest models will likely be proprietary (incentive structure) | Gap is closing (MMLU trend) but open source will likely lag |
| **Functionality** | Scaling, function calling, structured outputs out-of-box; but logprobs often restricted | Full access to logprobs and intermediate outputs; but must build infrastructure |
| **Cost** | Per-token pricing, expensive at scale | Engineering/talent cost; but per-token cost decreases with scale |
| **Control** | Rate limits, opaque model changes, risk of losing access (Italy banned OpenAI briefly) | Full control; can freeze models; but must maintain infrastructure |
| **On-device** | Not possible without internet | Required for offline/privacy use cases |

## Open Source Terminology

- **Open source**: Broadly, any model whose weights are public (Huyen's usage)
- **Open weight**: Weights public, training data hidden (vast majority of "open source" models)
- **Open model**: Weights + training data public
- **Restricted weight**: Open source with restrictive licenses

### Licensing Complexity
- Llama 2/3: Custom licenses, >700M MAU requires special license, distillation restricted
- Apache 2.0: Most permissive (Gemma, Mistral-7B)
- Key questions: Commercial use? Restrictions? Can outputs train other models?

## Navigating Public Benchmarks

Thousands of benchmarks exist; BIG-bench alone has 214. Key considerations:
- **Benchmark correlation** matters — strongly correlated benchmarks exaggerate biases
- **Leaderboards** (Hugging Face Open LLM, Stanford HELM) help but use limited benchmarks
- **Data contamination** is pervasive — models trained on benchmark data achieve misleadingly high scores
- Public benchmarks filter out bad models but **won't find the best model for you**
- Running benchmarks is expensive (~$80K–$100K for 30 models on full HELM)
- Benchmarks saturate rapidly: GLUE (2018) → SuperGLUE (2019) → MMLU (2020) → MMLU-Pro (2024)

## Connections

- Follows from [[Evaluation-Driven Development]] — criteria determine selection
- [[Comparative Evaluation]] (Chatbot Arena) provides relative rankings
- [[Perplexity]] can detect data contamination in benchmarks
- Leads into [[Evaluation Pipeline]] for private experiments
- [[AI Engineering Stack]] — model selection sits at the model development layer

## Sources

- [[AI Engineering — Ch.4: Evaluate AI Systems|AI Engineering Ch.4]]
