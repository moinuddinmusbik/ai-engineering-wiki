---
title: "Self-Supervision"
type: concept
created: 2026-04-07
updated: 2026-04-07
tags: [training, machine-learning, core-concept]
sources: [ai-engineering.pdf]
---

# Self-Supervision

A training approach where the model infers labels from the input data itself, rather than requiring manually created labels. This is the key enabler that allowed [[Language Models]] to scale up into [[Large Language Models]].

## How It Works

In language modeling, each input sequence provides both labels and context. The sentence "I love street food." yields six training samples:

| Input (context) | Output (next token) |
|-----------------|---------------------|
| \<BOS\> | I |
| \<BOS\>, I | love |
| \<BOS\>, I, love | street |
| \<BOS\>, I, love, street | food |
| \<BOS\>, I, love, street, food | . |
| \<BOS\>, I, love, street, food, . | \<EOS\> |

## Why It Matters

- **Removes the data-labeling bottleneck.** Manual labeling costs ~$0.05/image. Scaling ImageNet to 1M categories would cost ~$50M. Self-supervision gets labels for free from existing text.
- **Enables massive scale.** Text is everywhere — books, articles, Reddit, code. Self-supervision turns all of it into training data.
- **This is what made LLMs possible.** Other ML algorithms (fraud detection, object detection) require supervision. Language modeling's self-supervised nature is what allowed the scaling that produced ChatGPT.

## Distinctions

- **Self-supervised ≠ unsupervised.** In self-supervision, labels are inferred from data. In unsupervised learning, no labels are needed at all.
- **Natural language supervision** is a variant used for multimodal models (e.g., CLIP trained on image-text pairs found on the internet).

## Sources

- [[AI Engineering — Ch.1: Introduction to Building AI Applications with Foundation Models|AI Engineering Ch.1]]
