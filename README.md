# AI Engineering Wiki

A personal knowledge base (second brain) built from **AI Engineering** by Chip Huyen (O'Reilly, 2025). Maintained by an LLM agent following a structured schema — every page is generated and kept in sync by Claude.

---

## What's Inside

This wiki covers the full stack of building production AI applications, from foundation model internals to deployment architecture. It is organized as an [Obsidian](https://obsidian.md) vault with wikilinks, backlinks, and graph view.

```
.
├── README.md              ← You are here
├── CLAUDE.md              ← Schema & operating manual (governs LLM behavior)
├── raw/                   ← Source documents (immutable)
│   └── assets/
│       └── ai-engineering.pdf
└── wiki/                  ← LLM-generated knowledge base
    ├── index.md           ← Master catalog of all pages
    ├── overview.md        ← High-level synthesis of the entire wiki
    ├── log.md             ← Chronological record of all operations
    ├── sources/           ← One summary page per book chapter
    ├── entities/          ← People, orgs, tools, projects
    ├── concepts/          ← Ideas, frameworks, theories
    └── analyses/          ← Query results, comparisons, reference tables
```

---

## Coverage

All 10 chapters of *AI Engineering* have been ingested:

| Chapter | Topics |
|---|---|
| **Ch.1 — Introduction** | Use case taxonomy, AI engineering vs ML engineering, the three-layer stack |
| **Ch.2 — Foundation Models** | Transformer architecture, attention, scaling laws, post-training (SFT/RLHF/DPO), sampling, hallucination |
| **Ch.3 — Evaluation Methodology** | Perplexity, exact eval (BLEU/ROUGE/pass@k), embeddings, AI-as-a-judge, comparative evaluation (Chatbot Arena) |
| **Ch.4 — Evaluate AI Systems** | Evaluation criteria, factual consistency, safety evaluation, instruction following, model selection, eval pipeline |
| **Ch.5 — Prompt Engineering** | In-context learning, chain-of-thought, prompt decomposition, jailbreaking, defensive prompt engineering |
| **Ch.6 — RAG and Agents** | RAG architecture, vector search (HNSW/IVF/PQ), hybrid search, agents, planning (ReAct/Reflexion), memory |
| **Ch.7 — Fine-Tuning** | When to finetune, SFT/RLHF/DPO, memory math, LoRA, QLoRA, quantization, model merging |
| **Ch.8 — Dataset Engineering** | Data quality criteria, data flywheel, AI-powered synthesis (Alpaca, UltraChat), model distillation, model collapse |
| **Ch.9 — Inference Optimization** | Prefill/decode, KV cache, speculative decoding, FlashAttention, continuous batching, prompt caching, parallelism |
| **Ch.10 — AI Engineering Architecture** | Progressive architecture, guardrails, router/gateway, caching, observability, drift detection |

---

## Key Concepts

The wiki contains **49 concept pages** spanning the major themes:

**Adaptation** · [Model Adaptation](wiki/concepts/model-adaptation.md) · [Prompt Engineering](wiki/concepts/prompt-engineering.md) · [RAG](wiki/concepts/rag.md) · [Finetuning](wiki/concepts/finetuning.md) · [LoRA](wiki/concepts/lora.md) · [Quantization](wiki/concepts/quantization.md) · [Model Merging](wiki/concepts/model-merging.md)

**Evaluation** · [Hallucination](wiki/concepts/hallucination.md) · [Factual Consistency](wiki/concepts/factual-consistency.md) · [AI as a Judge](wiki/concepts/ai-as-a-judge.md) · [Evaluation Pipeline](wiki/concepts/evaluation-pipeline.md) · [Safety Evaluation](wiki/concepts/safety-evaluation.md)

**Data** · [Dataset Engineering](wiki/concepts/dataset-engineering.md) · [Data Synthesis](wiki/concepts/data-synthesis.md) · [Embeddings](wiki/concepts/embeddings.md)

**Systems** · [Agents](wiki/concepts/agents.md) · [Agent Planning](wiki/concepts/agent-planning.md) · [Memory](wiki/concepts/memory.md) · [Inference Optimization](wiki/concepts/inference-optimization.md) · [AI Engineering Architecture](wiki/concepts/ai-engineering-architecture.md)

**Models** · [Transformer Architecture](wiki/concepts/transformer-architecture.md) · [Attention Mechanism](wiki/concepts/attention-mechanism.md) · [Sampling](wiki/concepts/sampling.md) · [Post-Training](wiki/concepts/post-training.md)

---

## Reference Table

[`wiki/analyses/ai-engineering-urls.md`](wiki/analyses/ai-engineering-urls.md) contains all **880 unique hyperlinks** embedded in the book, organized by chapter with anchor text and context. Also available as a CSV: [`wiki/analyses/ai-engineering-urls.csv`](wiki/analyses/ai-engineering-urls.csv).

---

## How to Use

**Browse in Obsidian** (recommended):
1. Open Obsidian → Open folder as vault → select this directory
2. Use graph view to explore concept connections
3. Use backlinks panel to see all pages that reference a concept

**Browse on GitHub:**
- Start at [`wiki/overview.md`](wiki/overview.md) for a full thematic summary
- Use [`wiki/index.md`](wiki/index.md) to navigate to any specific page

**Query the wiki:**
Drop this repo into a Claude project and ask questions — the agent will read the wiki pages and synthesize answers with wikilinks.

---

## Schema

This wiki is governed by [`CLAUDE.md`](CLAUDE.md), which defines:
- Directory structure and page types
- Frontmatter schema
- Four operations: **INGEST**, **QUERY**, **LINT**, **EVOLVE**
- Ownership rules (human owns `raw/`, LLM owns `wiki/`)

The schema is designed to be reused for any knowledge domain — swap out the source material and the same agent workflow applies.

---

## Source

**Book:** *AI Engineering* by Chip Huyen (O'Reilly, 2025)
**Author's GitHub:** [github.com/chiphuyen/aie-book](https://github.com/chiphuyen/aie-book)
**Author's blog:** [huyenchip.com](https://huyenchip.com/blog/)
