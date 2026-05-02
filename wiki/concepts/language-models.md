---
title: "Language Models"
type: concept
created: 2026-04-07
updated: 2026-04-07
tags: [models, nlp, fundamentals]
sources: [ai-engineering.pdf]
---

# Language Models

A model that encodes statistical information about one or more languages — predicting how likely a word (or token) is to appear in a given context.

## History

- Statistical nature of language discovered centuries ago.
- Sherlock Holmes story (1905): used letter frequency to decode ciphers.
- Claude Shannon (1951): "Prediction and Entropy of Printed English" — introduced entropy for language modeling.
- First language models emerged in the 1950s.
- Scaled up to [[Large Language Models]] via [[Self-Supervision]].

## Tokens

The basic unit of a language model. A token can be a character, word, or subword (like "-tion").

- GPT-4: ~100 tokens ≈ 75 words. Vocabulary size: 100,256.
- Mixtral 8x7B: vocabulary size 32,000.
- Subword tokenization balances efficiency (fewer units than words) with meaning (more than characters).

## Two Types

| Type | How it predicts | Example | Use cases |
|------|----------------|---------|-----------|
| **Autoregressive** | Next token from preceding tokens | "My favorite color is __" → "blue" | Text generation (dominant today) |
| **Masked** (e.g., BERT) | Missing token from surrounding context | "My favorite __ is blue" → "color" | Classification, sentiment, debugging |

## Completion as a Paradigm

A language model is a *completion machine*. Many tasks reduce to completion:
- Translation: "How are you in French is …" → "Comment ca va"
- Classification: "Is this email spam? …" → "Likely spam"
- Coding, summarization, math — all frameable as completion.

> Completion is powerful, but completion ≠ conversation. Post-training is needed to make models respond appropriately to requests.

## Sources

- [[AI Engineering — Ch.1: Introduction to Building AI Applications with Foundation Models|AI Engineering Ch.1]]
