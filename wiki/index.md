---
title: "Wiki Index"
type: overview
created: 2026-04-07
updated: 2026-04-27
tags: [meta, index]
sources: []
---

# Wiki Index

Master catalog of all wiki pages. Updated on every ingest.

## Sources

- [[LLM Wiki Pattern]] — Founding methodology for this wiki (2026-04-07)
- [[AI Engineering — Ch.1: Introduction to Building AI Applications with Foundation Models|AI Engineering Ch.1]] — Rise of AI engineering, use cases, the AI stack (2026-04-07)
- [[AI Engineering — Ch.2: Understanding Foundation Models|AI Engineering Ch.2]] — Training data, transformer architecture, scaling, post-training, sampling (2026-04-07)
- [[AI Engineering — Ch.3: Evaluation Methodology|AI Engineering Ch.3]] — Perplexity, exact evaluation, embeddings, AI as a judge, comparative evaluation (2026-04-10)
- [[AI Engineering — Ch.4: Evaluate AI Systems|AI Engineering Ch.4]] — Evaluation criteria, factual consistency, safety, instruction following, model selection, evaluation pipeline (2026-04-10)
- [[AI Engineering — Ch.5: Prompt Engineering|AI Engineering Ch.5]] — In-context learning, best practices, chain-of-thought, prompt decomposition, jailbreaking, defenses (2026-04-15)
- [[AI Engineering — Ch.6: RAG and Agents|AI Engineering Ch.6]] — RAG architecture, retrieval algorithms, agents, planning, memory (2026-04-25)
- [[AI Engineering — Ch.7: Fine-Tuning|AI Engineering Ch.7]] — When to finetune, finetuning types, memory math, PEFT, LoRA, QLoRA, model merging (2026-04-27)
- [[AI Engineering — Ch.8: Dataset Engineering|AI Engineering Ch.8]] — Data quality criteria, coverage, data flywheel, augmentation, AI-powered synthesis, model distillation (2026-04-27)
- [[AI Engineering — Ch.9: Inference Optimization|AI Engineering Ch.9]] — Prefill/decode, metrics (goodput, TTFT, TPOT), KV cache, speculative decoding, FlashAttention, batching, parallelism (2026-04-27)
- [[AI Engineering — Ch.10: AI Engineering Architecture|AI Engineering Ch.10]] — Progressive architecture, guardrails, router, gateway, caching, observability, drift detection (2026-04-27)

## Entities

- [[Chip Huyen]] — Author of AI Engineering, defines the field
- [[OpenAI]] — Model provider, GPT series, pioneered model-as-a-service
- [[Anthropic]] — Model provider, Claude family, safety-focused
- [[GitHub Copilot]] — Flagship AI coding tool, $100M ARR in 2 years
- [[DeepMind]] — Google's AI lab, Chinchilla scaling law, AlphaFold, SAFE factuality evaluator
- [[Meta]] — Open-weight Llama models, PyTorch, Llama Guard
- [[LMSYS]] — Chatbot Arena, comparative evaluation, crowdsourced model ranking
- [[Hugging Face]] — Open LLM Leaderboard, model/dataset hosting platform

## Concepts

- [[AI Engineering]] — Discipline of building apps on foundation models
- [[Foundation Models]] — Large-scale general-purpose models (LLMs + multimodal)
- [[Language Models]] — Statistical models of language, tokens, autoregressive vs masked
- [[Self-Supervision]] — Training without manual labels; the enabler of LLM scale
- [[Model Adaptation]] — The trichotomy: prompt engineering, RAG, finetuning
- [[AI Engineering Stack]] — Three layers: app development, model development, infrastructure
- [[Transformer Architecture]] — Dominant architecture since 2017, attention-based
- [[Attention Mechanism]] — Key/Query/Value vectors, multi-headed attention
- [[Chinchilla Scaling Law]] — Optimal tokens ≈ 20x parameters (DeepMind, 2022)
- [[Sampling]] — Temperature, top-k, top-p, logprobs, structured outputs
- [[Post-Training]] — SFT + preference finetuning (RLHF/DPO), the Shoggoth metaphor
- [[Hallucination]] — Self-delusion and knowledge mismatch hypotheses, factual consistency evaluation
- [[Perplexity]] — Cross entropy, BPC/BPB; language modeling metric, data contamination detection
- [[Exact Evaluation]] — Functional correctness (pass@k), lexical similarity (BLEU/ROUGE), semantic similarity
- [[Embeddings]] — Numerical representations for semantic similarity; BERT, CLIP, joint multimodal spaces
- [[AI as a Judge]] — Using AI to evaluate AI; biases, specialized judges, criteria ambiguity
- [[Comparative Evaluation]] — Head-to-head model ranking; Elo, Bradley-Terry, Chatbot Arena
- [[Evaluation-Driven Development]] — Define evaluation criteria before building; the TDD of AI
- [[Factual Consistency]] — Local (against context) vs global (against open knowledge); SelfCheckGPT, SAFE
- [[Safety Evaluation]] — Six harm categories; toxicity classifiers, political bias in models
- [[Instruction Following]] — IFEval (25 verifiable types), INFOBench (broader criteria), roleplaying
- [[Model Selection]] — Hard vs soft attributes; build vs buy (7 axes); benchmark navigation
- [[Evaluation Pipeline]] — Component-level evaluation, guidelines, rubrics, per-turn vs per-task
- [[Prompt Engineering]] — In-context learning, system/user prompts, context length, best practices
- [[Chain-of-Thought]] — Step-by-step reasoning prompting; CoT, self-critique variants
- [[Jailbreaking]] — Prompt attacks: direct, automated (PAIR), indirect injection, information extraction
- [[Defensive Prompt Engineering]] — Three-level defense: model (instruction hierarchy), prompt, system
- [[RAG]] — Retrieval-augmented generation: retriever + generator, term-based and embedding-based retrieval
- [[Vector Search]] — ANN algorithms (LSH, HNSW, PQ, IVF, Annoy), vector databases, benchmarks
- [[Agents]] — Environment + tools + AI planner; knowledge augmentation, capability extension, write actions
- [[Agent Planning]] — Plan generation, validation, execution; ReAct, Reflexion, control flows, function calling
- [[Memory]] — Three-tier hierarchy: internal knowledge, short-term context, long-term external
- [[Finetuning]] — When to finetune, SFT/RLHF/DPO types, memory math, PEFT methods, training tactics
- [[LoRA]] — Low-rank adaptation, QLoRA (4-bit + LoRA), multi-LoRA serving, rank selection
- [[Quantization]] — Numerical formats (FP32→INT4), PTQ vs QAT, mixed precision training
- [[Model Merging]] — Model soups, task vectors, SLERP, TIES/DARE, frankenmerging, MoE creation
- [[Dataset Engineering]] — Data quality criteria, coverage/diversity, data flywheel, annotation, deduplication
- [[Data Synthesis]] — Augmentation, AI-powered synthesis (Alpaca, UltraChat), model distillation, model collapse
- [[Inference Optimization]] — Prefill/decode split, KV cache, speculative decoding, FlashAttention, continuous batching, parallelism
- [[AI Engineering Architecture]] — Progressive system architecture, guardrails, router, gateway, caching, observability, drift detection

## Analyses

- [[AI Engineering — All Reference URLs]] — Complete table of all 880 unique hyperlinks from the book, organized by chapter with link text and description (2026-05-02)
