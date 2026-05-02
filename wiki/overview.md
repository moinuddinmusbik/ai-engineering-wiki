---
title: "Wiki Overview"
type: overview
created: 2026-04-07
updated: 2026-04-27
tags: [meta, overview]
sources: []
---

# Wiki Overview

This is a personal knowledge base (second brain) focused on **AI Engineering** — the discipline of building applications on top of foundation models.

## Current Scope

The wiki is being built primarily from **AI Engineering** by [[Chip Huyen]] (O'Reilly, 2024), a comprehensive textbook covering the full stack of building AI applications. Ingestion is proceeding chapter-by-chapter.

## Major Themes

### The AI Engineering Discipline
[[AI Engineering]] emerged as a distinct field from [[ML Engineering]], driven by the availability of powerful [[Foundation Models]]. The focus shifted from *training* models to *adapting and evaluating* them.

### Foundation Models as Platform
[[Foundation Models]] — from [[Language Models]] through LLMs to multimodal models — serve as the platform on which all AI applications are built. Their general-purpose nature enables an unprecedented range of use cases.

### The Adaptation Trichotomy
Three core techniques for customizing models ([[Model Adaptation]]):
1. [[Prompt Engineering]] — steer via instructions
2. [[RAG]] — augment with external knowledge
3. [[Finetuning]] — update model weights

### The Stack
The [[AI Engineering Stack]] has three layers: application development, model development, and infrastructure.

### How Models Work (Ch.2)
Understanding the internals: the [[Transformer Architecture]] and [[Attention Mechanism]] dominate today. Scale is governed by the [[Chinchilla Scaling Law]]. Models are aligned via [[Post-Training]] (SFT → RLHF/DPO). [[Sampling]] strategies (temperature, top-k/p) control output behavior. [[Hallucination]] and inconsistency remain core unsolved challenges.

### Evaluation as the Bottleneck (Ch.3–4)
Evaluation is the **biggest bottleneck** to AI adoption. Chapters 3 and 4 build a comprehensive evaluation framework:

- **Language modeling metrics**: [[Perplexity]], cross entropy, BPC/BPB — foundational measures that also enable data contamination detection and anomaly flagging.
- **Exact evaluation**: [[Exact Evaluation]] via functional correctness (pass@k) and similarity measurements (exact match, lexical via BLEU/ROUGE, semantic via [[Embeddings]] and cosine similarity).
- **Subjective evaluation**: [[AI as a Judge]] — the most common production evaluation method (58% of LangChain evaluations). Powerful but prone to biases (self-bias, position bias, verbosity bias) and lacking standardization.
- **Comparative evaluation**: [[Comparative Evaluation]] via head-to-head matchups. [[LMSYS]]'s Chatbot Arena is the gold standard, using Bradley–Terry ratings. Harder to game than benchmarks but reveals only relative, not absolute, performance.
- **Evaluation criteria**: [[Evaluation-Driven Development]] — define criteria before building. Four buckets: domain-specific capability, generation quality (especially [[Factual Consistency]]), [[Safety Evaluation]], and [[Instruction Following]].
- **Model selection**: [[Model Selection]] requires distinguishing hard attributes (deal-breakers) from soft attributes (improvable), navigating the build-vs-buy decision across seven axes, and treating public benchmarks as filters rather than final answers.
- **Evaluation pipeline**: [[Evaluation Pipeline]] design — evaluate all components independently, create clear rubrics, combine exact and subjective methods.

### Prompt Engineering and Security (Ch.5)

[[Prompt Engineering]] is the first and most common [[Model Adaptation]] technique — crafting instructions to steer model behavior without changing weights. Key concepts include **in-context learning** (models learn from prompt examples at inference time), **[[Chain-of-Thought]]** prompting (step-by-step reasoning that improves accuracy and reduces hallucination), and **prompt decomposition** (breaking complex tasks into simpler subtasks for better performance, debugging, and cost management).

The chapter also reveals a fundamental tension: models are useful because they follow instructions, but this makes them vulnerable to **[[Jailbreaking]]** and prompt injection. Indirect prompt injection — where malicious instructions are placed in tools the model uses — is the most dangerous attack vector. [[Defensive Prompt Engineering]] operates at three levels: model (instruction hierarchy), prompt (explicit safety instructions), and system (sandboxing, human approval, guardrails). Security is an ever-evolving cat-and-mouse game with no foolproof solution.

### RAG, Agents, and Memory (Ch.6)

[[RAG]] is the dominant pattern for **context construction** — equivalent to feature engineering for classical ML. A RAG system retrieves relevant information from external sources (via term-based or embedding-based retrieval) and feeds it to a generator. Key insight: RAG won't be replaced by long context windows because data grows faster than context limits, and models don't use long context well. [[Vector Search]] (ANN algorithms like HNSW, IVF+PQ) powers embedding-based retrieval, with hybrid search (reciprocal rank fusion) combining both approaches.

[[Agents]] extend RAG by giving models tools to **act upon** their environment. Three tool categories: knowledge augmentation, capability extension (calculators, code interpreters, multimodal tools), and write actions (the most powerful but most dangerous). [[Agent Planning]] is the core challenge — the chapter advocates decoupling planning from execution with validation in between. ReAct interleaves reasoning and action; Reflexion adds separate evaluation and self-reflection. Agent accuracy degrades multiplicatively across steps (95% per step → 0.6% over 100 steps), making planning failures the primary concern.

[[Memory]] provides a three-tier hierarchy: internal knowledge (model weights), short-term (context window), and long-term (external storage via retrieval). Memory management strategies range from simple FIFO to reflection-based approaches that handle contradictions.

### Finetuning and Data Engineering (Ch.7–8)

[[Finetuning]] is the third [[Model Adaptation]] technique — and the most powerful, but most expensive. The key decision rule: *"Finetuning is for form, RAG is for facts."* Fine-tune when the model exhibits behavioral problems (wrong format, wrong tone); use RAG when it lacks knowledge. Full finetuning at scale is enabled by [[Quantization]] (FP32 → INT4, QLoRA) and [[LoRA]] (low-rank matrix decomposition: 0.003% of parameters for GPT-3 scale). [[Model Merging]] extends the picture post-training: combining finetuned checkpoints via task vectors, SLERP, or frankenmerging, without retraining.

[[Dataset Engineering]] is the data-centric counterpart: as models converge architecturally, data quality becomes the primary differentiator. Six quality criteria (relevant, aligned, consistent, correctly formatted, sufficiently unique, compliant) plus coverage/diversity. The **data flywheel** — using production application data to improve training — is a major competitive advantage. [[Data Synthesis]] via AI-powered pipelines (Alpaca, UltraChat, Llama 3's 2.7M-example coding pipeline) and model distillation (teacher → student) allows scaling to millions of examples. Risk: **model collapse** (Shumailov et al.) from training too heavily on synthetic data without real data anchors.

### Inference Optimization (Ch.9)

[[Inference Optimization]] is where 90% of ML production costs are incurred. The fundamental split: **prefill** (compute-bound, processes prompt in parallel) vs. **decode** (memory bandwidth-bound, generates one token at a time). Production systems decouple these phases. **KV cache** is the single biggest optimization: storing key-value attention vectors across decoding steps eliminates redundant recomputation, at the cost of memory (Llama 2 13B: 54 GB of KV cache at batch=32). PagedAttention (vLLM), multi-query attention, and KV cache quantization reduce this footprint — Character.AI achieved 20× reduction. **Speculative decoding** (draft model → target verification) delivers ~2× speedup. **FlashAttention** fuses attention operators to reduce HBM traffic. **Continuous batching** maximizes GPU utilization by letting new requests join mid-generation. **Prompt caching** (Anthropic) stores KV pairs for repeated prompt prefixes: up to 90% cost savings for long cached prompts.

### Production Architecture (Ch.10)

[[AI Engineering Architecture]] synthesizes everything into a production blueprint built progressively: bare minimum (query→model→response) → context enhancement (RAG) → guardrails → router/gateway → caching → agent patterns. Key insights: the **reverse PII map pattern** (mask PII before model, re-insert from map in output); **semantic caching** (embedding-based cache lookup for higher hit rate); **logs vs traces** (traces follow a single request across all components — essential for debugging agentic pipelines in tools like LangSmith); **three drift sources** (system prompt, user behavior, model version — Chen et al. 2023 found GPT-4 March vs June 2023 showed significant behavioral drift). The gateway provides a unified facade across model providers; the router applies intent classification to direct queries to appropriate models.

## Key Entities

- [[Chip Huyen]] — Author, defines the field
- [[OpenAI]] — Pioneered model-as-a-service, GPT series
- [[Anthropic]] — Claude models, safety-focused
- [[Meta]] — Open-weight Llama family, PyTorch
- [[DeepMind]] — Scaling law, AlphaFold, hallucination research, SAFE factuality evaluator
- [[GitHub Copilot]] — Flagship AI coding product
- [[LMSYS]] — Chatbot Arena, crowdsourced comparative evaluation
- [[Hugging Face]] — Open LLM Leaderboard, model/dataset platform

## Progress

- **Ch.1** ingested (2026-04-07): Introduction, use cases, the AI stack
- **Ch.2** ingested (2026-04-07): Training data, architecture, scaling, post-training, sampling
- **Ch.3** ingested (2026-04-10): Evaluation methodology — perplexity, exact eval, embeddings, AI judge, comparative eval
- **Ch.4** ingested (2026-04-10): Evaluate AI systems — criteria, factual consistency, safety, instruction following, model selection, pipeline
- **Ch.5** ingested (2026-04-15): Prompt engineering — in-context learning, best practices, CoT, decomposition, jailbreaking, defenses
- **Ch.6** ingested (2026-04-25): RAG — retrieval algorithms, vector search, hybrid search, chunking, contextual retrieval; Agents — tools, planning, ReAct, Reflexion, function calling; Memory — three-tier hierarchy, management strategies
- **Ch.7** ingested (2026-04-27): Finetuning — when to finetune, SFT/RLHF/DPO, memory math, LoRA, QLoRA, quantization formats, model merging (soups, task vectors, SLERP, TIES/DARE, frankenmerging, MoE)
- **Ch.8** ingested (2026-04-27): Dataset engineering — data quality criteria, coverage, data flywheel, augmentation, AI-powered synthesis (Alpaca, UltraChat, Llama 3 coding pipeline), model distillation, model collapse
- **Ch.9** ingested (2026-04-27): Inference optimization — prefill/decode split, goodput/TTFT/TPOT, KV cache (PagedAttention, MQA/GQA), speculative decoding, FlashAttention, continuous batching, prompt caching, parallelism
- **Ch.10** ingested (2026-04-27): AI engineering architecture — progressive build-up, PII reverse map, input/output guardrails, router, gateway, semantic caching, logs vs traces, drift detection, orchestration

## Meta

This wiki is itself an instance of the [[LLM Wiki]] pattern — a persistent, compounding knowledge artifact maintained by an LLM agent.
