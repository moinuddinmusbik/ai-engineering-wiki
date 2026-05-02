---
title: "Data Synthesis"
type: concept
created: 2026-04-27
updated: 2026-04-27
tags: [data-synthesis, model-distillation, alpaca, synthetic-data, model-collapse, self-play]
sources: [ai-engineering-ch8.md]
---

# Data Synthesis

Data synthesis is the programmatic or AI-assisted generation of training data. It has become a central technique in modern AI development because human annotation doesn't scale to the quantities and diversities needed, and because well-designed synthetic pipelines can produce higher-quality data than noisy web scrapes.

## Why Synthesize Data?

- **Scale**: human annotation is expensive and slow; synthesis can generate millions of examples in hours
- **Control**: synthesized data can be targeted precisely at capability gaps
- **Privacy**: synthetic data avoids PII exposure
- **Rare examples**: synthesis can generate edge cases that almost never appear in real data
- **Cost**: GPT-4 generating training data for a smaller model is often cheaper than human annotation

## Rule-Based Synthesis

The oldest approach — use programs and templates to generate training examples:
- **Templates**: fill-in-the-blank structures (e.g., "What is the capital of {country}?")
- **Grammar-based**: generate syntactically valid but diverse text from formal grammars
- **Faker libraries**: generate realistic-looking PII (names, addresses, emails) for testing and anonymization pipelines

Rule-based synthesis produces highly controlled data but lacks the naturalness and diversity of real or model-generated text.

## Simulation-Based Synthesis

Generate data by simulating environments:
- **CARLA**: autonomous driving simulator generates camera/LiDAR data with perfect labels
- **Self-play**: agents play against each other, generating trajectories as training data. OpenAI's Dota 2 bot was trained entirely on self-play games — no human gameplay data needed.
- **Procedural generation**: video games and robotics environments generate endless variation

Particularly effective for domains where real data is dangerous, expensive, or impossible to collect at scale.

## AI-Powered Synthesis

Using language models to generate training data for themselves or other models. The dominant approach in modern LLM training.

### Instruction Generation (UltraChat)
Generate a diverse instruction dataset top-down:
1. Define **topics** (e.g., science, coding, creative writing)
2. For each topic, generate **subtopics**
3. For each subtopic, generate **instruction-response pairs**

The hierarchy ensures coverage across topics rather than clustering around the most common queries.

### Reverse Instruction Generation
Generate the output first, then derive the instruction:
1. Sample a piece of text (document, code, etc.)
2. Ask a model: "What instruction would produce this output?"
3. Pair (derived instruction, original text) as training example

Li et al. showed this produces more diverse and natural instruction pairs than forward generation (instruction → response), because the space of plausible outputs is richer than the space of instruction templates.

### Alpaca (Self-Instruct / Bootstrapping)
The landmark example of AI-powered data synthesis for instruction tuning:
1. Start with **175 human-written seed instruction-response pairs**
2. Prompt GPT-3 (175B) to generate new instruction-response pairs, showing the seeds as in-context examples
3. Filter and deduplicate
4. Result: **52,000 instruction-response pairs**
5. Finetune LLaMA-7B on these examples → Alpaca

Alpaca demonstrated that a 7B model finetuned on GPT-3-generated data could match GPT-3's instruction-following ability on many tasks. The limitation: Alpaca learns GPT-3's *style*, not its underlying reasoning capabilities (superficial imitation).

### Llama 3 Coding Synthesis Pipeline
A sophisticated multi-step pipeline that generated 2.7M coding examples:
1. **Generate problem descriptions**: diverse coding problems across languages and difficulty levels
2. **Generate solutions**: model writes code solutions to the problems
3. **Generate unit tests**: model writes tests for each solution
4. **Fix errors**: execute tests, feed failures back to model, iterate on fixes
5. **Translate to other languages**: take Python solutions, translate to Java, C++, Rust, etc.
6. **Back-translation verification**: translate back to Python and check semantic equivalence

The use of **execution feedback** (running unit tests) grounds the synthesis in correctness rather than just plausibility — a key quality advantage over pure text generation.

## Model Distillation

Distillation uses a large **teacher** model to generate training data for a smaller **student** model. Can be done two ways:

### Output Distillation (Response-Based)
Train the student to mimic the teacher's final outputs:
```
Student loss = cross_entropy(student_output, teacher_output)
```
Examples: Alpaca (7B student, GPT-3 teacher), Vicuna, WizardLM.

### Soft Distillation (Distribution-Based)
Train the student to match the teacher's full output probability distribution, not just the argmax:
```
Student loss = KL_divergence(student_distribution, teacher_distribution)
```
More information is transferred (the teacher's uncertainty is preserved), but requires white-box access to the teacher (can't use GPT-4 API for soft distillation).

### Notable Results
- **DistilBERT**: 40% smaller than BERT, retains 97% of capability via soft distillation
- **Alpaca**: 7B model approaches 175B GPT-3 on instruction following
- **LIMA**: 1,000 carefully curated examples (human-selected, not synthetic) → competitive with GPT-4 in 43% of head-to-head comparisons. Suggests *alignment quality* may be learnable with very small but high-quality datasets.
- **Nemotron-4 340B**: NVIDIA's 340B student model outperforms its 56B teacher by training on synthetic data generated by the 340B model itself (self-improvement loop)

## Limitations and Risks

### Model Collapse
Shumailov et al. (2023): when models are repeatedly trained on synthetic data from previous model generations (without anchoring on real data), quality **degrades monotonically**. Errors compound — the model's distribution drifts from reality. Real data must remain in the training mix as an anchor.

### Superficial Imitation
Alpaca learns to sound like GPT-3 but doesn't acquire GPT-3's reasoning depth. The student learns surface patterns (format, hedging language, response structure) without the underlying capability. This matters most for complex reasoning tasks.

### Quality Control
Synthetic data can contain hallucinations, errors, biases, and artifacts from the generating model. Quality filtering is essential (model-based scoring, rule-based filters, human spot-checking).

### Obscure Data Lineage
When a model is trained on data generated by another model, tracking what knowledge came from where becomes difficult. This creates legal and reproducibility challenges.

## Connections

- [[Dataset Engineering]] — data synthesis is one strategy within the broader dataset engineering workflow
- [[Finetuning]] — synthesized data is used as finetuning training data
- [[Hallucination]] — model collapse and superficial imitation are hallucination-adjacent failure modes
- [[Model Adaptation]] — distillation is a model adaptation strategy distinct from finetuning on human data
- [[Evaluation Pipeline]] — synthesized datasets need rigorous quality evaluation
