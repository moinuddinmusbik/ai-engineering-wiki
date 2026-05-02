---
title: "Vector Search"
type: concept
created: 2026-04-25
updated: 2026-04-25
tags: [vector-search, ann, retrieval, infrastructure]
sources: [ai-engineering.pdf]
---

# Vector Search

Vector search is the process of finding vectors in a database that are closest to a query vector. It is the backbone of embedding-based [[RAG]] retrieval and is also widely used in search engines, recommender systems, clustering, fraud detection, and other applications.

## Vector Databases

A vector database stores vectors and enables fast vector search. The hard part isn't storage — it's **search**: indexing and organizing vectors so that similar ones can be found quickly.

Even though vector databases emerged as their own category with the rise of RAG, any database that can store vectors qualifies. Many traditional databases have extended (or will extend) to support vector storage and search.

> Vector database spending can be one-fifth or even half of a company's model API spending.

## Naive Approach: k-NN

The simplest approach is k-nearest neighbors:
1. Compute similarity (e.g., cosine similarity) between query embedding and **all** vectors
2. Rank by similarity
3. Return top k

Precise but computationally heavy — only viable for small datasets.

## Approximate Nearest Neighbor (ANN) Algorithms

For large datasets, **ANN algorithms** trade some accuracy for much faster retrieval. Vectors are organized into buckets, trees, or graphs using heuristics to keep similar vectors close.

### LSH (Locality-Sensitive Hashing)
*Indyk and Motwani, 1999*

Hashes similar vectors into the same buckets. Versatile — works with more than just vectors. Trades accuracy for efficiency. Implemented in FAISS and Annoy.

- **Index**: Quick to build, low memory
- **Query**: Slower, less accurate than graph-based methods

### HNSW (Hierarchical Navigable Small World)
*Malkov and Yashunin, 2016*

Constructs a multi-layer graph where nodes are vectors and edges connect similar ones. Nearest-neighbor search traverses graph edges. Open-source implementation by authors; also in FAISS and Milvus.

- **Index**: Significant time and memory to build
- **Query**: High accuracy, fast query times

### Product Quantization
*Jégou et al., 2011*

Reduces each vector to a lower-dimensional representation by decomposing into subvectors. Distances computed on compressed representations. Key component of FAISS; supported by almost all vector search libraries.

### IVF (Inverted File Index)
*Sivic and Zisserman, 2003*

Uses K-means clustering to organize similar vectors into clusters (typically 100–10,000 vectors per cluster). During querying, finds closest cluster centroids, then searches within those clusters. Together with Product Quantization, forms the backbone of FAISS.

### Annoy (Approximate Nearest Neighbors Oh Yeah)
*Bernhardsson, 2013 (Spotify)*

Tree-based approach. Builds multiple binary trees that split vectors using random criteria. During search, traverses trees to gather candidate neighbors. Open-sourced by Spotify.

### Other Algorithms

- **SPTAG** (Microsoft) — Space Partition Tree And Graph
- **FLANN** — Fast Library for Approximate Nearest Neighbors

## Popular Libraries

| Library | Organization | Key Algorithms |
|---------|-------------|----------------|
| **FAISS** | Meta (Facebook AI) | LSH, HNSW, IVF + Product Quantization |
| **ScaNN** | Google | Scalable Nearest Neighbors |
| **Annoy** | Spotify | Tree-based splitting |
| **Hnswlib** | Open source | HNSW |

## Indexing vs. Querying Trade-offs

A more detailed index → more accurate retrieval but slower/more memory-intensive indexing:

- **HNSW**: High accuracy, fast queries, but expensive to build
- **LSH**: Quick and cheap to build, but slower and less accurate queries

## Evaluation Metrics

The **ANN-Benchmarks** website compares ANN algorithms across:

1. **Recall** — fraction of true nearest neighbors found
2. **Queries per second (QPS)** — throughput for high-traffic applications
3. **Build time** — important if data changes frequently
4. **Index size** — scalability and storage requirements

Vectors can also be **quantized** (reduced precision) or made **sparse** to reduce computational cost.

## Sources

- [[AI Engineering — Ch.6: RAG and Agents|AI Engineering Ch.6]]
