---
title: "Model Adaptation"
type: concept
created: 2026-04-07
updated: 2026-04-27
tags: [core-concept, prompt-engineering, rag, finetuning]
sources: [ai-engineering.pdf]
---

# Model Adaptation

The central organizing principle of [[AI Engineering]]: techniques for customizing a pre-trained [[Foundation Models|foundation model]] to your specific needs, without training from scratch.

## Two Categories

### 1. Prompt-Based (No Weight Changes)

- **[[Prompt Engineering]]**: Craft instructions and examples to steer model behavior.
- **[[RAG]]**: Retrieve external knowledge to supplement the prompt context.
- Easier to start, requires less data, allows experimentation across multiple models.
- May not be sufficient for complex tasks or strict performance requirements.

### 2. Weight-Based (Model Changes)

- **[[Finetuning]]**: Continue training the model on your data, updating weights.
- More complex, requires more data, but can significantly improve quality, latency, and cost.
- Required for tasks the model wasn't exposed to during pre-training.

## The Adaptation Spectrum

From Chip Huyen's framing:

```
Prompt Engineering (easiest, least data)
  → RAG (add external context)
    → Finetuning (most powerful, most data)
      → Pre-training from scratch (most expensive, rare)
```

These techniques are not mutually exclusive — production systems often combine them.

## The "Last Mile" Problem

> "The journey from 0 to 60 is easy, whereas progressing from 60 to 100 becomes exceedingly challenging." — Ding et al. (UltraChat)

- Initial demos are misleading — base model capabilities already look impressive.
- LinkedIn: 1 month to 80%, then 4 more months to reach 95%.
- Most time is spent fixing edge cases and hallucinations.

## Prompt Engineering in Depth (Ch.5)

Chapter 5 provides the full treatment of the first adaptation technique:

- **In-context learning** (GPT-3, 2020): Models learn new behaviors from prompt examples without weight updates — a form of continual learning.
- **Best practices**: Clear instructions, personas, examples, sufficient context, task decomposition, [[Chain-of-Thought]], systematic iteration.
- **Prompt organization**: Separate from code, version in catalogs with metadata.
- **Security**: [[Jailbreaking]] and prompt injection are inherent risks; [[Defensive Prompt Engineering]] operates at model, prompt, and system levels.
- **Key insight**: "You should make the most out of prompting before moving to more resource-intensive techniques like finetuning."

## RAG and Agents in Depth (Ch.6)

Chapter 6 provides the full treatment of the second prompt-based adaptation technique and introduces the agentic pattern:

- **[[RAG]]**: Context construction via retrieval — equivalent to feature engineering for classical ML. Uses term-based (TF-IDF, BM25) or embedding-based ([[Vector Search]]) retrieval, with optimization via chunking, reranking, query rewriting, and contextual retrieval.
- **[[Agents]]**: Extend RAG by giving models tools to perceive and act upon their environment. Tools fall into knowledge augmentation, capability extension, and write actions.
- **[[Agent Planning]]**: The core challenge — decoupling planning from execution, with reflection (ReAct, Reflexion) to catch and correct errors.
- **[[Memory]]**: Three-tier hierarchy (internal knowledge, short-term context, long-term external) enables agents to manage information overflow and persist across sessions.
- RAG and agents are both **prompt-based** — they influence model quality through inputs without modifying weights. Modifying the model itself (finetuning) is covered in Ch.7.

## Finetuning in Depth (Ch.7)

Chapter 7 provides the full treatment of the third adaptation technique:

- **Decision rule**: "Finetuning is for form, RAG is for facts." Fine-tune when the model has behavioral/output-format problems; use [[RAG]] when the model lacks knowledge.
- **Progression path**: prompt engineering → RAG → [[Finetuning]] → full pre-training (as last resort).
- **Finetuning types**: SFT (most common), preference finetuning (RLHF/DPO), continued pre-training, infilling.
- **Memory math**: training requires weights + activations + gradients + optimizer states (Adam adds 2× weight memory in optimizer states alone).
- **PEFT / [[LoRA]]**: low-rank adaptation updates only ~0.003% of parameters, enabling finetuning of 65B+ models on single GPUs via QLoRA.
- **[[Model Merging]]**: combine capabilities from multiple finetuned checkpoints post-training via task arithmetic, SLERP, frankenmerging, or MoE creation.
- **[[Quantization]]**: reduce numerical precision (FP32 → BF16 → INT8 → INT4) to fit larger models in memory.

## Dataset Engineering (Ch.8)

Chapter 8 establishes that data quality is the primary lever once models converge architecturally:

- **Data-centric AI**: improve data quality, coverage, and diversity rather than model architecture when performance plateaus.
- **[[Dataset Engineering]]**: six quality criteria (relevant, aligned, consistent, correctly formatted, sufficiently unique, compliant); coverage/diversity as orthogonal concerns; the data flywheel as competitive advantage.
- **[[Data Synthesis]]**: AI-powered synthesis (Alpaca, UltraChat, Llama 3 coding pipeline), model distillation (teacher→student), reverse instruction generation.
- **Model collapse risk**: training repeatedly on synthetic data without real data anchors degrades quality monotonically (Shumailov et al. 2023).

## Sources

- [[AI Engineering — Ch.1: Introduction to Building AI Applications with Foundation Models|AI Engineering Ch.1]]
- [[AI Engineering — Ch.5: Prompt Engineering|AI Engineering Ch.5]]
- [[AI Engineering — Ch.6: RAG and Agents|AI Engineering Ch.6]]
- [[AI Engineering — Ch.7: Fine-Tuning|AI Engineering Ch.7]]
- [[AI Engineering — Ch.8: Dataset Engineering|AI Engineering Ch.8]]
