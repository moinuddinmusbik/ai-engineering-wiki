---
title: "Wiki Log"
type: overview
created: 2026-04-07
updated: 2026-04-07
tags: [meta, log]
sources: []
---

# Wiki Log

Append-only chronological record of all wiki operations.

## [2026-04-07] evolve | Wiki Initialized
- **Operation:** evolve
- **Details:** Created wiki directory structure, CLAUDE.md schema, index.md, and log.md. Wiki is ready for first ingestion.
- **Pages touched:** [[Wiki Index]], [[Wiki Log]]

## [2026-04-07] ingest | LLM Wiki Pattern
- **Operation:** ingest
- **Details:** Ingested the founding methodology document that defines how this wiki operates. Created source summary page.
- **Pages touched:** [[LLM Wiki Pattern]], [[Wiki Overview]], [[Wiki Index]]

## [2026-04-07] ingest | AI Engineering Ch.1
- **Operation:** ingest
- **Details:** Ingested Chapter 1 of *AI Engineering* by Chip Huyen (pp. 25–71). Covers the rise of AI engineering, eight use case categories, the three-layer AI stack, and how AI engineering differs from ML engineering. Created 1 source page, 4 entity pages, 6 concept pages, and the wiki overview.
- **Pages created:** [[AI Engineering — Ch.1: Introduction to Building AI Applications with Foundation Models|AI Engineering Ch.1]], [[Chip Huyen]], [[OpenAI]], [[Anthropic]], [[GitHub Copilot]], [[AI Engineering]], [[Foundation Models]], [[Language Models]], [[Self-Supervision]], [[Model Adaptation]], [[AI Engineering Stack]], [[Wiki Overview]]
- **Pages updated:** [[Wiki Index]], [[Wiki Log]]

## [2026-04-07] ingest | AI Engineering Ch.2
- **Operation:** ingest
- **Details:** Ingested Chapter 2 of *AI Engineering* by Chip Huyen (pp. 73–135). Covers training data (multilingual, domain-specific), transformer architecture (attention, KV cache, prefill/decode), model sizing (Chinchilla scaling law, MoE, scaling bottlenecks), post-training (SFT, RLHF, DPO), and sampling (temperature, top-k/p, structured outputs, hallucination). Created 1 source page, 2 entity pages, 6 concept pages.
- **Pages created:** [[AI Engineering — Ch.2: Understanding Foundation Models|AI Engineering Ch.2]], [[DeepMind]], [[Meta]], [[Transformer Architecture]], [[Attention Mechanism]], [[Chinchilla Scaling Law]], [[Sampling]], [[Post-Training]], [[Hallucination]]
- **Pages updated:** [[Foundation Models]], [[Wiki Index]], [[Wiki Log]]

## [2026-04-10] ingest | AI Engineering Ch.3

- **Operation:** ingest
- **Details:** Ingested Chapter 3 of *AI Engineering* by Chip Huyen (pp. 137–180). Covers challenges of evaluating foundation models, language modeling metrics (entropy, cross entropy, BPC/BPB, perplexity), exact evaluation (functional correctness, lexical similarity via BLEU/ROUGE, semantic similarity via embeddings/cosine), AI as a judge (biases: self-bias, first-position bias, verbosity bias; specialized judges: reward models, reference-based, preference models), and comparative evaluation (Elo, Bradley-Terry, LMSYS Chatbot Arena). Created 1 source page, 1 entity page, 5 concept pages.
- **Pages created:** [[AI Engineering — Ch.3: Evaluation Methodology|AI Engineering Ch.3]], [[LMSYS]], [[Perplexity]], [[Exact Evaluation]], [[Embeddings]], [[AI as a Judge]], [[Comparative Evaluation]]
- **Pages updated:** [[Hallucination]], [[Foundation Models]], [[DeepMind]], [[Wiki Index]], [[Wiki Log]], [[Wiki Overview]]

