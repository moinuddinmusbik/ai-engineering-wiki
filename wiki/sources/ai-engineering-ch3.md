---
title: "AI Engineering — Ch.3: Evaluation Methodology"
type: source
created: 2026-04-10
updated: 2026-04-10
tags: [evaluation, metrics, ai-judge, perplexity, comparative-evaluation]
sources: [ai-engineering.pdf]
---

# AI Engineering — Ch.3: Evaluation Methodology

**Source:** *AI Engineering* by [[Chip Huyen]], O'Reilly 2024, Chapter 3 (pp. 137–180).

## Summary

Chapter 3 tackles the methodology of evaluating foundation models — a task that has become the biggest bottleneck to bringing AI applications to production. Huyen opens with a stark framing: without systematic evaluation, the risks of AI may outweigh its benefits, as illustrated by real-world failures (AI-encouraged suicide, hallucinated legal evidence, Air Canada chatbot damages).

The chapter begins with **language modeling metrics** — [[Perplexity]], cross entropy, bits-per-character (BPC), and bits-per-byte (BPB) — which remain the foundational measures for guiding training and finetuning. These four metrics are interconvertible and measure how well a model predicts its training data. Perplexity has useful secondary applications: detecting data contamination, identifying abnormal texts, and serving as a proxy for downstream capability.

The core of the chapter is a taxonomy of evaluation approaches split into **exact evaluation** and **subjective evaluation**. [[Exact Evaluation]] covers functional correctness (e.g., pass@k for code generation via HumanEval/MBPP) and similarity measurements against reference data — exact match, lexical similarity (BLEU, ROUGE), and semantic similarity via [[Embeddings]] and cosine distance. An important aside introduces embeddings in depth: BERT, CLIP, joint multimodal embedding spaces, and their role in semantic textual similarity metrics like BERTScore.

The chapter then pivots to [[AI as a Judge]] — the rising star of subjective evaluation. AI judges are fast, flexible, and relatively cheap; studies show GPT-4's agreement with humans (85%) exceeds inter-human agreement (81%). Huyen covers how to prompt AI judges (scoring systems, rubrics, examples), then catalogs their limitations: **inconsistency** (probabilistic outputs), **criteria ambiguity** (no standardization across tools), **increased costs and latency**, and **biases** (self-bias, first-position bias, verbosity bias). She surveys specialized judges: reward models (Cappy), reference-based judges (BLEURT, Prometheus), and preference models (PandaLM, JudgeLM).

The chapter closes with [[Comparative Evaluation]] — ranking models via head-to-head matchups rather than absolute scores. Inspired by game rating systems, this approach uses algorithms like Elo and Bradley–Terry. LMSYS's Chatbot Arena is the premier example. Huyen discusses scalability bottlenecks (quadratic model pairs), lack of standardization in crowdsourced comparisons, and the fundamental limitation that comparative evaluation reveals relative but not absolute performance.

## Key Takeaways

