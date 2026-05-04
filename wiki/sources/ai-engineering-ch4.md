---
title: "AI Engineering — Ch.4: Evaluate AI Systems"
type: source
created: 2026-04-10
updated: 2026-04-10
tags: [evaluation, model-selection, benchmarks, factual-consistency, safety, instruction-following]
sources: [ai-engineering.pdf]
---

# AI Engineering — Ch.4: Evaluate AI Systems

**Source:** *AI Engineering* by [[Chip Huyen]], O'Reilly 2024, Chapter 4 (pp. 183–232).

## Summary

Where Chapter 3 covered evaluation *methods*, Chapter 4 covers how to use those methods to evaluate models *for your application*. It is structured in three parts: evaluation criteria, model selection, and evaluation pipeline design.

### Part 1: Evaluation Criteria

Huyen introduces [[Evaluation-Driven Development]] — defining evaluation criteria before building, inspired by test-driven development. She identifies four buckets of criteria:

1. **Domain-specific capability** — evaluated via MCQs and benchmarks (MMLU, HumanEval, BIRD-SQL). MCQs are popular because they're easy to create and verify, but they test classification ability (differentiating good from bad) rather than generation ability.

2. **Generation capability** — the chapter focuses on [[Factual Consistency]], the most pressing issue. Local factual consistency checks outputs against provided context; global factual consistency checks against open knowledge. Techniques include AI as a judge, self-verification (SelfCheckGPT), knowledge-augmented verification (SAFE by [[DeepMind]]), and textual entailment (NLI). TruthfulQA is the key benchmark (817 questions across 38 categories).