## [2026-04-10] ingest | AI Engineering Ch.4

- **Operation:** ingest
- **Details:** Ingested Chapter 4 of *AI Engineering* by Chip Huyen (pp. 183–232). Covers evaluation-driven development, evaluation criteria (domain-specific capability via MCQs, factual consistency — local vs global, SelfCheckGPT, SAFE, textual entailment; safety — six harm categories, political biases in models; instruction following — IFEval 25 types, INFOBench; cost/latency), model selection (hard vs soft attributes, build vs buy across 7 axes, open source terminology, licensing), navigating public benchmarks (data contamination, leaderboards), and evaluation pipeline design (component-level evaluation, guidelines, rubrics). Created 1 source page, 1 entity page, 6 concept pages.
- **Pages created:** [[AI Engineering — Ch.4: Evaluate AI Systems|AI Engineering Ch.4]], [[Hugging Face]], [[Evaluation-Driven Development]], [[Factual Consistency]], [[Safety Evaluation]], [[Instruction Following]], [[Model Selection]], [[Evaluation Pipeline]]
- **Pages updated:** [[Hallucination]], [[Foundation Models]], [[DeepMind]], [[Wiki Index]], [[Wiki Log]], [[Wiki Overview]]

## [2026-04-15] ingest | AI Engineering Ch.5

- **Operation:** ingest
- **Details:** Ingested Chapter 5 of *AI Engineering* by Chip Huyen (pp. 211–252). Covers prompt engineering fundamentals (in-context learning, zero/few-shot, system vs user prompts, chat templates, context length/efficiency, needle-in-a-haystack test), best practices (clear instructions, personas, examples, context provision, prompt decomposition, chain-of-thought, self-critique, prompt versioning/catalogs, prompt optimization tools — DSPy, Promptbreeder), and defensive prompt engineering (prompt extraction, jailbreaking — DAN/grandma/PAIR/indirect injection, information extraction — divergence attacks/memorization/copyright, three-level defense — instruction hierarchy/prompt-level/system-level guardrails). Created 1 source page, 4 concept pages.
- **Pages created:** [[AI Engineering — Ch.5: Prompt Engineering|AI Engineering Ch.5]], [[Prompt Engineering]], [[Chain-of-Thought]], [[Jailbreaking]], [[Defensive Prompt Engineering]]
- **Pages updated:** [[Model Adaptation]], [[Hallucination]], [[Wiki Index]], [[Wiki Log]], [[Wiki Overview]]

## [2026-04-25] ingest | AI Engineering Ch.6

- **Operation:** ingest
- **Details:** Ingested Chapter 6 of *AI Engineering* by Chip Huyen (pp. 253–306). Covers RAG architecture (retriever + generator, context construction as feature engineering), retrieval algorithms (term-based: TF-IDF, BM25, Elasticsearch/inverted index; embedding-based: vector databases, ANN algorithms — LSH, HNSW, Product Quantization, IVF, Annoy, FAISS/ScaNN), hybrid search (reciprocal rank fusion), retrieval optimization (chunking strategies, reranking, query rewriting, contextual retrieval — Anthropic), evaluating retrievers (context precision/recall, NDCG/MAP/MRR, BEIR), RAG beyond text (multimodal RAG with CLIP, tabular RAG with text-to-SQL), agents (environment + tools, three tool categories — knowledge augmentation/capability extension/write actions, function calling, tool selection, Chameleon/Voyager), planning (decoupling from execution, intent classification, FM vs RL planners, plan generation via prompting/function calling, planning granularity, complex control flows — sequential/parallel/if/for loop, ReAct, Reflexion, agent failure modes), and memory (internal knowledge/short-term/long-term, FIFO, summarization-based, reflection-based management). Created 1 source page, 5 concept pages.
- **Pages created:** [[AI Engineering — Ch.6: RAG and Agents|AI Engineering Ch.6]], [[RAG]], [[Vector Search]], [[Agents]], [[Agent Planning]], [[Memory]]
- **Pages updated:** [[Embeddings]], [[Model Adaptation]], [[Hallucination]], [[Defensive Prompt Engineering]], [[Wiki Index]], [[Wiki Log]], [[Wiki Overview]]

