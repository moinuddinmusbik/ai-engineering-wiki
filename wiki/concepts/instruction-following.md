---
title: "Instruction Following"
type: concept
created: 2026-04-10
updated: 2026-04-10
tags: [evaluation, capability, structured-outputs, benchmarks]
sources: [ai-engineering.pdf]
---

# Instruction Following

Instruction-following capability measures how well a model follows the instructions given to it. If the model is bad at following instructions, it doesn't matter how good the instructions are. It is a **core requirement** for foundation models — InstructGPT was named for being finetuned specifically for this.

## The Conflation Problem

Instruction-following failure can be confused with domain-specific capability failure. If a model fails to write a Vietnamese verse form (lục bát), it could be because:
- The model doesn't know the form (domain capability issue), or
- The model doesn't understand what it's being asked (instruction-following issue)

Similarly, poor performance can stem from bad instructions rather than a bad model.

## Key Benchmarks

### IFEval (Google)
Focuses on **automatically verifiable** format instructions. Zhou et al. (2023) identified 25 instruction types across six groups:

| Group | Example Instructions |
|-------|---------------------|
| **Keywords** | Include keywords, keyword frequency, forbidden words, letter frequency |
| **Language** | Response must be entirely in a specified language |
| **Length constraints** | Number of paragraphs, words, sentences; first word of i-th paragraph |
| **Detectable content** | Postscript, number of placeholders |
| **Detectable format** | Number of bullets, title in angular brackets, choose from options, highlighted sections, multiple sections, JSON format |

Score = fraction of instructions followed correctly.

### INFOBench (Qin et al., 2024)
Takes a **broader view** beyond format:
- Content constraints ("discuss only climate change")
- Linguistic guidelines ("use Victorian English")
- Style rules ("use a respectful tone")

Verification: decompose each instruction into yes/no criteria questions. A model passes if it meets all criteria. GPT-4 is a reasonably reliable evaluator — more accurate than Amazon Mechanical Turk annotators, though less than human experts.

## Roleplaying as Instruction Following

Roleplaying (assuming a character/persona) is the 8th most common use case in LMSYS's million-conversation dataset. Evaluated on:
- **Style**: Does the model capture the character's speaking style?
- **Knowledge**: Does the model respond based on the character's knowledge (including "negative knowledge" — things the character doesn't know)?
- Benchmarks: RoleLLM, CharacterEval

## Beyond Benchmarks

> "You should curate your own benchmark to evaluate your model's capability to follow your instructions using your own criteria."

A model performing well on IFEval/INFOBench may not follow your specific instructions well. If you need YAML output, include YAML in your benchmark. If you want the model to avoid "As a language model..." responses, test for that.

## Connections

- Essential for structured outputs ([[Sampling]] strategies like constrained decoding)
- [[Post-Training]] (especially SFT/InstructGPT) is how models learn to follow instructions
- [[AI as a Judge]] can evaluate the broader instruction-following criteria that IFEval can't automate
- Part of the [[Evaluation-Driven Development]] criteria framework

## Sources

- [[AI Engineering — Ch.4: Evaluate AI Systems|AI Engineering Ch.4]]
