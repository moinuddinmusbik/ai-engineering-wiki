---
title: "RAG"
type: concept
created: 2026-04-25
updated: 2026-04-25
tags: [rag, retrieval, context-construction, core-pattern]
sources: [ai-engineering.pdf]
---

# RAG (Retrieval-Augmented Generation)

RAG is a technique that enhances a model's generation by retrieving relevant information from external memory sources. It is one of the three core [[Model Adaptation]] techniques (alongside [[Prompt Engineering]] and finetuning) and is the dominant pattern for context construction in AI applications.

> Context construction for foundation models is equivalent to feature engineering for classical ML models. They serve the same purpose: giving the model the necessary information to process an input.

## Architecture

A RAG system has two components:

1. **Retriever** — retrieves information from external memory sources (databases, documents, the internet)
   - **Indexing**: processing data so it can be quickly retrieved later
   - **Querying**: sending a query to retrieve relevant data
2. **Generator** — generates a response based on the retrieved information

In the original RAG paper (Lewis et al., 2020), the retriever and generator were trained together. Modern RAG systems typically use off-the-shelf components, though end-to-end finetuning can improve performance significantly.

## Why RAG Won't Be Replaced by Long Context

Despite rapidly expanding context windows, RAG remains essential:

1. **Data grows faster than context** — people generate and add new data but rarely delete it
2. **Long context ≠ good context usage** — longer context increases the chance the model focuses on the wrong information (see [[Prompt Engineering]], NIAH test)
3. **Cost and latency** — every extra context token incurs cost and potential latency
4. RAG ensures only the **most relevant** information is included per query

> [[Anthropic]] suggests that for Claude models, knowledge bases under 200,000 tokens (~500 pages) can skip RAG entirely.

## Retrieval Algorithms

### Term-Based Retrieval

Computes relevance at a **lexical** level. Two key metrics:

- **TF (Term Frequency)** — how often a term appears in a document
- **IDF (Inverse Document Frequency)** — rarer terms are more informative (IDF = log(N/C(t)))
- **TF-IDF** — combines both: Score(D, Q) = Σ IDF(tᵢ) × f(tᵢ, D)

Key solutions:
- **Elasticsearch** (Banon, 2010) — built on Lucene, uses an **inverted index** (maps terms → documents containing them)
- **BM25** (Robertson et al., 1980s) — modification of TF-IDF that normalizes by document length; still widely used as a formidable baseline

### Embedding-Based Retrieval

Computes relevance at a **semantic** level using [[Embeddings]]. Also called semantic retrieval.

Workflow:
1. **Indexing** — convert data chunks into embeddings, store in a vector database
2. **Query embedding** — convert query into an embedding using the same model
3. **[[Vector Search]]** — find k data chunks whose embeddings are closest to the query embedding

See [[Vector Search]] for ANN algorithms (LSH, HNSW, Product Quantization, IVF, Annoy).

### Comparing Approaches

| Aspect | Term-Based | Embedding-Based |
|--------|-----------|-----------------|
| Speed | Much faster | Slower (embedding generation + vector search) |
| Performance | Strong out-of-box, hard to improve | Can outperform with finetuning |
| Cost | Much cheaper | Embedding, storage, search can be expensive |
| Weakness | Term ambiguity | Can obscure keywords (error codes, product names) |

### Hybrid Search

Combining term-based and embedding-based retrieval. Can be done:
- **Sequentially** — cheap retriever fetches candidates, then more precise mechanism reranks
- **In parallel (ensemble)** — multiple retrievers fetch simultaneously, rankings combined via **Reciprocal Rank Fusion (RRF)**: Score(D) = Σ 1/(k + rᵢ(D)), where k ≈ 60

## Retrieval Optimization

### Chunking Strategies

Documents must be split into manageable chunks. Strategies:

- **Equal length** — fixed character/word/sentence/paragraph counts
- **Recursive** — split by sections → paragraphs → sentences until chunks fit max size
- **Overlapping** — prevents loss of boundary information (e.g., 2048-char chunks with 20-char overlap)
- **Token-based** — chunk by the generator's tokenizer (couples chunking to specific model)
- **Domain-specific** — by Q&A pairs, programming language constructs, etc.

**Trade-off**: Smaller chunks → more diversity but potential info loss and higher computational overhead. Larger chunks → more context per chunk but fewer chunks fit in the model's context.

### Reranking

Refine initial retriever rankings for accuracy. Approaches:
- Sequential reranking (cheap retriever → expensive reranker)
- Time-based reranking (weight recent data higher)
- Context reranking differs from search reranking: exact position is less critical as long as the document is included

### Query Rewriting

Rewrite ambiguous queries to be self-contained. Example: "How about Emily Doe?" → "When was the last time Emily Doe bought something from us?" Can be done by another AI model or heuristics. Gets complicated with identity resolution.

### Contextual Retrieval

Augment each chunk with context to improve retrievability:
- Add metadata (tags, keywords, entities)
- Add questions the chunk can answer
- **[[Anthropic]]'s technique**: Use an AI model to generate 50–100 token context explaining each chunk's relationship to its source document, then prepend to the chunk before indexing

## Evaluating RAG Systems

Evaluate both component-by-component and end-to-end:

1. **Retrieval quality**: context precision (% retrieved docs that are relevant) and context recall (% relevant docs that are retrieved)
2. **Ranking quality**: NDCG, MAP, MRR — for when document ordering matters
3. **Embedding quality**: MTEB benchmark, BEIR benchmark (14 retrieval benchmarks)
4. **Final output quality**: using evaluation methods from [[Evaluation Pipeline]]

> Vector database spending can be one-fifth or even half of a company's model API spending.

## RAG Beyond Text

### Multimodal RAG
If the generator is multimodal, context can include images, video, audio. Requires multimodal embedding models like CLIP to compare images to text queries. See [[Embeddings]].

### Tabular RAG
Uses **text-to-SQL**: (1) generate SQL query from natural language + table schemas, (2) execute SQL, (3) generate response from results. Can be seen as a bridge to the [[Agents]] pattern.

## Sources

- [[AI Engineering — Ch.6: RAG and Agents|AI Engineering Ch.6]]
