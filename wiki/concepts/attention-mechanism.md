---
title: "Attention Mechanism"
type: concept
created: 2026-04-07
updated: 2026-04-07
tags: [architecture, transformer, deep-learning]
sources: [ai-engineering.pdf]
---

# Attention Mechanism

The core innovation inside the [[Transformer Architecture]]. Allows the model to weigh the importance of different input tokens when generating each output token — like referencing any page in a book rather than only the summary.

## How It Works

Three vector types, computed from input x using learned weight matrices:

- **Query (Q)** = x * W_Q — "What am I looking for?" (current decoder state)
- **Key (K)** = x * W_K — "What do I contain?" (like a page number)
- **Value (V)** = x * W_V — "What's my actual content?" (like page content)

**Attention score** = dot product of Q and K. High score → model uses more of that token's Value.

**Formula:** Attention(Q, K, V) = softmax(QK^T / sqrt(d)) * V

## Multi-Headed Attention

Vectors are split across multiple "heads" that attend to different groups of tokens simultaneously.

- Llama 2-7B: 32 heads, each operating on 128-dim vectors (4096 / 32)
- Outputs of all heads are concatenated, then transformed by an output projection matrix.

## Key Implications

- **KV Cache**: Each token has K and V vectors that must be stored. Longer sequences = more memory. This is why extending context length is hard.
- **Prefill parallelism**: All input K/V pairs computed at once. Output K/V pairs computed sequentially.
- Attention was introduced 3 years before the transformer (Bahdanau et al., 2014) but only took off when Vaswani showed it could replace RNNs entirely.

## Sources

- [[AI Engineering — Ch.2: Understanding Foundation Models|AI Engineering Ch.2]]
