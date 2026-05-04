---
title: "AI Engineering — Ch.8: Dataset Engineering"
type: source
created: 2026-04-27
updated: 2026-04-27
tags: [dataset-engineering, data-synthesis, data-curation, model-distillation, data-flywheel]
sources: [ai-engineering.pdf]
---

# AI Engineering — Ch.8: Dataset Engineering

**Book:** *AI Engineering* by Chip Huyen (O'Reilly, 2024)
**Chapter:** 8 — Dataset Engineering
**Pages:** ~387–429

---

## Summary

Chapter 8 makes the case for **data-centric AI**: as models converge in capability, the quality and diversity of training data becomes the decisive differentiator. The chapter covers the full data engineering pipeline — from defining quality criteria and sourcing data, through augmentation and synthesis, to model distillation as a form of data generation.

Data quality is defined along six criteria: relevant, aligned (matching intended behavior), consistent (no contradictions), correctly formatted, sufficiently unique (no excessive duplicates), and compliant (license/privacy). Coverage and diversity are treated as orthogonal concerns: a dataset can be high quality per-example but systematically miss entire topic areas or demographic groups.

The chapter details the data mix used for Llama 3 across training phases: general knowledge dominates at all stages (50% in pre-training, 52.66% in SFT, 81.99% in preference fine-tuning), reflecting the model's emphasis on broad capability over narrow specialization.

A significant portion covers **AI-powered data synthesis** — using models to generate training data for themselves or other models. Key techniques include: generating instruction datasets (UltraChat's topics→subtopics→instructions pipeline), reverse instruction generation (generate responses first, then derive instructions), and the Alpaca approach (175 seed examples → 52K instruction-response pairs via GPT-3). The Llama 3 coding synthesis pipeline is particularly detailed: generate problem descriptions → solutions → unit tests → fix errors → translate to other languages → back-translation verification → 2.7M examples.

**Model distillation** is framed as a data generation strategy: the teacher model's outputs (or soft probability distributions) become training signal for a smaller student. LIMA demonstrates that 1,000 carefully curated examples can approach GPT-4-level quality in 43% of head-to-head comparisons, suggesting data quality often matters more than quantity.

The chapter honestly addresses limitations of synthetic data: **model collapse** (Shumailov et al. 2023) — iteratively training on model-generated data degrades performance as error compounds; superficial imitation (Alpaca learns GPT-3's style, not its reasoning); quality control challenges; and obscured data lineage.

---

## Key Takeaways

- **Data-centric AI**: model improvements plateau; data quality is the remaining lever. "Garbage in, garbage out" applies at every training phase.
- **Six quality criteria**: relevant, aligned, consistent, correctly formatted, sufficiently unique, compliant.
- **Coverage / diversity**: separate from per-example quality. Systematic gaps (topics, demographics, languages) undermine model capability even with high per-example quality.
- **Data flywheel**: application deployment generates real user data — the ideal training source. Closing this loop is a major competitive advantage.
- **Data augmentation**: word replacement, perturbation, back-translation, paraphrasing — cheap diversity expansion for small datasets.
- **AI-powered synthesis**: instruction generation, reverse instruction, self-play, Alpaca bootstrapping. Can scale to millions of examples from hundreds of seeds.
- **Llama 3 coding pipeline**: 2.7M synthetic coding examples generated via a 6-step pipeline including unit test generation and back-translation verification.
- **LIMA result**: 1,000 curated examples → competitive with GPT-4 in 43% of cases. Alignment may be "superficial" — easily taught with small high-quality data.
- **Model distillation**: teacher model generates training data (outputs or soft distributions) for student. DistilBERT: 40% smaller, 97% of teacher capability.
- **Model collapse risk**: repeatedly training on synthetic data degrades performance. Real data remains essential as an anchor.

---

## Notable Quotes

> "LIMA showed that 1,000 carefully curated examples can approach GPT-4 quality in 43% of head-to-head comparisons."

> "The data flywheel refers to the virtuous cycle where deploying an application generates real user data, which improves training, which improves the application."

---

## Pages Created / Updated

**Created:**
- [[Dataset Engineering]] — data quality criteria, coverage, acquisition, the data flywheel
- [[Data Synthesis]] — augmentation, rule-based synthesis, AI-powered synthesis, model distillation, model collapse

**Updated:**
- [[Finetuning]] — added dataset engineering context

---

## References (92 links)

All hyperlinks embedded by the author in this chapter, extracted from the book PDF. See also the [complete URL reference](../analyses/ai-engineering-urls.md) across all chapters.


| URL | Name / Link Text | Description |
|-----|-----------------|-------------|
| https://oreil.ly/R4-VI | OpenAI, 2020 | In the con‐ tribution list for GPT-3 (OpenAI, 2020), only two people were credited with data collecting, filtering, and deduplicating, and conducting overlap an |
| https://oreil.ly/F9Fyc | OpenAI, 2023 | For GPT-4 (OpenAI, 2023), eighty people were credited for being involved in different data processes. |
| https://oreil.ly/h-lAl | 2016 AMA (ask me anything) thread | Back in their 2016 AMA (ask me anything) thread, Wojciech Zaremba, one of OpenAI’s cofounders, said that they intended to conduct most of their research using p |
| https://oreil.ly/2JlmX | data-centric AI competition | In recent years, more benchmarks have become data-centric. |
| https://arxiv.org/abs/2304.14108 | Gadre et al., 2023 | In 2023, DataComp (Gadre et al., 2023) hosted a competition whose goal was to cre‐ ate the best dataset for training a CLIP model (Radford et al., 2021). |
| https://oreil.ly/Xe50R | competition | In 2023, DataComp (Gadre et al., 2023) hosted a competition whose goal was to cre‐ ate the best dataset for training a CLIP model (Radford et al., 2021). |
| https://arxiv.org/abs/2406.11794 | Li et al., 2024 | In 2024, they hosted a similar competition to evaluate datasets for language models with scales from 412M to 7B parameters (Li et al., 2024). |
| https://oreil.ly/IK-1c | MLCommons, 2023 | Other similar data-centric benchmarks include DataPerf (MLCommons, 2023) and dcbench (Eyuboglu and Karlaš, 2022). |
| https://oreil.ly/BHEh1 | Eyuboglu and Karlaš, 2022 | Other similar data-centric benchmarks include DataPerf (MLCommons, 2023) and dcbench (Eyuboglu and Karlaš, 2022). |
| https://oreil.ly/imdhy | Chun et al., 2024 | problem step-by-step before producing the final answer. |
| https://oreil.ly/3d_EG | IBM | IBM defined data quality along seven dimensions: completeness, uniqueness, validity, timeliness, accuracy, consistency, and fitness for purpose. |
| https://en.wikipedia.org/wiki/Data_quality | Wikipedia | Wikipedia added accessibility, comparability, credi‐ bility, flexibility, and plausibility. |
| https://arxiv.org/abs/2403.04652 | Young et al., 2024 | The creators of the Yi model family found that 10K carefully crafted instructions are superior to hundreds of thousands of noisy instructions (Young et al., 202 |
| https://arxiv.org/abs/2305.11206 | Zhou et al., 2023 | that 10K carefully crafted instructions are superior to hundreds of thousands of noisy instructions (Young et al., 2024). |
| https://arxiv.org/abs/2406.11704 | Adler et al., 2024 | that the most straightforward way to further improve the performance of chat lan‐ guage models is to increase the quality and diversity of data employed in the |
| https://www.arxiv.org/abs/2408.04154 | Shen et al., 2024 | “The Data Addition Dilemma” (Shen et al., 2024) demonstrated that in some cases, adding more heterogeneous data can lead to worse performance. |
| https://oreil.ly/mUEJO | Jeremy Howard | At one extreme, Jeremy Howard and Jonathan Whitaker did a fun experiment to show that LLMs can learn from a single example. |
| https://arxiv.org/abs/2102.01293 | Hernandez et al., 2021 | This is due to a phenomenon called ossification, where pre- training can ossify (i.e., freeze) the model weights so that they don’t adapt as well to the finetun |
| https://oreil.ly/-R3Wd | OpenAI’s finetuning guide | OpenAI’s finetuning guide shows that if you have fewer examples (100), more advanced models give you better finetuning performance. |
| https://oreil.ly/tlt5h | Hugging Face | Hugging Face and Kaggle each host hundreds of thousands of datasets. |
| https://oreil.ly/g8A4a | Kaggle | Hugging Face and Kaggle each host hundreds of thousands of datasets. |
| https://oreil.ly/TgOaR | Dataset Search | Google has a wonderful and underrated Dataset Search. |
| https://data.gov | Data.gov | Data.gov hosts hundreds of thousands of datasets, and data.gov.in hosts tens of thousands. |
| https://data.gov.in | data.gov.in | Data.gov hosts hundreds of thousands of datasets, and data.gov.in hosts tens of thousands. |
| https://oreil.ly/VhVzp | Institute for Social Research | University of Michigan’s Institute for Social Research ICPSR has data from tens of thousands of social studies. |
| https://oreil.ly/jAR9e | UC Irvine’s Machine Learning Repository | UC Irvine’s Machine Learning Repository and OpenML are two older dataset repositories, each hosting several thousand datasets. |
| https://oreil.ly/d-Yty | OpenML | UC Irvine’s Machine Learning Repository and OpenML are two older dataset repositories, each hosting several thousand datasets. |
| https://oreil.ly/_tW6P | Open Data Network | The Open Data Network lets you search among tens of thousands of datasets. |
| https://oreil.ly/DZ5uV | AWS’s Open Data | Cloud service providers often host a small collection of open datasets; the most notable one is AWS’s Open Data. |
| https://oreil.ly/HMJX_ | TensorFlow datasets | ML frameworks often have small pre-built datasets that you can load while using the framework, such as TensorFlow datasets. |
| https://github.com/EleutherAI/lm-evaluation-harness | Eleuther AI’s lm-evaluation- | For example, Eleuther AI’s lm-evaluation- harness hosts 400+ benchmark datasets, averaging 2,000+ examples per dataset. |
| https://oreil.ly/eb_Bn | Stanford Large Network Dataset Collection | The Stanford Large Network Dataset Collection is a great repository for graph datasets. |
| https://learning.oreilly.com/library/view/designing-machine-learning/9781098107956/ch04.html#perturbation | Designing Machine Learning Systems | 8 My book, Designing Machine Learning Systems, discusses data augmentation in Chapter 4. |
| https://www.linkedin.com/blog/engineering/generative-ai/musings-on-building-a-generative-ai-product?_l=en_US | LinkedIn | Some teams, including LinkedIn, have reported that annotation guidelines were among the most challenging parts of their AI engineering pipeline. |
| https://github.com/joke2k/faker | Faker | For example, libraries like Faker and Chance let you generate data in simple formats such as names, addresses, phone numbers, and email addresses for testing. |
| https://chancejs.com | Chance | For example, libraries like Faker and Chance let you generate data in simple formats such as names, addresses, phone numbers, and email addresses for testing. |
| https://arxiv.org/abs/2305.11171 | Gekh‐ | As described in “TrueTeacher”, Gekh‐ man et al. |
| https://oreil.ly/skn8z | Trinh et al., 2024 | DeepMind trained an Olympiad-level geometry model, AlphaGeometry, using 100 million synthetic examples (Trinh et al., 2024). |
| https://oreil.ly/ez6Iw | Krizhevsky et al. (2012) | For images, you can randomly rotate, crop, scale, or erase part of an image. |
| https://oreil.ly/i7hpS | Deng et al., 2009 | (2012) demonstrated in their legendary AlexNet paper the usefulness of this technique by using it to augment the ImageNet dataset (Deng et al., 2009). |
| https://arxiv.org/abs/1710.08864 | Su et al., 2017 | can trick models into misclassifying it. |
| https://arxiv.org/abs/1302.4389 | Goodfellow et al., | Perturbation can both improve the model’s performance and make it more robust against attacks; see Goodfellow et al., 2013 and Moosavi-Dezfooli et al., 2015). |
| https://arxiv.org/abs/1511.04599 | Moosavi-Dezfooli et al., 2015 | Perturbation can both improve the model’s performance and make it more robust against attacks; see Goodfellow et al., 2013 and Moosavi-Dezfooli et al., 2015). |
| https://arxiv.org/abs/1903.12261 | ImageNet-C and ImageNet-P | model’s performance and make it more robust against attacks; see Goodfellow et al., 2013 and Moosavi-Dezfooli et al., 2015). |
| https://oreil.ly/1YFbA | Snap (2022) | Snap (2022) has a great case study on how they augment their assets to create unrepresented corner cases and mitigate implicit biases in their data. |
| https://arxiv.org/abs/1711.03938 | Dosovitskiy et al., 2017 | Examples of self-driving simulation engines include CARLA (Dosovitskiy et al., 2017), Waymo’s SimulationCity, and Tesla’s sim‐ ulation of San Francisco. |
| https://oreil.ly/xbyXd | Waymo’s SimulationCity | Examples of self-driving simulation engines include CARLA (Dosovitskiy et al., 2017), Waymo’s SimulationCity, and Tesla’s sim‐ ulation of San Francisco. |
| https://oreil.ly/YnbiK | Tesla’s sim‐ | Examples of self-driving simulation engines include CARLA (Dosovitskiy et al., 2017), Waymo’s SimulationCity, and Tesla’s sim‐ ulation of San Francisco. |
| https://arxiv.org/abs/2403.07714 | Guo et al., 2024 | For example, “StableToolBench” (Guo et al., 2024) demonstrates how to use AI to simulate APIs without having to evoke them. |
| https://oreil.ly/rX6oc | OpenAI, 2019 | The bot learned by playing against itself, an approach called self-play, which helped it develop and refine strate‐ gies over time (OpenAI, 2019). |
| https://oreil.ly/prIw9 | Silver et al., 2016 | Similarly, DeepMind used self-play to collect data from millions of Go games to train AlphaGo (Silver et al., 2016). |
| https://arxiv.org/abs/2309.12284 | Yu et al. (2023) | “Steps to reset passwords.” Yu et al. |
| https://oreil.ly/0ymnI | Cosmopedia | However, as the internet becomes flooded with AI-generated content, models that rely on internet data are likely already pre-trained on synthetic data. |
| https://oreil.ly/FyHwn | Mixtral-8x7B-Instruct-v0.1 | Data synthesis for post-training is also more common because post-training data, including both instruction data and preference data, generally demands the most |
| https://oreil.ly/f8LPj | NVIDIA, 2024 | They picked a valid (prompt, winning, losing) triplet only when the AI judge picked the same winner both times (NVIDIA, 2024). |
| https://oreil.ly/u9ghd | Taori et al., 2023 | Similarly, to train Alpaca (Taori et al., 2023), Stanford researchers began with 175 (instruction, response) examples from the Self-Instruct seed dataset (Wang |
| https://arxiv.org/abs/2212.10560 | Wang et al., | Similarly, to train Alpaca (Taori et al., 2023), Stanford researchers began with 175 (instruction, response) examples from the Self-Instruct seed dataset (Wang |
| https://arxiv.org/abs/2304.08460 | Köksal et al. (2023) | Some research‐ ers, such as Köksal et al. |
| https://arxiv.org/abs/2308.06259 | Li et al. (2023) | The longer the response, the more chance AI has to hallucinate. |
| https://arxiv.org/abs/2309.05447 | Chen et al. (2023) | The longer the response, the more chance AI has to hallucinate. |
| https://arxiv.org/abs/2305.15717 | Gudibande et al., 2023 | As warned by “The False Promise of Imitating Proprietary LLMs” (Gudibande et al., 2023), the perceived performance achieved by mimicking might be superficial. |
| https://oreil.ly/hJhTF | “AI Models Collapse When Trained on Recur‐ | 14 The concept was also later explained by the same authors in “AI Models Collapse When Trained on Recur‐ sively Generated Data” (Nature, July 2024). |
| https://arxiv.org/abs/2404.01413 | Gerstgrasser et al. (2024) | In “Is Model Collapse Inevitable?” Gerstgrasser et al. |
| https://arxiv.org/abs/2310.00429 | Bertrand et al. (2023) | In “Is Model Collapse Inevitable?” Gerstgrasser et al. |
| https://arxiv.org/abs/2402.07043 | Dohmatob et al. | (2023) and Dohmatob et al. |
| https://arxiv.org/abs/2403.04706 | Li et al., 2024 | Some people have been able to improve model performance using a large amount of synthetic data. |
| https://oreil.ly/IUA3j | Nemotron-4 340B-Instruct | Similarly, Nemotron-4 340B-Instruct (NVIDIA, 2024) used 98% synthetic data during its instruction finetuning and preference finetuning phase. |
| https://oreil.ly/OZxiz | Taori and Hashimoto, 2023 | However, these experiments were carried out for only one model iteration. |
| https://arxiv.org/abs/1503.02531 | Hinton et al., 2015 | Model Distillation Model distillation (also called knowledge distillation) is a method in which a small model (student) is trained to mimic a larger model (teac |
| https://arxiv.org/abs/1910.01108 | Sanh et al., 2019 | example, DistilBERT, a model distilled from BERT, reduces the size of a BERT model by 40% while retaining 97% of its language comprehension capabilities and bei |
| https://github.com/tatsu-lab/stanford_alpaca | Alpaca | The student model can be trained from scratch like DistilBERT or finetuned from a pre-trained model like Alpaca. |
| https://oreil.ly/U7gfm | BuzzFeed | For example, BuzzFeed finetuned a Flan-T5 model using LoRA and examples generated by OpenAI’s text-davinci-003. |
| https://oreil.ly/-Vd_q | Mixtral-8x7B- | ous section, is one example. |
| https://oreil.ly/iGToR | NVIDIA, 2024 | The Llama 3 paper notes that while training on data generated by a more competent model can significantly improve a model’s performance, training indiscriminate |
| https://arxiv.org/abs/2304.03277 | arXiv 2304.03277 | How relevant are these topics and lan guages to your task? |
| https://x.com/gdb/status/1622683988736479232 | Greg Brockman, an OpenAI co-founder | ments for manual data inspection. |
| https://arxiv.org/abs/2107.06499 | Lee et al. (2021) | Multiple studies have shown the negative impact of training data duplications on model performance; see Lee et al. |
| https://arxiv.org/abs/2308.12284 | Tirumala et al. (2023) | (2021) and Tirumala et al. |
| https://arxiv.org/abs/2205.10487 | Hernandez et al., | Even when duplications don’t hurt your model’s performance, they can waste your time and compute. |
| https://github.com/chiphuyen/lazynlp | lazyNLP | 16 One of my open source libraries, lazyNLP, also supports overlap estimation and deduplication using Bloom filter. |
| https://en.wikipedia.org/wiki/MinHash | MinHash | Hash-related deduplication methods include MinHash and Bloom filter. |
| https://en.wikipedia.org/wiki/Bloom_filter | Bloom filter | Hash-related deduplication methods include MinHash and Bloom filter. |
| https://github.com/arsenetar/dupeguru | dupeGuru | Some of them are dupeGuru, Dedupe, datasketch, TextDistance, TheFuzz, and deduplicate-text- datasets.16 |
| https://github.com/dedupeio/dedupe | Dedupe | Some of them are dupeGuru, Dedupe, datasketch, TextDistance, TheFuzz, and deduplicate-text- datasets.16 |
| https://github.com/ekzhu/datasketch | datasketch | Some of them are dupeGuru, Dedupe, datasketch, TextDistance, TheFuzz, and deduplicate-text- datasets.16 |
| https://github.com/life4/textdistance | TextDistance | Some of them are dupeGuru, Dedupe, datasketch, TextDistance, TheFuzz, and deduplicate-text- datasets.16 |
| https://github.com/seatgeek/thefuzz | TheFuzz | Some of them are dupeGuru, Dedupe, datasketch, TextDistance, TheFuzz, and deduplicate-text- datasets.16 |
| https://github.com/google-research/deduplicate-text-datasets | deduplicate-text- | Some of them are dupeGuru, Dedupe, datasketch, TextDistance, TheFuzz, and deduplicate-text- datasets.16 |
| https://oreil.ly/Gbu2T | Databricks | Databricks found that removing extraneous Markdown and HTML tokens improved their model’s accuracy by 20% while reducing their input token lengths by 60%. |
| https://arxiv.org/html/2311.14212v2 | Kern et al. | For example, Kern et al. |
| https://oreil.ly/Tb4-W | importance sampling | You can also use importance sampling to find examples that are most important to your task. |
| https://arxiv.org/abs/2206.14486 | Sorscher et al., 2022 | Their efficiencies depend on whether you have a good way to evaluate the importance of each training example. |
