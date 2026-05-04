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

---

## References (39 links)

All hyperlinks embedded by the author in this chapter, extracted from the book PDF. See also the [complete URL reference](../analyses/ai-engineering-urls.md) across all chapters.


| URL | Name / Link Text | Description |
|-----|-----------------|-------------|
| https://arxiv.org/abs/1702.08734 | Johnson et al., 2017 | neighbor (ANN) algorithm. |
| https://oreil.ly/faJqj | Sun et al., 2020 | and libraries have been developed for it. |
| https://github.com/spotify/annoy | Spotify’s Annoy | and libraries have been developed for it. |
| https://oreil.ly/4ATBC | Hnswlib | Most application developers won’t implement vector search themselves, so I’ll give only a quick overview of different approaches. |
| https://github.com/nmslib/hnswlib | Hierarchical Navigable Small World | Most application developers won’t implement vector search themselves, so I’ll give only a quick overview of different approaches. |
| https://oreil.ly/MVsgB | series | For those wanting to learn more about vector search, Zilliz has an excellent series on it. |
| https://oreil.ly/slO9x | Indyk and Motwani, 1999 | LSH (locality-sensitive hashing) (Indyk and Motwani, 1999) This is a powerful and versatile algorithm that works with more than just vectors. |
| https://oreil.ly/VaLf4 | (Jégou et al., 2011 | Product Quantization (Jégou et al., 2011) This works by reducing each vector into a much simpler, lower-dimensional rep‐ resentation by decomposing each vector |
| https://oreil.ly/9BcYN | Sivic and Zisserman, 2003) | IVF (inverted file index) (Sivic and Zisserman, 2003) IVF uses K-means clustering to organize similar vectors into the same cluster. |
| https://github.com/microsoft/SPTAG | Microsoft’s SPTAG | There are other algorithms, such as Microsoft’s SPTAG (Space Partition Tree And Graph), and FLANN (Fast Library for Approximate Nearest Neighbors). |
| https://github.com/flann-lib/flann | FLANN | There are other algorithms, such as Microsoft’s SPTAG (Space Partition Tree And Graph), and FLANN (Fast Library for Approximate Nearest Neighbors). |
| https://en.wikipedia.org/wiki/Discounted_cumulative_gain | NDCG | For semantic retrieval, you need to also evaluate the quality of your embeddings. |
| https://en.wikipedia.org/wiki/Evaluation_measures_(information_retrieval)#Mean_average_precision | MAP | For semantic retrieval, you need to also evaluate the quality of your embeddings. |
| https://en.wikipedia.org/wiki/Mean_reciprocal_rank | MRR | For semantic retrieval, you need to also evaluate the quality of your embeddings. |
| https://oreil.ly/pbh3y | ANN-Benchmarks website | The ANN-Benchmarks website compares different ANN algorithms on multiple datasets using four main metrics, taking into account the trade-offs between indexing a |
| https://arxiv.org/abs/2104.08663 | Thakur et al., 2021 | Additionally, BEIR (Benchmarking IR) (Thakur et al., 2021) is an evaluation harness for retrieval. |
| https://oreil.ly/3xtwh | reciprocal rank fusion (RRF) | An algorithm for combining different rankings is called reciprocal rank fusion (RRF) (Cormack et al., 2009). |
| https://github.com/grantjenks/py-tree-sitter-languages#license | splitters | For example, there are splitters developed especially for different programming languages. |
| https://oreil.ly/-Sny7 | Anthropic, 2024 | Here’s the prompt Anthropic used for this purpose (Anthropic, 2024): <document> {{WHOLE_DOCUMENT}} / |
| https://arxiv.org/abs/2405.15793 | Yang et al., 2024 | Figure 6-8 shows a visualization of SWE-agent (Yang et al., 2024), an agent built on top of GPT-4. |
| https://arxiv.org/abs/2304.09842 | Lu et al., 2023 | Chameleon (Lu et al., 2023) shows that a GPT-4-powered agent, aug‐ mented with a set of 13 tools, can outperform GPT-4 alone on several benchmarks. |
| https://x.com/ylecun/status/1702027572077326505 | autoregres‐ | Meta’s Chief AI Scientist Yann LeCun states unequivocally that autoregres‐ sive LLMs can’t plan (2023). |
| https://oreil.ly/8_j7E | Kambhampati (2023) | In the article “Can LLMs Really Reason and Plan?” Kambhampati (2023) argues that LLMs are great at extracting knowledge but not planning. |
| https://arxiv.org/abs/2305.14992 | Hao et al., 2023 | This means it’s not sufficient to prompt a model to generate only a sequence of actions like what the popular chain-of-thought prompting technique does. |
| https://en.wikipedia.org/wiki/Reinforcement_learning | Wikipedia | They are both characterized by |
| https://oreil.ly/UziTE | Konda and Tsitsiklis, 1999 | 14 This reminds me of the actor-critic (AC) agent method (Konda and Tsitsiklis, 1999) in reinforcement learn‐ ing. |
| https://arxiv.org/abs/2210.03629 | Yao et al., 2022 | First proposed by ReAct (Yao et al., 2022), interleaving reasoning and action has become a common pattern for agents. |
| https://arxiv.org/abs/1809.09600 | Yang et al., 2018 | You can implement reflection in a multi-agent setting: one agent plans and takes actions, and another agent evaluates the outcome after each step or after a num |
| https://arxiv.org/abs/2303.11366 | Shinn et al., 2023 | This is the approach that Reflexion (Shinn et al., 2023) took. |
| https://github.com/noahshinn/reflexion | Reflexion Git‐ | Images from the Reflexion Git‐ Hub repo. |
| https://arxiv.org/abs/2302.04761 | Schick et al., 2023 | For example, Toolformer (Schick et al., 2023) finetuned GPT-J to learn five tools. |
| https://arxiv.org/abs/2305.15334 | Patil et al., 2023 | On the other hand, Gorilla (Patil et al., 2023) attempted to prompt agents to select the right API call among 1,645 APIs. |
| https://arxiv.org/abs/2305.16291 | Wang et al., 2023 | Vogager (Wang et al., 2023) proposes a skill manager to keep track of new skills (tools) that an agent acquires for later reuse. |
| https://github.com/aie-book | GitHub repository | I created a simple benchmark to illustrate these different failure modes that you can see on the book’s GitHub repository. |
| https://oreil.ly/lKB61 | Berkeley Function Calling Leaderboard | There are also agent benchmarks and leader‐ boards such as the Berkeley Function Calling Leaderboard, the AgentOps evaluation harness, and the TravelPlanner ben |
| https://github.com/AgentOps-AI/agentops | AgentOps evaluation | There are also agent benchmarks and leader‐ boards such as the Berkeley Function Calling Leaderboard, the AgentOps evaluation harness, and the TravelPlanner ben |
| https://github.com/OSU-NLP-Group/TravelPlanner | TravelPlanner benchmark | There are also agent benchmarks and leader‐ boards such as the Berkeley Function Calling Leaderboard, the AgentOps evaluation harness, and the TravelPlanner ben |
| https://arxiv.org/abs/2210.08750 | Bae et al. (2022) | One way to remove redundancy is by using a summary of the conversation. |
| https://arxiv.org/abs/2311.08719v1 | Liu et al. (2023) | each sentence in the summary, determines if only one, both, or neither should be added to the new memory. |