- **Evaluation is the biggest bottleneck** to AI adoption; investment in evaluation infrastructure lags far behind modeling and training.
- **Perplexity** is the default language modeling metric; lower = better prediction. Useful beyond training: data contamination detection, anomaly detection, deduplication.
- **Exact evaluation** (functional correctness + similarity measurements) produces unambiguous scores but requires reference data and works best for close-ended tasks.
- **BLEU/ROUGE** scores are declining in use since foundation models; higher lexical similarity doesn't always mean better responses (OpenAI found BLEU scores similar for correct and incorrect code).
- **Embeddings** are the backbone of semantic similarity; CLIP pioneered joint text–image embedding spaces.
- **AI as a judge** is the most common evaluation method in production (58% of evaluations on LangChain's platform). A judge = model + prompt; changing either changes the judge.
- **AI judge biases**: self-bias (GPT-4 favors itself +10%, Claude-v1 +25%), first-position bias, verbosity bias (preferring longer responses even with factual errors).
- **Comparative evaluation** captures human preference and is harder to game than benchmarks, but doesn't tell you if a model is *good enough*, only if it's *better*.

## Notable Quotes

> "Evals are surprisingly often all you need." — Greg Brockman, OpenAI (December 2023)

> "Do not trust any AI judge if you can't see the model and the prompt used for the judge."

> "A benchmark stops being useful as soon as it becomes public."

## Pages Created or Updated

- **Concepts:** [[Perplexity]], [[Exact Evaluation]], [[Embeddings]], [[AI as a Judge]], [[Comparative Evaluation]]
- **Entities:** [[LMSYS]]
- **Updated:** [[Hallucination]], [[Foundation Models]]

---

## References (76 links)

All hyperlinks embedded by the author in this chapter, extracted from the book PDF. See also the [complete URL reference](../analyses/ai-engineering-urls.md) across all chapters.


| URL | Name / Link Text | Description |
|-----|-----------------|-------------|
| https://x.com/gdb/status/1733553161884127435 | tweeted | 1 In December 2023, Greg Brockman, an OpenAI cofounder, tweeted that “evals are surprisingly often all you need.” 113 |
| https://oreil.ly/tMH21 | encouraged by a chatbot | A man committed suicide after being encouraged by a chatbot. |
| https://oreil.ly/-0Iq1 | false evidence hallucinated by AI | Lawyers sub‐ mitted false evidence hallucinated by AI. |
| https://oreil.ly/kKWnZ | gave a passenger false information | Air Canada was ordered to pay damages when its AI chatbot gave a passenger false information. |
| https://oreil.ly/fti6d | a16z | 4 When OpenAI’s GPT-o1 came out in September 2024, the Fields medalist Terrence Tao compared the expe‐ rience of working with this model to working with “a medi |
| https://oreil.ly/4KJQM | Fields medalist Terrence Tao | y 3 Also known as vibe check. |
| https://arxiv.org/abs/1804.07461 | GLUE | The benchmark GLUE (General Language Understanding Evaluation) came out in 2018 and became saturated in just a year, necessitating the introduction of Super‐ GL |
| https://arxiv.org/abs/1905.00537 | Super‐ | The benchmark GLUE (General Language Understanding Evaluation) came out in 2018 and became saturated in just a year, necessitating the introduction of Super‐ GL |
| https://arxiv.org/abs/2104.08773 | NaturalInstructions | Similarly, NaturalInstructions (2021) was replaced by Super- NaturalInstructions (2022). |
| https://arxiv.org/abs/2406.01574 | MMLU-Pro | MMLU (2020), a strong benchmark that many early foundation models relied on, was largely replaced by MMLU-Pro (2024). |
| https://arxiv.org/abs/2307.03109 | Chang et al. | Image from Chang et al. |
| https://arxiv.org/abs/1806.02643 | Balduzzi et al. from Deep‐ | the growth curve looks exponential, as shown in Figure 3 2. |
| https://oreil.ly/gPbjS | Anthropic | attention compared to developing algorithms.” According to the paper, experiment results are almost exclusively used to improve algorithms and are rarely used t |
| https://oreil.ly/vX-My | Liu et al., 2023 | For these models, the performance of the language model component tends to be well correlated to the foundation model’s performance on downstream applications ( |
| https://oreil.ly/HjUlH | his own words | Here’s entropy in his own words: “The entropy is a statistical parameter which measures, in a certain sense, how much information is produced on the average for |
| https://en.wikipedia.org/wiki/E_(mathematical_constant) | base of e | Nat uses the base of e, the base of natural log‐ arithm.8 If you use nat as the unit, perplexity is the exponential of e: PPL (P, Q) = e H (P,Q) |
| https://oreil.ly/Loidb | OpenAI, 2018 | Source: OpenAI, 2018. |
| https://en.wikipedia.org/wiki/Unit_testing | unit tests | Code is typically validated with unit tests where code is executed in different scenarios to ensure that it gener‐ ates the expected outputs. |
| https://oreil.ly/CjYs9 | OpenAI’s HumanEval | Popular benchmarks for evaluating AI’s code generation capabilities, such as OpenAI’s HumanEval and Google’s MBPP (Mostly Basic Python Problems Dataset) use fun |
| https://github.com/google-research/google-research/tree/master/mbpp | Google’s MBPP | Popular benchmarks for evaluating AI’s code generation capabilities, such as OpenAI’s HumanEval and Google’s MBPP (Mostly Basic Python Problems Dataset) use fun |
| https://oreil.ly/ijU20 | Yu et al., 2018 | OpenAI’s HumanEval and Google’s MBPP (Mostly Basic Python Problems Dataset) use functional correctness as their metrics. |
| https://oreil.ly/rrSS9 | Li et al., 2023 | use functional correctness as their metrics. |
| https://arxiv.org/abs/1709.00103 | Zhong, et al., 2017 | A benchmark problem comes with a set of test cases. |
| https://oreil.ly/92yRh | WMT | Examples of benchmarks that use these metrics are WMT, COCO Cap‐ tions, and GEMv2. |
| https://oreil.ly/BO3-0 | COCO Cap‐ | Examples of benchmarks that use these metrics are WMT, COCO Cap‐ tions, and GEMv2. |
| https://arxiv.org/abs/2206.11249 | GEMv2 | Examples of benchmarks that use these metrics are WMT, COCO Cap‐ tions, and GEMv2. |
| https://oreil.ly/OWD2v | Adept | On some benchmark examples, Adept found that its model Fuyu performed poorly not because the model’s outputs were wrong, but because some correct answers were m |
| https://oreil.ly/tmWqk | Freitag et al., 2023 | Low-quality reference data is one of the reasons that reference-free metrics were strong contenders for reference-based metrics in terms of correlation to human |
| https://arxiv.org/abs/2107.03374 | Chen et al., 2021 | This indicates that optimizing for BLEU scores isn’t the same as optimizing for functional correctness (Chen et al., 2021). |
| https://arxiv.org/abs/1904.09675 | BERTScore | Semantic textual similarity doesn’t require a set of reference responses as comprehen‐ |
| https://oreil.ly/v2ENK | MoverScore | \| \| \| \| Metrics for semantic textual similarity include BERTScore (embeddings are gener‐ ated by BERT) and MoverScore (embeddings are generated by a mixture of |
| https://arxiv.org/abs/1301.3781 | “Efficient Estimation of Word Representations in Vector Space” | 14 There are also models that generate word embeddings, as opposed to documentation embeddings, such as word2vec (Mikolov et al., “Efficient Estimation of Word |
| https://oreil.ly/O5QTX | “GloVe: Global Vectors for Word Representation” | 14 There are also models that generate word embeddings, as opposed to documentation embeddings, such as word2vec (Mikolov et al., “Efficient Estimation of Word |
| https://github.com/UKPLab/sentence-transformers | Sentence Transform‐ | Models trained especially to produce embeddings include the open source models BERT, CLIP (Contrastive Language–Image Pre-training), and Sentence Transform‐ ers |
| https://oreil.ly/0Cfcw | OpenAI’s CLIP | Google’s BERT BERT base: 768 BERT large: 1024 OpenAI’s CLIP Image: 512 Text: 512 OpenAI Embeddings API text-embedding-3-small: 1536 text-embedding-3-large: 3072 |
| https://oreil.ly/SBUiU | OpenAI Embeddings API | OpenAI’s CLIP Image: 512 Text: 512 OpenAI Embeddings API text-embedding-3-small: 1536 text-embedding-3-large: 3072 Cohere’s Embed v3 embed-english-v3.0: 1024 em |
| https://oreil.ly/BNNNm | Cohere’s Embed v3 | OpenAI Embeddings API text-embedding-3-small: 1536 text-embedding-3-large: 3072 Cohere’s Embed v3 embed-english-v3.0: 1024 embed-english-light-3.0: 384 Because |
| https://arxiv.org/abs/2210.07316 | Muennigh‐ | An example of benchmarks that measure embedding quality on multiple tasks is MTEB, Massive Text Embedding Benchmark (Muennigh‐ off et al., 2023). |
| https://arxiv.org/abs/1607.07326 | Criteo | For exam‐ ple, ecommerce solutions like Criteo and Coveo have embeddings for products. |
| https://oreil.ly/a6jbV | Coveo | For exam‐ ple, ecommerce solutions like Criteo and Coveo have embeddings for products. |
| https://oreil.ly/uJNFH | Pin‐ | Pin‐ terest has embeddings for images, graphs, queries, and even users. |
| https://arxiv.org/abs/2103.00020 | Radford et al., 2021 | CLIP (Radford et al., 2021) was one of the first major models that could map data of differ‐ ent modalities, text and images, into a joint embedding space. |
| https://arxiv.org/abs/2212.05171 | Xue et al., 2022 | ULIP (unified repre‐ sentation of language, images, and point clouds), (Xue et al., 2022) aims to create unified representations of text, images, and 3D point c |
| https://arxiv.org/abs/2305.05665 | Girdhar et | ImageBind (Girdhar et al., 2023) learns a joint embedding across six different modalities, including text, images, and audio. |
| https://oreil.ly/7Fkh- | LangChain’s State of AI | LangChain’s State of AI report in 2023 noted that 58% of evaluations on their platform were done by AI judges. |
| https://arxiv.org/abs/2306.05685 | Zheng et al. | In 2023, Zheng et al. |
| https://arxiv.org/abs/2404.04475 | Dubois et al., 2023 | AlpacaEval authors (Dubois et al., 2023) also found that their AI judges have a near perfect (0.98) correlation with LMSYS’s Chat Arena leaderboard, which is ev |
| https://oreil.ly/57jOL | Azure AI Studio | Note that as these tools evolve, these built in criteria will change. |
| https://oreil.ly/2oEO1 | MLflow.metrics | AI Tools Built-in criteria Azure AI Studio Groundedness, relevance, coherence, fluency, similarity MLflow.metrics Faithfulness, relevance LangChain Criteria Eva |
| https://oreil.ly/R1sCz | LangChain Criteria Evaluation | Azure AI Studio Groundedness, relevance, coherence, fluency, similarity MLflow.metrics Faithfulness, relevance LangChain Criteria Evaluation Conciseness, releva |
| https://oreil.ly/5T3ey | Ragas | Azure AI Studio’s relevance scores might be very different from MLflow’s relevance scores. |
| https://oreil.ly/Hlkax | relevance | Here’s part of the prompt used for the criteria relevance by Azure AI Studio. |
| https://github.com/mlflow/mlflow/blob/5cdae7c4321015620032d02a3b84fb6127247392/mlflow/metrics/genai/prompts/v1.py | MLflow | Faithfulness assesses how much of the provided output is factually consistent with the provided con text 1–5 |
| https://github.com/explodinggradients/ragas/blob/b276f59c0d4eb4795dc28966bfbce14d5aacd140/src/ragas/metrics/_faithfulness.py#L93C1-L94C1 | Ragas | - Score 2: … Ragas Your task is to judge the faithfulness of a series of state ments based on a given context. |
| https://github.com/run-llama/llama_index/blob/main/llama-index-core/llama_index/core/evaluation/faithfulness.py | LlamaIndex | Tool Prompt [partially omitted for brevity] Scoring system LlamaIndex Please tell if a given piece of information is supported by the context. |
| https://oreil.ly/2XDI0 | the answer they see last, | Humans tend to favor the answer they see last, which is called recency bias. |
| https://arxiv.org/abs/2307.03025 | Wu and Aji (2023) | Wu and Aji (2023) found that both GPT-4 and Claude-1 prefer longer responses (~100 words) with factual errors over shorter, correct responses (~50 words). |
| https://oreil.ly/IOp9H | Saito et al. (2023) | (2023) and Saito et al. |
| https://arxiv.org/abs/2210.03350 | Press et al., 2022 | If a model thinks its own response is incorrect, the model might not be that reliable. |
| https://arxiv.org/abs/2305.11738 | Gou et al., 2023 | If a model thinks its own response is incorrect, the model might not be that reliable. |
| https://arxiv.org/abs/2310.08118 | Valmeekamet et | If a model thinks its own response is incorrect, the model might not be that reliable. |
| https://github.com/google-research/bleurt/issues/1 | between -2.5 and 1.0 | It’s approximately between -2.5 and 1.0. |
| https://arxiv.org/abs/2311.06720 | Cappy | Cappy is an example of a reward model developed by Google (2023). |
| https://arxiv.org/abs/2004.04696 | Sellam et al., 2020 | more reference responses. |
| https://arxiv.org/abs/2310.08491 | Kim et al., 2023 | For example, BLEURT (Sellam et al., 2020) takes in a (candidate response, refer‐ ence response) pair and outputs a similarity score between the candidate and re |
| https://arxiv.org/abs/2306.05087 | Wang et al., 2023 | There have been many initiatives in building preference models, including PandaLM (Wang et al., 2023) and JudgeLM (Zhu et al., 2023). |
| https://arxiv.org/abs/2310.17631 | Zhu et al., 2023 | There have been many initiatives in building preference models, including PandaLM (Wang et al., 2023) and JudgeLM (Zhu et al., 2023). |
| https://en.wikipedia.org/wiki/Likert_scale | Likert scale | 148 \| Chapter 3: Evaluation Methodology |
| https://arxiv.org/abs/2112.00861 | Anthropic | In AI, comparative evaluation was first used in 2021 by Anthropic to rank different models. |
| https://oreil.ly/MHt5H | Chatbot Arena | It also powers the popular LMSYS’s Chatbot Arena leaderboard that ranks models using scores computed from pairwise model comparisons from the commu‐ nity. |
| https://arxiv.org/abs/2311.17295 | Boubdir et al. | Many papers that analyze Elo for AI evaluation cite transitivity assumption as a limitation (Boubdir et al.; Balduzzi et al.; and Munos et al.). |
| https://arxiv.org/abs/2312.00886 | Munos et al. | Many papers that analyze Elo for AI evaluation cite transitivity assumption as a limitation (Boubdir et al.; Balduzzi et al.; and Munos et al.). |
| https://oreil.ly/td_MY | the website | Anyone can go to the website, enter a prompt, get back two responses from two anonymous models, and vote for the better one. |
| https://oreil.ly/eI9Vq | 33,000 prompts | There are many brainteasers. |
| https://x.com/lmarena_ai/status/1792625968865026427 | hard | LMSYS instead lets users use any prompts but then filter out hard prompts using their internal model and rank models using only these hard prompts. |
| https://oreil.ly/kIJ9F | their | This is the approach that Scale uses with their private comparative leaderboard. |
