---
title: "Perplexity"
type: concept
created: 2026-04-10
updated: 2026-04-10
tags: [evaluation, metrics, language-models, training]
sources: [ai-engineering.pdf]
---

# Perplexity

Perplexity (PPL) is the exponential of cross entropy and the default metric for evaluating [[Language Models]]. It measures the amount of uncertainty a model has when predicting the next token. Lower perplexity = better prediction = better model.

## Related Metrics

Four closely related metrics — if you know one, you can compute the other three:

| Metric | What it measures |
|--------|-----------------|
| **Entropy** H(P) | How much information each token carries on average |
| **Cross Entropy** H(P,Q) | How difficult it is for the model Q to predict the next token in data with distribution P |
| **Bits-per-Character (BPC)** | Cross entropy normalized per character |
| **Bits-per-Byte (BPB)** | Cross entropy normalized per byte (more standardized than BPC) |

Cross entropy = Entropy + KL Divergence: `H(P,Q) = H(P) + D_KL(P||Q)`. A perfectly trained model has cross entropy equal to the entropy of its training data.

Perplexity is defined as: `PPL(P,Q) = 2^H(P,Q)` (using bits) or `PPL(P,Q) = e^H(P,Q)` (using nats, as in PyTorch/TensorFlow).

## Interpretation Rules

- **More structured data → lower expected perplexity** (HTML is more predictable than prose)
- **Bigger vocabulary → higher perplexity** (more possible next tokens)
- **Longer context → lower perplexity** (more information to predict from)
- Values as low as 3 are not uncommon — remarkable given vocabularies of 10K–100K tokens

## Applications Beyond Training

1. **Proxy for model capability**: Larger GPT-2 models consistently showed lower perplexity across datasets (OpenAI, 2018)
2. **Data contamination detection**: If a model's perplexity on benchmark data is unusually low, the benchmark was likely in the training data
3. **Training data deduplication**: Add new data only if the model's perplexity on it is high
4. **Anomaly detection**: Unusually high perplexity flags gibberish or unusual texts

## Caveats

- **Post-training increases perplexity**: SFT and RLHF teach models to complete tasks, which can worsen raw next-token prediction. "Post-training collapses entropy."
- **Quantization** can change perplexity in unexpected ways
- Not ideal for comparing models with different tokenizers (use BPC/BPB instead)
- Many commercial providers have stopped reporting perplexity

## Connections

- Evolved from Claude Shannon's 1951 work on entropy of printed English
- Cross entropy is the training loss for autoregressive [[Language Models]]
- [[Sampling]] strategies (temperature, top-k/p) operate on the same probability distributions that perplexity measures
- Used in data contamination detection for [[Comparative Evaluation]] and public benchmarks

## Sources

- [[AI Engineering — Ch.3: Evaluation Methodology|AI Engineering Ch.3]]
- [[AI Engineering — Ch.2: Understanding Foundation Models|AI Engineering Ch.2]] (cross entropy as training loss)
