---
title: "Embeddings"
type: concept
created: 2026-04-10
updated: 2026-04-25
tags: [representation, vectors, similarity, multimodal]
sources: [ai-engineering.pdf]
---

# Embeddings

An embedding is a numerical representation (vector) that aims to capture the meaning of data. Since computers work with numbers, models convert inputs into embeddings for processing. A good embedding algorithm produces vectors where **more-similar inputs have closer embeddings** (measured by cosine similarity).

## Properties

- Typical vector sizes: 100–10,000 elements (lower-dimensional than raw data)
- Any data modality can have embeddings: text, images, audio, products, users, graphs
- Cosine similarity: 1 = identical, 0 = orthogonal, –1 = opposite

## Key Models

| Model | Embedding Size | Notes |
|-------|---------------|-------|
| BERT (base/large) | 768 / 1024 | Open source, text only |
| CLIP | 512 (text + image) | Joint text–image space |
| OpenAI Embeddings API | 1536 / 3072 | Proprietary |
| Cohere Embed v3 | 384 / 1024 | Proprietary |
| Sentence Transformers | Varies | Open source, optimized for sentences |

## Joint (Multimodal) Embedding Spaces

A breakthrough direction: mapping data of different modalities into a shared vector space.

- **CLIP** (Radford et al., 2021): Trained on (image, text) pairs. Text encoder + image encoder project into a joint space where an image embedding is close to its caption's embedding. Enables text-based image search.
- **ULIP**: Unified text, images, and 3D point clouds
- **ImageBind** (Girdhar et al., 2023): Joint embedding across six modalities (text, images, audio, etc.)

## Applications

1. **Semantic similarity** — core of [[Exact Evaluation]] via BERTScore, MoverScore
2. **Vector search / retrieval** — backbone of [[RAG]] systems (Ch.6)
3. **Classification and topic modeling**
4. **Recommender systems** (Criteo, Coveo)
5. **Data deduplication** (Ch.8)
6. **Anomaly detection**

## Quality Evaluation

- Embeddings are evaluated by downstream task performance
- **MTEB** (Massive Text Embedding Benchmark) evaluates embedding quality across multiple tasks
- General-purpose models (GPT, Llama) also produce embeddings in intermediate layers, but specialized embedding models typically produce higher-quality embeddings

## Role in RAG (Ch.6)

Chapter 6 details how embeddings power the retrieval component of [[RAG]] systems:

- **Embedding-based retrieval** (semantic retrieval): documents are converted to embeddings and stored in vector databases. Queries are embedded using the same model, and [[Vector Search]] finds the closest document embeddings.
- **Multimodal RAG**: Joint embedding models like **CLIP** enable retrieving images alongside text by embedding both modalities into the same space.
- **Contextual retrieval**: [[Anthropic]]'s technique augments chunks with AI-generated context before embedding, improving retrieval quality.
- **Cost concern**: Embedding generation can be expensive at scale — regenerating embeddings for 100M documents daily is a significant cost driver. Vector database spending can reach one-fifth to half of model API spending.
- **BEIR benchmark** (Thakur et al., 2021): Evaluation harness for retrieval across 14 common benchmarks, complementing MTEB for embedding-specific evaluation.

## Connections

- [[Transformer Architecture]] includes an embedding layer
- [[Exact Evaluation]] uses embeddings for semantic similarity
- Central to [[RAG]] ([[Vector Search]], retrieval optimization) and data deduplication (Ch.8)
- [[AI as a Judge]] is an alternative when embedding-based similarity is insufficient

## Sources

- [[AI Engineering — Ch.3: Evaluation Methodology|AI Engineering Ch.3]]
- [[AI Engineering — Ch.6: RAG and Agents|AI Engineering Ch.6]]
