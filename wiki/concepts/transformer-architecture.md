---
title: "Transformer Architecture"
type: concept
created: 2026-04-07
updated: 2026-04-07
tags: [architecture, attention, deep-learning, core-concept]
sources: [ai-engineering.pdf]
---

# Transformer Architecture

The dominant architecture for language-based [[Foundation Models]] since its introduction in "Attention Is All You Need" (Vaswani et al., 2017). Built on the [[Attention Mechanism]], replacing RNN-based seq2seq models.

## Problems It Solved

The transformer replaced seq2seq, which had two key limitations:

1. **Information bottleneck**: Seq2seq decoder only saw the final hidden state of the encoder — like writing a book summary from only the last paragraph.
2. **Sequential processing**: RNN encoder/decoder processed tokens one at a time. 200-token input = 200 sequential steps.

The transformer solves both: attention lets the decoder reference *any* input token (not just the final state), and input tokens are processed in *parallel*.

## Architecture

A transformer model consists of:

1. **Embedding module** — Converts tokens + positions into embedding vectors. Positional embeddings naively determine max context length.
2. **Transformer blocks** (stacked) — Each contains:
   - **Attention module**: Query (Q), Key (K), Value (V), and Output Projection matrices
   - **MLP module**: Linear (feedforward) layers + nonlinear activation (ReLU, GELU)
3. **Output layer** (unembedding / model head) — Maps output vectors to token probabilities for sampling.

Number of transformer blocks = number of "layers" in the model.

## Key Dimensions

| Model | Blocks | Model Dim | FF Dim | Vocab | Context |
|-------|--------|-----------|--------|-------|---------|
| Llama 2-7B | 32 | 4,096 | 11,008 | 32K | 4K |
| Llama 2-70B | 80 | 8,192 | 22,016 | 32K | 4K |
| Llama 3-405B | 126 | 16,384 | 53,248 | 128K | 128K |

## Inference: Prefill vs Decode

- **Prefill**: Process all input tokens in parallel → create KV cache. Fast.
- **Decode**: Generate output tokens one at a time (sequential). Slow.

This asymmetry motivates many inference optimization techniques (Ch.9).

## Limitations

- Context length scales quadratically with attention computation.
- Sequential output generation remains a bottleneck.
- Heavily optimized for specific hardware (originally TPUs, then GPUs) — hard for new architectures to compete.

## Alternative Architectures

- [[RWKV]]: Parallelizable RNN, no theoretical context length limit.
- [[State Space Models]] / Mamba: Linear-time scaling, strong on long sequences.
- [[Jamba]]: Hybrid transformer-Mamba.

## Sources

- [[AI Engineering — Ch.2: Understanding Foundation Models|AI Engineering Ch.2]]