3. **[[Safety Evaluation]]** — covers six harm categories: inappropriate language, harmful recommendations, hate speech, violence, stereotypes, and political/religious bias. Tools range from general-purpose AI judges to specialized toxicity classifiers (Perspective API, Meta's Llama Guard). Studies show models have measurable political biases (GPT-4 leans left-libertarian; Llama leans authoritarian). Key benchmarks: RealToxicityPrompts, BOLD.

4. **[[Instruction Following]]** — IFEval identifies 25 automatically verifiable instruction types (keyword inclusion, length constraints, JSON format, etc.). INFOBench takes a broader view, including content constraints, linguistic guidelines, and style rules, verified via yes/no decomposition questions. Roleplaying evaluation (RoleLLM, CharacterEval) is also covered as a common real-world instruction type.

5. **Cost and latency** — metrics include time to first token, time per token, time per query. Pareto optimization across quality, latency, and cost is essential.

### Part 2: Model Selection

The [[Model Selection]] workflow has four steps: (1) filter by hard attributes, (2) narrow via public benchmarks, (3) run private experiments, (4) monitor in production. Hard attributes (licenses, data privacy, model size) vs. soft attributes (accuracy, toxicity — improvable via adaptation).

The **build vs. buy** analysis is thorough, covering seven axes: data privacy (Samsung ChatGPT leak), data lineage/copyright (evolving IP laws), performance (gap closing but incentives favor proprietary models keeping their best behind paywalls), functionality (logprobs, finetuning, structured outputs), API cost vs. engineering cost, control/transparency (model versioning risks, rate limits), and on-device deployment.

Open source terminology is clarified: **open weight** (weights public, data hidden) vs. **open model** (weights + data public). Licensing is complex — Llama licenses restrict >700M MAU and distillation; Apache 2.0 is the most permissive (Gemma, Mistral-7B).

### Part 3: Evaluation Pipeline Design

The chapter closes with practical pipeline design: evaluate all components independently (not just end-to-end), create clear evaluation guidelines with scoring rubrics, and use both per-turn and per-task evaluation. Benchmarks become saturated rapidly (GLUE → SuperGLUE → MMLU → MMLU-Pro) and are prone to data contamination. Detection methods include n-gram overlapping and perplexity analysis.

Public leaderboards ([[Hugging Face]] Open LLM Leaderboard, Stanford HELM, BIG-bench) are useful for initial filtering but can't replace custom evaluation. Benchmark correlation matters — strongly correlated benchmarks exaggerate biases. Stanford spent ~$80K–$100K to evaluate 30 models on full HELM.

## Key Takeaways

- **Evaluation-driven development**: define how you'll evaluate before you build. Evaluation is the biggest bottleneck to AI adoption.
- **Factual consistency** has two settings: local (against context) and global (against open knowledge). SelfCheckGPT and SAFE represent self-verification and knowledge-augmented approaches respectively.
- **Safety** encompasses six categories; models have measurable political biases that vary by provider.
- **IFEval** provides 25 automatically verifiable instruction types — a practical template for custom instruction-following benchmarks.
- **Model selection** should distinguish hard attributes (deal-breakers) from soft attributes (improvable). The build-vs-buy decision spans seven axes.
- **Open source ≠ open model**: most "open source" models are open weight only, with hidden training data.
- **Data contamination** is pervasive and often unintentional; public benchmarks should always be supplemented with private evaluation.
- **Benchmark saturation** is accelerating — benchmarks are replaced within 1–2 years.

## Notable Quotes

> "I believe that evaluation is the biggest bottleneck to AI adoption. Being able to build reliable evaluation pipelines will unlock many new applications."

> "An application that is deployed but can't be evaluated is worse [than one never deployed]. It costs to maintain, but if you want to take it down, it might cost even more."

## Pages Created or Updated

- **Concepts:** [[Evaluation-Driven Development]], [[Factual Consistency]], [[Safety Evaluation]], [[Instruction Following]], [[Model Selection]], [[Evaluation Pipeline]]
- **Entities:** [[Hugging Face]]
- **Updated:** [[Hallucination]], [[Foundation Models]], [[DeepMind]]

---

## References (94 links)

All hyperlinks embedded by the author in this chapter, extracted from the book PDF. See also the [complete URL reference](../analyses/ai-engineering-urls.md) across all chapters.


| URL | Name / Link Text | Description |
|-----|-----------------|-------------|
| https://en.wikipedia.org/wiki/Test-driven_development | test-driven development | The name is inspired by test-driven development in software engineering, which refers to the method of writing tests before writing code. |
| https://oreil.ly/mOAjn | BIRD- | Similarly, if an SQL query generated by your text to SQL model is correct but takes too long or requires too much memory to run, it might not be usable. |
| https://github.com/EleutherAI/lm-evaluation-harness/blob/master/docs/task_table.md | lm-evaluation-harness | the expected answer is option C and the model outputs option A, the model is wrong. |
| https://arxiv.org/abs/2304.06364 | Microsoft’s AGIEval (2023) | This is the approach that most public benchmarks follow. |
| https://oreil.ly/d3ggH | AI2 Reasoning Chal‐ | This is the approach that most public benchmarks follow. |
| https://arxiv.org/abs/2402.01781 | Alzahrani et al. | always, mean that the model is doing better than random. |
| https://oreil.ly/hOlhJ | GPT-2 | 2 A reason that OpenAI’s GPT-2 created so much buzz in 2019 was that it was able to generate texts that were remarkably more fluent and more coherent than any l |
| https://arxiv.org/abs/2203.05227 | Li et al., 2022 | might use is faithfulness: how faithful is the generated translation to the original sen‐ tence? |
| https://oreil.ly/hJucg | Wan et al. (2024) | One interesting research question is what evidence AI models find convincing, as the answer sheds light on how AI models process conflicting information and det |
| https://oreil.ly/HnIVp | Liu et al. (2023) | retrieval is discussed in Chapter 6). |
| https://arxiv.org/abs/2303.15621 | Luo et al. (2023) | retrieval is discussed in Chapter 6). |
| https://oreil.ly/xvYjL | “TruthfulQA: Measuring How Models Mimic Human Falsehoods” | including factual consistency. |
| https://arxiv.org/abs/2303.08896 | Manakul et al., 2023 | Given a response R to evaluate, SelfCheckGPT generates N new responses and measures how consistent R is with respect to these N new |
| https://arxiv.org/abs/2403.18802 | “Long-Form Factuality in Large Language Mod‐ | It works in four steps, as visualized in Figure 4-1: 1. |
| https://oreil.ly/ICHH3 | DeBERTa-v3-base-mnli-fever-anli | For example, DeBERTa-v3-base-mnli-fever-anli is a 184-million-parameter model trained on 764,000 annotated (hypothesis, premise) pairs to predict entailment. |
| https://oreil.ly/PSNna | GPT-4’s technical report | Figure 4-2 shows the performance of several models on this benchmark, as shown in GPT-4’s technical report (2023). |
| https://oreil.ly/ZRwVI | content moderation | Different safety solutions have different ways of categorizing harms—see the taxonomy defined in OpenAI’s content moderation endpoint and Meta’s Llama Guard pap |
| https://arxiv.org/abs/2312.06674 | Inan et al., 2023 | Different safety solutions have different ways of categorizing harms—see the taxonomy defined in OpenAI’s content moderation endpoint and Meta’s Llama Guard pap |
| https://oreil.ly/AB2FU | tutorial | Evaluation Criteria \| 171 |
| https://arxiv.org/abs/2305.08283 | Feng et | For example, studies (Feng et al., 2023; Motoki et al., 2023; and Hartman et al., 2023) have shown that models, depending on their training, can be imbued with |
| https://oreil.ly/u9_vA | Motoki et al., 2023 | For example, studies (Feng et al., 2023; Motoki et al., 2023; and Hartman et al., 2023) have shown that models, depending on their training, can be imbued with |
| https://arxiv.org/abs/2301.01768 | Hartman et al., 2023 | For example, studies (Feng et al., 2023; Motoki et al., 2023; and Hartman et al., 2023) have shown that models, depending on their training, can be imbued with |
| https://oreil.ly/BndEu | Facebook’s hate speech detection model | Examples of these models are Facebook’s hate speech detection model, the Skolkovo Institute’s toxicity classifier, and Perspective API. |
| https://oreil.ly/2aIvB | Skolkovo Institute’s toxicity | Examples of these models are Facebook’s hate speech detection model, the Skolkovo Institute’s toxicity classifier, and Perspective API. |
| https://oreil.ly/0VrKU | Perspective API | Examples of these models are Facebook’s hate speech detection model, the Skolkovo Institute’s toxicity classifier, and Perspective API. |
| https://oreil.ly/70VH1 | Danish | There are also many toxicity and hate speech detec‐ tion models specialized in different languages, such as Danish and Vietnamese. |
| https://arxiv.org/abs/2102.12162 | Vietnamese | There are also many toxicity and hate speech detec‐ tion models specialized in different languages, such as Danish and Vietnamese. |
| https://oreil.ly/Bfa4q | Gehman et | Common benchmarks to measure toxicity include RealToxicityPrompts (Gehman et al., 2020) and BOLD (bias in open-ended language generation dataset) (Dhamala et al |
| https://oreil.ly/aFvUh | Dhamala et | Common benchmarks to measure toxicity include RealToxicityPrompts (Gehman et al., 2020) and BOLD (bias in open-ended language generation dataset) (Dhamala et al |
| https://arxiv.org/abs/2311.07911 | IFEval | Instruction-following criteria Different benchmarks have different notions of what instruction-following capability encapsulates. |
| https://oreil.ly/SaIST | INFOBench | Instruction-following criteria Different benchmarks have different notions of what instruction-following capability encapsulates. |
| https://arxiv.org/abs/2309.11998 | LMSYS published a study | LMSYS published a study of one million conversations on Chatbot Arena, but these conver‐ sations aren’t grounded in real-world applications. |
| https://arxiv.org/abs/2310.00746 | Wang et al., 2023 | Benchmarks to evaluate roleplaying capability include RoleLLM (Wang et al., 2023) and CharacterEval (Tu et al., 2024). |
| https://arxiv.org/abs/2401.01275 | Tu et | Benchmarks to evaluate roleplaying capability include RoleLLM (Wang et al., 2023) and CharacterEval (Tu et al., 2024). |
| https://en.wikipedia.org/wiki/Multi-objective_optimization | Pareto optimiza‐ | Optimizing for multiple objectives is an active field of study called Pareto optimiza‐ tion. |
| https://oreil.ly/XaRwG | Elastic License | 11 In spirit, this restriction is similar to the Elastic License that forbids companies from offering the open source version of Elastic as a hosted service and |
| https://oreil.ly/wRlEh | Llama 2 Community License Agreement | For example, Meta released Llama 2 under the Llama 2 Community License Agreement and Llama 3 under the Llama 3 Community License Agreement. |
| https://oreil.ly/FL-1Z | Llama 3 Community License Agreement | For example, Meta released Llama 2 under the Llama 2 Community License Agreement and Llama 3 under the Llama 3 Community License Agreement. |
| https://oreil.ly/yED-R | BigCode Open RAIL-M v1 | Hugging Face released their model Big‐ Code under the BigCode Open RAIL-M v1 license. |
| https://github.com/google-deepmind/gemma/blob/main/LICENSE | Google’s Gemma | Both Google’s Gemma and Mistral-7B were released under Apache 2.0. |
| https://oreil.ly/uTBwP | Mistral-7B | Both Google’s Gemma and Mistral-7B were released under Apache 2.0. |
| https://oreil.ly/V1P8X | noncommercial license | When Meta’s first Llama model was released, it was under a noncommercial license. |
| https://x.com/arthurmensch/status/1734470462451732839 | license | Mistral didn’t allow this originally but later changed its license. |
| https://oreil.ly/mlHyX | “Samsung Workers Made a Major Error | For these companies, the data privacy policy is more about what services they can trust. |
| https://oreil.ly/fWs9H | Samsung to ban | However, the incident was serious enough for Samsung to ban ChatGPT in May 2023. |
| https://oreil.ly/xndQu | Zoom faced a backlash | If you use a model API, there’a risk that the API provider will use your data to train its models. |
| https://x.com/dhuynh95/status/1713917852162424915 | Hugging Face’s | For example, it’s been found that Hugging Face’s StarCoder model memorizes 8% of its training set. |
| https://oreil.ly/AhHI_ | Gemini’s technical report | For most models, there’s little transparency about what data a model is trained on. |
| https://x.com/JoannaStern/status/1768306032466428291 | OpenAI’s CTO | OpenAI’s CTO wasn’t able to provide a satisfactory answer when asked what data was used to train their models. |
| https://oreil.ly/p23MQ | US Patent and | workers are paid at least a local living wage . |
| https://oreil.ly/-qEXt | hesitant to use AI | use this model to create your product, you can defend your product’s IP. |
| https://oreil.ly/Zj1GZ | 2024 study by a16z | A 2024 study by a16z shows two key reasons that enterprises care about open source models are control and customizability, as shown in Figure 4-8. |
| https://convai.com | Convai | A company I advise, Convai, builds 3D AI characters that can interact in 3D environments, including picking up objects. |
| https://oreil.ly/pY1FF | Italy briefly banned OpenAI in 2023 | A model provider can also go out of business altogether. |
| https://github.com/google/BIG-bench/blob/main/bigbench/benchmark_tasks/README.md | Google’s BIG-bench (2022) | Google’s BIG-bench (2022) alone has 214 benchmarks. |
| https://github.com/openai/evals | OpenAI’s evals | OpenAI’s evals lets you run any of the approximately 500 existing benchmarks and register new benchmarks to evaluate OpenAI models. |
| https://oreil.ly/7PFUy | expensive to run | For example, HELM (Holistic Evaluation of Language Mod‐ els) Lite left out an information retrieval benchmark (MS MARCO, Microsoft Machine Reading Comprehension |
| https://oreil.ly/pgGZ0 | large compute requirements | Hugging Face opted out of HumanEval due to its large compute requirements—you need to generate a lot of completions. |
| https://oreil.ly/-uhru | Hugging Face first launched Open LLM Leaderboard in 2023 | When Hugging Face first launched Open LLM Leaderboard in 2023, it consisted of four benchmarks. |
| https://arxiv.org/abs/1803.05457 | Clark et al., 2018 | ARC-C (Clark et al., 2018): Measuring the ability to solve complex, grade school- level science questions. |
| https://arxiv.org/abs/1905.07830 | Zellers et al., 2019 | HellaSwag (Zellers et al., 2019): Measuring the ability to predict the completion of a sentence or a scene in a story or video. |
| https://arxiv.org/abs/2109.07958 | Lin et al., 2021 | TruthfulQA (Lin et al., 2021): Measuring the ability to generate responses that are not only accurate but also truthful and non-misleading, focusing on a model’ |
| https://arxiv.org/abs/1907.10641 | Sakaguchi et al., 2019 | are not only accurate but also truthful and non-misleading, focusing on a model’s understanding of facts. |
| https://oreil.ly/eH7Ho | responded | Thanks to the Hugging Face team for being so wonderfully responsive and for their great contributions to the community. |
| https://github.com/openai/grade-school-math | Grade School Math, OpenAI, 2021 | GSM-8K (Grade School Math, OpenAI, 2021): Measuring the ability to solve a diverse set of math problems typically encountered in grade school curricula. |
| https://oreil.ly/CQ52G | Stanford’s HELM Leaderboard | At around the same time, Stanford’s HELM Leaderboard used ten benchmarks, only two of which (MMLU and GSM-8K) were in the Hugging Face leaderboard. |
| https://arxiv.org/abs/2103.03874 | MATH | gg g other eight benchmarks are: • A benchmark for competitive math (MATH) • One each for legal (LegalBench), medical (MedQA), and translation (WMT 2014) • Two |
| https://oreil.ly/jCo7o | LegalBench | • A benchmark for competitive math (MATH) • One each for legal (LegalBench), medical (MedQA), and translation (WMT 2014) • Two for reading comprehension—answeri |
| https://arxiv.org/abs/2009.13081 | MedQA | • A benchmark for competitive math (MATH) • One each for legal (LegalBench), medical (MedQA), and translation (WMT 2014) • Two for reading comprehension—answeri |
| https://oreil.ly/bdGKm | WMT | • A benchmark for competitive math (MATH) • One each for legal (LegalBench), medical (MedQA), and translation (WMT 2014) • Two for reading comprehension—answeri |
| https://arxiv.org/abs/1712.07040 | NarrativeQA | 2014) • Two for reading comprehension—answering questions based on a book or a long story (NarrativeQA and OpenBookQA) • Two for general question answering (Nat |
| https://arxiv.org/abs/1809.02789 | OpenBookQA | 2014) • Two for reading comprehension—answering questions based on a book or a long story (NarrativeQA and OpenBookQA) • Two for general question answering (Nat |
| https://oreil.ly/QB4XP | Natural Questions | • Two for reading comprehension—answering questions based on a book or a long story (NarrativeQA and OpenBookQA) • Two for general question answering (Natural Q |
| https://oreil.ly/4X6Dm | a great analysis | When launching their new leaderboard, Hugging Face shared a great analysis of the benchmarks correlation (2024). |
| https://x.com/polynoamial/status/1803812369237528825 | GSM-8K was replaced by MATH lvl 5 | For example, GSM-8K was replaced by MATH lvl 5, which consists of the most challenging questions from the competitive math benchmark MATH. |
| https://arxiv.org/abs/2311.12022 | Rein et al., 2023 | MMLU was replaced by MMLU PRO (Wang et al., 2024). |
| https://arxiv.org/abs/2310.16049 | Sprague et al., 2023 | • GPQA (Rein et al., 2023): a graduate-level Q&A benchmark22 • MuSR (Sprague et al., 2023): a chain-of-thought, multistep reasoning benchmark • BBH (BIG-bench H |
| https://arxiv.org/abs/2206.04615 | Srivastava et al., 2023 | • MuSR (Sprague et al., 2023): a chain-of-thought, multistep reasoning benchmark • BBH (BIG-bench Hard) (Srivastava et al., 2023): another rea‐ soning benchmark |
| https://x.com/gblazex | Balázs Galambosi | Table 4-5 shows the Pearson correlation scores among the six benchmarks used on Hugging Face’s leaderboard, computed in January 2024 by Balázs Galambosi. |
| https://oreil.ly/MLlDD | HELM authors | even if, for some tasks, truthfulness might weigh a lot more than being able to solve grade school math problems. |
| https://oreil.ly/4c6in | a 10% drop | For exam‐ ple, migrating from GPT-3.5-turbo-0301 to GPT-3.5-turbo-1106 led to a 10% drop in Voiceflow’s intent classification task but an improvement in GoDaddy |
| https://oreil.ly/V48iM | improvement | For exam‐ ple, migrating from GPT-3.5-turbo-0301 to GPT-3.5-turbo-1106 led to a 10% drop in Voiceflow’s intent classification task but an improvement in GoDaddy |
| https://arxiv.org/abs/2307.09009 | Chen et al., 2023 | Are OpenAI s Models Getting Worse? |
| https://arxiv.org/abs/2211.09110 | full HELM suite | to run the evaluation yourself.25 Hopefully, an evaluation harness can help you with that. |
| https://arxiv.org/abs/2309.08632 | “Pretraining on the Test Set Is All You Need” | Rylan Schaeffer, a PhD student at Stanford, demonstrated this beautifully in his 2023 satirical paper “Pretraining on the Test Set Is All You Need”. |
| https://oreil.ly/LghFT | to spot outliers | To combat data contamination, leaderboard hosts like Hugging Face plot standard deviations of models’ performance on a given benchmark to spot outliers. |
| https://github.com/google/BIG-bench/blob/main/bigbench/benchmark_tasks/twenty_questions/README.md | BIG- | Here’s an example of a plausible conversation in this task, taken from the BIG- bench’s GitHub repository: Bob: Is the concept an animal? |
| https://www.linkedin.com/feed/update/urn:li:activity:7189260630053261313/ | LinkedIn | In retrospect of one year of deploying generative AI applications, LinkedIn shared that the first hurdle was in creating an evaluation guideline. |
| https://oreil.ly/d1ey3 | Lang‐ | Lang‐ Chain’s State of AI 2023 found that, on average, their users used 2.3 different types of feedback (criteria) to evaluate an application. |
| https://oreil.ly/J3pbA | Designing Machine | I wrote at length about slice-based evaluation in Designing Machine Learning Systems (O’Reilly), so here, I’ll just go over the key points. |
| https://en.wikipedia.org/wiki/Simpson's_paradox | Simpson’s paradox | • Avoid falling for Simpson’s paradox, a phenomenon in which model A performs better than model B on aggregated data but worse than model B on every subset of d |
| https://oreil.ly/9Ku73 | “Comparison of | Model A 93% (81/87) 73% (192/263) 78% (273/350) Model B 87% (234/270) 69% (55/80) 83% (289/350) a I also used this example in Designing Machine Learning Systems |
| https://oreil.ly/xAbHm | OpenAI | OpenAI suggested a rough estimation of the number of evaluation samples needed to be certain that one system is better, given a score difference, as shown in Ta |
| https://oreil.ly/Ek0wH | Inverse Scaling prize | As a reference, among evaluation benchmarks in Eleuther’s lm-evaluation-harness, the median number of examples is 1,000, and the average is 2,159. |