## [2026-04-27] ingest | AI Engineering Ch.7

- **Operation:** ingest
- **Details:** Ingested Chapter 7 of *AI Engineering* by Chip Huyen (pp. 331–386). Covers the full finetuning decision framework ("finetuning is for form, RAG is for facts"), finetuning types (SFT, preference finetuning/RLHF/DPO, continued pre-training, infilling), memory bottleneck arithmetic (weights + activations + gradients + optimizer states; Adam stores 2 values/param), numerical representations (FP32/FP16/BF16/TF32/INT8/INT4/NF4), quantization (PTQ vs QAT vs mixed precision), PEFT (adapter-based vs soft prompt-based; LoRA dominant), LoRA deep dive (low-rank factorization W'=W+(α/r)×BA; 0.0027% of full params for GPT-3; rank 4–64; merged vs separate adapters for multi-LoRA serving), QLoRA (4-bit NF4 base + LoRA + paged optimizers → 65B model on single 48GB GPU), model merging (summing: model soups/task vectors/task arithmetic/SLERP/TIES/DARE; layer stacking: frankenmerging/MoE creation/SOLAR depthwise scaling; concatenation), and finetuning tactics (learning rate 1e-7 to 1e-3, gradient accumulation, prompt loss masking, frameworks: LLaMA-Factory/unsloth/PEFT/Axolotl/LitGPT). Created 1 source page, 4 concept pages.
- **Pages created:** [[AI Engineering — Ch.7: Fine-Tuning|AI Engineering Ch.7]], [[Finetuning]], [[LoRA]], [[Quantization]], [[Model Merging]]
- **Pages updated:** [[Model Adaptation]], [[Wiki Index]], [[Wiki Log]], [[Wiki Overview]]

## [2026-04-27] ingest | AI Engineering Ch.8

- **Operation:** ingest
- **Details:** Ingested Chapter 8 of *AI Engineering* by Chip Huyen (pp. 387–429). Covers data-centric AI philosophy, data quality criteria (6 criteria: relevant/aligned/consistent/correctly formatted/sufficiently unique/compliant), coverage and diversity as orthogonal concerns, Llama 3 data mix across training phases (general knowledge: 50%/52.66%/81.99% for pre-training/SFT/preference finetuning), data acquisition (public datasets, annotation guidelines, data flywheel), data augmentation (word replacement, perturbation, back-translation, paraphrasing), data synthesis (rule-based templates/Faker, simulation/CARLA/self-play, AI-powered: instruction generation/reverse instruction/Alpaca 175 seeds→52K examples, Llama 3 coding pipeline generating 2.7M examples via problem→solution→unit test→fix→translate→back-translate), model distillation (teacher-student: output distillation vs soft distillation; DistilBERT 40% smaller 97% capability; LIMA 1000 examples → 43% GPT-4 parity; Nemotron-4 340B), and limitations (model collapse/Shumailov 2023, superficial imitation, quality control, obscure data lineage). Created 1 source page, 2 concept pages.
- **Pages created:** [[AI Engineering — Ch.8: Dataset Engineering|AI Engineering Ch.8]], [[Dataset Engineering]], [[Data Synthesis]]
- **Pages updated:** [[Finetuning]], [[Wiki Index]], [[Wiki Log]], [[Wiki Overview]]

## [2026-04-27] ingest | AI Engineering Ch.9

- **Operation:** ingest
- **Details:** Ingested Chapter 9 of *AI Engineering* by Chip Huyen (pp. 429–473). Covers prefill (compute-bound) vs decode (memory bandwidth-bound) phases and production decoupling (DistServe), performance metrics (TTFT/TPOT/throughput/goodput — the business-critical metric, MFU/MBU vs nvidia-smi), AI accelerators (GPU/TPU/AWS Inferentia/Apple Neural Engine; inference = up to 90% of ML costs), memory hierarchy (CPU DRAM 25-50 GB/s → GPU HBM 256GB/s-1.5TB/s → GPU SRAM >10TB/s ~40MB), model-level optimizations (quantization/distillation/pruning), speculative decoding (draft → parallel verification → ~2× speedup for Chinchilla-70B), inference with reference (copy from input overlap → ~2× speedup), parallel decoding (Medusa 1.9× speedup), KV cache (formula: 2×B×S×L×H×M; Llama 2 13B batch=32 seq=2048 → 54GB), KV cache optimizations (PagedAttention/MQA/GQA/cross-layer/quantization; Character.AI 20× reduction), FlashAttention (kernel fusion, less HBM traffic), kernel optimization (vectorization/parallelization/loop tiling/operator fusion; torch.compile/XLA/TensorRT), service optimizations (static/dynamic/continuous batching; DistServe prefill-decode decoupling; prompt caching — Anthropic up to 90% cost savings 79% latency reduction for 100K cached prompt), parallelism (replica/tensor/pipeline/context/sequence). Created 1 source page, 1 concept page.
- **Pages created:** [[AI Engineering — Ch.9: Inference Optimization|AI Engineering Ch.9]], [[Inference Optimization]]
- **Pages updated:** [[Quantization]], [[RAG]], [[Wiki Index]], [[Wiki Log]], [[Wiki Overview]]

## [2026-04-27] ingest | AI Engineering Ch.10

- **Operation:** ingest
- **Details:** Ingested Chapter 10 of *AI Engineering* by Chip Huyen (pp. 473–535). Covers progressive AI system architecture build-up (bare minimum → context enhancement → guardrails → router/gateway → caching → agent patterns), input guardrails (PII detection/masking with reverse PII map, prompt injection defense), output guardrails (format validation, factual inconsistency detection, toxicity/sensitivity/brand-risk, retry logic, human fallback), router (intent classifier for specialized model routing, out-of-scope detection, clarification requests), gateway (unified interface, access control, cost management, fallback policies, load balancing), caching (exact vs semantic caching with embedding similarity + vector search; prompt caching server-side), agent patterns (loops, write actions), observability (MTTD/MTTR/CFR; metrics: format failures/factual consistency/toxicity/TTFT/TPOT/cost; logs vs traces — traces essential for agentic debugging, LangSmith), drift detection (system prompt drift, user behavior drift, model drift — Chen et al. found significant differences GPT-4 March vs June 2023), user feedback hierarchy (explicit thumbs → implicit conversational: generation stop/turn count → implicit token: output length), and pipeline orchestration (LangChain/LlamaIndex/LangGraph trade-offs). Created 1 source page, 1 concept page.
- **Pages created:** [[AI Engineering — Ch.10: AI Engineering Architecture|AI Engineering Ch.10]], [[AI Engineering Architecture]]
- **Pages updated:** [[RAG]], [[Agents]], [[Defensive Prompt Engineering]], [[Hallucination]], [[Wiki Index]], [[Wiki Log]], [[Wiki Overview]]

## [2026-05-02] query | All Reference URLs

- **Operation:** query
- **Details:** Extracted all 880 unique hyperlinks embedded in the AI Engineering PDF via PyMuPDF annotation parsing. Each URL was paired with its anchor text (the phrase linked in the book) and a surrounding sentence providing context. Results organized by chapter and saved as both a markdown table and a CSV for spreadsheet use.
- **Pages touched:** [[AI Engineering — All Reference URLs]]
- **Files created:** `wiki/analyses/ai-engineering-urls.md`, `wiki/analyses/ai-engineering-urls.csv`
