---
title: "AI Engineering — Ch.6: RAG and Agents"
type: source
created: 2026-04-25
updated: 2026-04-25
tags: [source, ai-engineering, rag, agents, memory]
sources: [ai-engineering.pdf]
---

# AI Engineering — Ch.6: RAG and Agents

**Source:** *AI Engineering* by [[Chip Huyen]], O'Reilly 2024, Chapter 6 (pp. 253–306)

## Summary

This chapter covers two dominant patterns for context construction in AI applications: **[[RAG]]** (retrieval-augmented generation) and **[[Agents]]**. Both patterns allow models to access information beyond their training data and context limits, but agents extend further by enabling models to take actions in their environment.

**RAG** enhances model generation by retrieving relevant information from external memory sources. A RAG system has two components: a retriever (indexing + querying) and a generator. The chapter covers two main retrieval approaches: term-based (TF-IDF, BM25, Elasticsearch with inverted indexes) and embedding-based (vector databases, ANN algorithms). It argues that RAG will remain relevant even as context windows grow, because longer context doesn't mean better context usage, and data always grows faster than context limits.

**Retrieval optimization** techniques include chunking strategies (equal length, recursive, overlapping), reranking, query rewriting, and contextual retrieval — Anthropic's technique of augmenting each chunk with AI-generated context explaining its relationship to the original document. The chapter also covers RAG beyond text: multimodal RAG using [[Embeddings]] like CLIP, and tabular RAG using text-to-SQL.

**Agents** are defined by their environment and the actions they can perform. Tools fall into three categories: knowledge augmentation (retrievers, web browsing, APIs), capability extension (calculators, code interpreters, multimodal tools), and write actions (modifying databases, sending emails). The chapter emphasizes that write actions are powerful but require serious security consideration.

**[[Agent Planning]]** is presented as the core intellectual challenge of agents. The chapter advocates decoupling planning from execution, with plan validation as an intermediate step. It covers function calling, planning granularity (exact function names vs. natural language), complex control flows (sequential, parallel, if, for loop), and reflection/error correction patterns like ReAct and Reflexion.

**[[Memory]]** provides the third major concept: three memory mechanisms (internal knowledge, short-term/context, long-term/external) and management strategies (FIFO, summarization-based, reflection-based with contradiction handling).

## Key Takeaways

- RAG is equivalent to feature engineering for classical ML — it gives the model the necessary information to process an input
- RAG won't be replaced by long context: data grows faster than context limits, and models don't use long context well
- Anthropic suggests that knowledge bases under 200K tokens (~500 pages) can skip RAG entirely
- Term-based retrieval (BM25) remains a formidable baseline — "Making a genuine improvement over BM25 is hard" (Perplexity CEO)
- [[Vector Search]] introduces ANN algorithms (LSH, HNSW, Product Quantization, IVF, Annoy) with different speed/accuracy/memory trade-offs
- Hybrid search combining term-based and embedding-based retrieval via reciprocal rank fusion often outperforms either alone
- Contextual retrieval (Anthropic) augments chunks with AI-generated context, improving retrieval quality
- Agents face compound accuracy problems: 95% per-step accuracy → 60% over 10 steps → 0.6% over 100 steps
- Tool use (Chameleon) improved GPT-4's ScienceQA results by 11.37% and TabMWP by 17%
- Planning should be decoupled from execution to avoid fruitless runs
- ReAct interleaves reasoning and action; Reflexion adds separate evaluator and self-reflection modules
- Memory management strategies range from simple FIFO to reflection-based approaches that handle contradictions

## Notable Quotes

> "Context construction for foundation models is equivalent to feature engineering for classical ML models."

> "Just as you shouldn't give an intern the authority to delete your production database, you shouldn't allow an unreliable AI to initiate bank transfers."

> "If we can get people to trust a machine to take us into space, I hope that one day, security measures will be sufficient for us to trust autonomous AI systems."

## Pages Created or Updated

- Created: [[RAG]], [[Vector Search]], [[Agents]], [[Agent Planning]], [[Memory]]
- Updated: [[Embeddings]], [[Model Adaptation]], [[Hallucination]], [[Defensive Prompt Engineering]]
