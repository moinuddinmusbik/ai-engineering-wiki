---
title: "AI Engineering — Ch.2: Understanding Foundation Models"
type: source
created: 2026-04-07
updated: 2026-04-07
tags: [ai-engineering, foundation-models, transformer, training, sampling, chip-huyen]
sources: [ai-engineering.pdf]
---

# Ch.2: Understanding Foundation Models

**Book:** AI Engineering by [[Chip Huyen]] (O'Reilly, 2024)
**Pages:** 73–135

## Summary

This chapter dives into how [[Foundation Models]] are built, covering the four major design decisions: training data, model architecture, model size, and post-training alignment. It also covers [[Sampling]], an underrated concept that explains many AI behaviors including hallucination and inconsistency.

The chapter is structured as: Training Data → Modeling (architecture + size) → Post-Training (SFT + preference finetuning) → Sampling (strategies + structured outputs + probabilistic nature).

## Key Takeaways

### Training Data
- **Data quality matters as much as quantity.** Common Crawl is used in most models but contains fake news, propaganda, and low-quality content. A 1.3B model trained on 7B tokens of high-quality coding data outperformed much larger models.
- **English dominance:** 45.88% of Common Crawl is English, 8x more than Russian (2nd place). Languages like Punjabi (231x underrepresented), Swahili (115x), and Bengali (37x) are severely underrepresented.
- **Multilingual performance gap:** GPT-4 performs 3x better on math in English vs Armenian/Farsi. Fails completely in Burmese and Amharic. Burmese requires 10x more tokens than English for the same content.
- **Domain-specific models** (AlphaFold, BioNeMo, Med-PaLM2) outperform general models on specialized tasks. Data for these domains is expensive and scarce.
- **Data is running out.** Training dataset growth rate exceeds new data creation rate. 28% of C4 sources are now restricted. Proprietary data becomes competitive advantage.

### Model Architecture
- **[[Transformer Architecture]]** dominates since 2017, solving seq2seq's two problems: information bottleneck (decoder only sees final hidden state) and sequential processing (slow for long inputs).
- **[[Attention Mechanism]]** is the key innovation — lets models weigh importance of different input tokens via Key/Query/Value vectors. Multi-headed attention enables attending to different token groups simultaneously.
- **Transformer block** = Attention module + MLP module. Number of blocks = number of layers. Model size determined by: model dimension, number of blocks, feedforward dimension, vocabulary size.
- **Prefill vs Decode**: Input tokens processed in parallel (prefill), output tokens generated sequentially (decode). This asymmetry drives many optimization techniques.
- **Alternative architectures gaining traction:** [[RWKV]] (parallelizable RNN), [[State Space Models]] (Mamba — linear-time scaling, million-length sequences), [[Jamba]] (hybrid transformer-Mamba, 52B params on single 80GB GPU).

### Model Size & Scaling
- **Three numbers signal scale:** number of parameters (learning capacity), training tokens (how much learned), FLOPs (training cost).
- **[[Chinchilla Scaling Law]]**: Optimal training tokens ≈ 20x model parameters. Double model → double data. Proposed by DeepMind (2022) from training 400 models (70M–16B params).
- **Compute cost example:** Training GPT-3-175B on 256 H100s at 70% utilization costs ~$4.1M.
- **Mixture-of-Experts (MoE):** Sparse models where only a subset of parameters is active per token. Mixtral 8x7B has 46.7B total params but only 12.9B active per token.
- **[[Inverse Scaling]]:** Larger/more-aligned models can sometimes perform *worse* (e.g., expressing specific political/religious views). Inverse Scaling Prize found failures in memorization and strong-prior tasks.
- **Scaling bottlenecks:** Running out of internet data (training data growth > new data creation). Electricity — data centers at 1–2% global consumption, projected 4–20% by 2030.

### Post-Training
- **Two-step process:** [[Supervised Finetuning]] (SFT) → [[Preference Finetuning]] (RLHF/DPO)
- **SFT** teaches the model to *converse* (not just complete). Uses (prompt, response) demonstration data. InstructGPT: 13,000 pairs at ~$10 each = $130,000. Labelers are highly educated (~90% college degree, 33% master's).
- **Preference finetuning** teaches the model *what kind of conversations to have*. Uses comparison data: (prompt, winning_response, losing_response). RLHF trains a reward model, then optimizes the foundation model against it. DPO is simpler (Meta switched from RLHF to DPO for Llama 3).
- **The Shoggoth metaphor:** Pre-training = untamed monster, SFT = socially acceptable, preference finetuning = smiley face.
- Post-training uses only ~2% of compute (InstructGPT), but unlocks capabilities already latent in the pre-trained model.
- **"Best of N" strategy:** Some companies (Stitch Fix, Grab) skip RL entirely — generate multiple outputs, pick the highest-scored by reward model.

### Sampling
- **Fundamentals:** Model outputs logit vector → softmax → probability distribution → sample next token.
- **[[Temperature]]**: Higher = more creative/diverse (redistributes probability to rare tokens). Lower = more consistent/predictable. 0 = greedy (always pick highest logit). 0.7 common for creative tasks.
- **[[Top-k Sampling]]**: Only consider top-k logits. Reduces computation. k typically 50–500.
- **[[Top-p Sampling]]** (nucleus): Dynamic selection — sum probabilities until reaching p (typically 0.9–0.95). More contextually appropriate than top-k.
- **[[Test Time Compute]]**: Generate multiple outputs, pick the best. Using a verifier gives the same boost as 30x model size increase. Optimal up to ~400 samples (OpenAI), log-linear improvement up to 10,000 (Stanford).
- **[[Structured Outputs]]**: Five approaches — prompting, post-processing, test time compute, constrained sampling (filter invalid logits), finetuning. LinkedIn's defensive YAML parser: 90% → 99.99%.

### The Probabilistic Nature
- **[[Inconsistency]]**: Same input → different outputs. Slightly different input → drastically different outputs. Even fixing temperature/seed/top-p doesn't guarantee 100% consistency (hardware differences matter).
- **[[Hallucination]]**: Two hypotheses:
  1. **Self-delusion** (DeepMind, Ortega 2021): Model can't distinguish its own outputs from given facts. Snowballing — initial wrong assumption leads to more hallucinations.
  2. **Knowledge mismatch** (Leo Gao, OpenAI): SFT trains model to mimic labeler responses using knowledge the model doesn't have → teaches model to hallucinate.
- RLHF results are mixed: helped overall but actually worsened hallucination for InstructGPT.

## Notable Data Points

- Llama training data: 1.4T tokens (v1) → 2T (v2) → 15T (v3)
- RedPajama-v2: 30 trillion tokens (≈ 450M books)
- InstructGPT: 98% compute for pre-training, 2% for post-training
- Comparison labeling: $3.50 per comparison (Llama 2), $25 per written response
- RLHF labeler agreement: ~73% (InstructGPT)
- Llama 2-7B: 32 transformer blocks, 4096 model dim, 32K vocab, 4K context
- Llama 3-405B: 126 transformer blocks, 16384 model dim, 128K vocab, 128K context

## Connections

- Deepens [[Foundation Models]] and [[Language Models]] from Ch.1
- Training data quality connects to [[Model Adaptation]] — garbage in, garbage out
- Post-training section is foundation for Ch.7 ([[Finetuning]])
- Sampling section is foundation for Ch.5 ([[Prompt Engineering]]) and Ch.9 (Inference Optimization)
- Scaling law connects to the [[AI Engineering Stack]] cost considerations

---

## References (132 links)

All hyperlinks embedded by the author in this chapter, extracted from the book PDF. See also the [complete URL reference](../analyses/ai-engineering-urls.md) across all chapters.


| URL | Name / Link Text | Description |
|-----|-----------------|-------------|
| https://oreil.ly/wf2Lw | Common Crawl | For example, a common source for training data is Common Crawl, created by a nonprofit organization that sporadically crawls websites on the internet. |
| https://arxiv.org/abs/1910.10683v4 | Colossal Clean Crawled | Google provides a clean subset of Common Crawl called the Colossal Clean Crawled Corpus, or C4 for short. |
| https://oreil.ly/-1UMD | study by the | A study by the Washington Post shows that the 1,000 most common websites in the dataset include several media outlets that rank low on NewsGuard’s scale for tru |
| https://oreil.ly/OisOs | NewsGuard’s scale for trustworthiness | A study by the Washington Post shows that the 1,000 most common websites in the dataset include several media outlets that rank low on NewsGuard’s scale for tru |
| https://oreil.ly/gGwRz | GPT-2 | For exam‐ ple, OpenAI used only the Reddit links that received at least three upvotes to train GPT-2. |
| https://arxiv.org/abs/2306.11644 | Gunasekar | Using 7B tokens of high-quality coding data, Gunasekar et al. |
| https://arxiv.org/abs/2304.05613 | Lai et al., 2023 | English dominates the internet. |
| https://oreil.ly/qK2Ap | GPT-4 performed much better in English | purpose models work much better for English than other languages, according to multiple studies. |
| https://oreil.ly/G13KM | “GPT-4 Can Solve Math Problems—but Not in All Languages” | You can verify the study using OpenAI’s Tokenizer. |
| https://oreil.ly/iqhNY | OpenAI’s Tokenizer | You can verify the study using OpenAI’s Tokenizer. |
| https://oreil.ly/LcBfx | NewsGuard | For example, NewsGuard found that ChatGPT is more willing to produce misinfor‐ mation in Chinese than in English. |
| https://oreil.ly/Zq5Sw | a lot | more efficient for some languages than others. |
| https://github.com/THUDM/ChatGLM2-6B | ChatGLM | The most active language, other than English, is undoubtedly Chinese, with ChatGLM, YAYI, Llama-Chinese, and others. |
| https://github.com/wenge-research/YAYI | YAYI | The most active language, other than English, is undoubtedly Chinese, with ChatGLM, YAYI, Llama-Chinese, and others. |
| https://github.com/LlamaFamily/Llama-Chinese | Llama-Chinese | The most active language, other than English, is undoubtedly Chinese, with ChatGLM, YAYI, Llama-Chinese, and others. |
| https://oreil.ly/a6j-N | CroissantLLM | There are also models in French (CroissantLLM), Vietnamese (PhoGPT), Arabic (Jais), and many more languages. |
| https://github.com/VinAIResearch/PhoGPT | PhoGPT | There are also models in French (CroissantLLM), Vietnamese (PhoGPT), Arabic (Jais), and many more languages. |
| https://oreil.ly/uG27L | Jais | There are also models in French (CroissantLLM), Vietnamese (PhoGPT), Arabic (Jais), and many more languages. |
| https://oreil.ly/St1o8 | “Inside the Secret List of Websites That Make AI like ChatGPT Sound Smart” | Most analyses I could find about vision datasets are about image sizes, resolutions, or video lengths. |
| https://oreil.ly/4XsOV | Gemini | This is largely thanks to the inclusion of these domains in their training data. |
| https://oreil.ly/KLVgX | GPTs | This is largely thanks to the inclusion of these domains in their training data. |
| https://oreil.ly/58gxQ | Llamas | This is largely thanks to the inclusion of these domains in their training data. |
| https://oreil.ly/MTqyR | perform on different benchmarks | Table 2-3 shows how two models, CLIP and Open CLIP, perform on different benchmarks. |
| https://oreil.ly/JX37g | DeepMind’s AlphaFold | One of the most famous domain-specific models is per‐ haps DeepMind’s AlphaFold, trained on the sequences and 3D structures of around 100,000 known proteins. |
| https://oreil.ly/M1Nsc | NVIDIA’s BioNeMo | NVIDIA’s BioNeMo is another model that focuses on bio‐ molecular data for drug discovery. |
| https://oreil.ly/F76hq | Google’s Med-PaLM2 | Google’s Med-PaLM2 combined the power of an LLM with medical data to answer medical queries with higher accuracy. |
| https://arxiv.org/abs/1706.03762 | Vaswani et al., 2017 | It addresses many limitations of the previous architectures, which contributed to its popularity. |
| https://arxiv.org/abs/1409.3215 | seq2seq | The transformer architecture was popularized on the heels of the success of the seq2seq (sequence-to-sequence) architecture. |
| https://oreil.ly/fb1aR | Google incorporated seq2seq into Google Translate | In 2016, Google incorporated seq2seq into Google Translate, an update that they claimed to have given them the “largest improvements to date for machine transla |
| https://arxiv.org/abs/1409.0473 | “Neural Machine Translation by Jointly Learning to Align and Translate” | 7 Bahdanau et al., “Neural Machine Translation by Jointly Learning to Align and Translate”. |
| https://en.wikipedia.org/wiki/Dot_product | dot product | g The attention mechanism computes how much attention to give an input token by performing a dot product between the query vector and its key vector. |
| https://arxiv.org/abs/2307.09288 | Touvron et al., 2023 | For example, in Llama 2-7B (Touvron et al., 2023), the model’s hidden dimension size is 4096, meaning that each of these matrices has a 4096 × 4096 dimension. |
| https://arxiv.org/abs/1803.08375 | Agarap, 2018 | Common nonlinear functions are ReLU, Rectified Linear Unit (Agarap, 2018), and GELU (Hendrycks and Gimpel, 2016), which was used by GPT-2 and GPT-3, respectivel |
| https://arxiv.org/abs/1606.08415 | Hendrycks and Gimpel, 2016 | Common nonlinear functions are ReLU, Rectified Linear Unit (Agarap, 2018), and GELU (Hendrycks and Gimpel, 2016), which was used by GPT-2 and GPT-3, respectivel |
| https://arxiv.org/abs/2407.21783 | Dubey et al., | Table 2-4 shows these dimen‐ sion values for different Llama 2 (Touvron et al., 2023) and Llama 3 (Dubey et al., 2024) models. |
| https://oreil.ly/j4wwW | Sutskever’s talk at the Simons Institute at Berkeley (2023) | For more information, watch Sutskever’s talk at the Simons Institute at Berkeley (2023). |
| https://oreil.ly/ON55d | run fast on Tensor Processing Units (TPUs) | perform existing ones, these new architectures have to be able to simulate programs that existing architec‐ tures cannot. |
| https://oreil.ly/1spG5 | AlexNet | Since AlexNet revived the interest in deep learning in 2012, many architectures have gone in and out of fashion. |
| https://arxiv.org/abs/1406.2661 | GANs | GANs (generative adversarial networks) captured the collective imagination a bit longer (2014–2019). |
| https://github.com/BlinkDL/RWKV-LM | RWKV | One popular model is RWKV (Peng et al., 2023), an RNN-based model that can be parallelized for training. |
| https://arxiv.org/abs/2110.13985 | Gu et al., 2021a | An architec‐ ture that has shown a lot of promise in long-range memory is SSMs (state space mod‐ els) (Gu et al., 2021a). |
| https://arxiv.org/abs/2111.00396 | Gu et al., 2021b | • S4, introduced in “Efficiently Modeling Long Sequences with Structured State Spaces” (Gu et al., 2021b), was developed to make SSMs more efficient. |
| https://arxiv.org/abs/2212.14052 | Fu et al., 2022 | Spaces (Gu et al., 2021b), was developed to make SSMs more efficient. |
| https://oreil.ly/n7wYO | Gu and Dao, 2023 | • Mamba, introduced in “Mamba: Linear-Time Sequence Modeling with Selective State Spaces” (Gu and Dao, 2023), scales SSMs to three billion parameters. |
| https://arxiv.org/abs/2403.19887 | Lieber et al., 2024 | • Jamba, introduced in “Jamba: A Hybrid Transformer–Mamba Language Model” (Lieber et al., 2024), interleaves blocks of transformer and Mamba layers to scale up |
| https://oreil.ly/uyiBH | 52B | The authors released a mixture-of-experts model with 52B total available parameters (12B active parameters) designed to fit in a single 80 GB GPU. |
| https://arxiv.org/abs/1701.06538 | Shazeer et al., 2017 | A type of sparse model that has gained popularity in recent years is mixture-of- experts (MoE) (Shazeer et al., 2017). |
| https://oreil.ly/VvXbu | Mixtral 8x7B | For example, Mixtral 8x7B is a mixture of eight experts, each expert with seven bil‐ lion parameters. |
| https://arxiv.org/abs/2204.14198 | Alayrac et al., 2022 | For example, Google’s Flamingo (Alayrac et al., 2022) was trained using four datasets—one of them has 1.8 billion (image, text) pairs and one has 312 million (i |
| https://arxiv.org/abs/2302.13971 | Llama 1 | g Meta used increasingly larger datasets to train their Llama models: • 1.4 trillion tokens for Llama 1 • 2 trillion tokens for Llama 2 • 15 trillion tokens for |
| https://oreil.ly/vfSQw | Llama 3 | • 2 trillion tokens for Llama 2 • 15 trillion tokens for Llama 3 Together’s open source dataset RedPajama-v2 has 30 trillion tokens. |
| https://oreil.ly/SfB4g | 30 trillion tokens | • 15 trillion tokens for Llama 3 Together’s open source dataset RedPajama-v2 has 30 trillion tokens. |
| https://oreil.ly/A3K90 | (DeepMind, | Source: “Training Compute-Optimal Large Language Models” (DeepMind, 2022). |
| https://arxiv.org/abs/2204.02311 | Chowdhery et al., 2022 | Google’s largest PaLM-2 model, for example, was trained using 1022 FLOPs (Chowdhery et al., 2022). |
| https://arxiv.org/abs/2005.14165 | Brown et al., 2020 | GPT-3-175B was trained using 3.14 × 1023 FLOPs (Brown et al., 2020). |
| https://oreil.ly/HcFYz | 60 TeraFLOP/s | For example, an NVIDIA H100 NVL GPU can deliver a maximum of 60 TeraFLOP/s: 6 × 1013 FLOPs a second or 5.2 × 1018 FLOPs a day.16 Be alert for confusing notation |
| https://arxiv.org/abs/2212.09251 | Perez et al., 2022 | In 2022, Anthropic discovered that, counterintuitively, more alignment training (discussed in “Post-Training” on page 78) leads to models that align less with h |
| https://arxiv.org/abs/2306.09479 | Inverse Scaling Prize | In 2023, a group of researchers, mostly from New York University, launched the Inverse Scaling Prize to find tasks where larger language models perform worse. |
| https://arxiv.org/abs/2203.15556 | “Training Compute-Optimal Large Language Models” | They found that for compute-optimal |
| https://arxiv.org/abs/2401.00448 | Sardana et al. (2023) | would perform better, but they opted for smaller models. |
| https://oreil.ly/oq-LE | Artificial Intelligence Index Report 2022 (Stanford University HAI) | For example, on the ImageNet dataset, the cost to achieve 93% accuracy halved from 2019 to 2021, according to the Artificial Intelligence Index Report 2022 (Sta |
| https://oreil.ly/kO41d | “Beyond Neural Scaling Laws: Beating | mance improvement remains high. |
| https://x.com/jaschasd/status/1756930242965606582 | shared a beautiful visualization of what hyperparameters work | 18 Jascha Sohl-Dickstein, an amazing researcher, shared a beautiful visualization of what hyperparameters work and don’t work on his X page. |
| https://oreil.ly/sHwbw | 2022 paper | study the impact of hyperparameters on models of different sizes, usually much smaller than the target model size, and then extrapolate how these hyperparameter |
| https://oreil.ly/GxSe0 | Dario Amodei, Anthropic CEO | 19 Dario Amodei, Anthropic CEO, said that if the scaling hypothesis is true, a $100 billion AI model will be as good as a Nobel prize winner. |
| https://arxiv.org/abs/2206.07682 | Wei et al., 2022 | In addition, emergent abilities (Wei et al., 2022) make the extrapolation less accurate. |
| https://oreil.ly/kuG3J | Luke Metz, 2022 | To learn more about scaling extrapo‐ lation, check out this excellent blog post: “On the Difficulty of Extrapolation with NN Scaling” (Luke Metz, 2022). |
| https://x.com/ibab/status/1733558576982155274 | Igor Babuschkin, a core developer behind Grok | Igor Babuschkin, a core developer behind Grok, |
| https://arxiv.org/abs/2401.05749 | Thompson et al., 2024 | AI can be used to generate an article, then translate that article into multiple languages, as shown in “A Shocking Amount of the Web Is Machine Translated” (Th |
| https://arxiv.org/abs/2305.17493 | Shumailov et al., 2023 | However, the impact of AI- generated data on models is more nuanced and is discussed in Chapter 8. |
| https://oreil.ly/AkAyI | deals | This is a reason why OpenAI negotiated deals with publishers and media outlets including Axel Springer and the Associated Press. |
| https://oreil.ly/o7WB3 | Reddit | It’s not surprising that in light of ChatGPT, many companies, including Reddit and Stack Overflow, have changed their data terms to prevent other companies from |
| https://oreil.ly/xNuju | Stack Overflow | It’s not surprising that in light of ChatGPT, many companies, including Reddit and Stack Overflow, have changed their data terms to prevent other companies from |
| https://arxiv.org/abs/2407.14933 | Longpre et al. (2024) | It’s not surprising that in light of ChatGPT, many companies, including Reddit and Stack Overflow, have changed their data terms to prevent other companies from |
| https://github.com/google-research/text-to-text-transfer-transformer#c4 | google-research/text-to-text-transfer-transformer | Due to changes in its Terms of Service and crawling restrictions, a full 45% of C4 is now restricted. |
| https://oreil.ly/0DKHL | 4% and 20% by | This number is estimated to reach between 4% and 20% by 2030 (Patel, Nishball, and Ontiveros, 2024). |
| https://oreil.ly/iJG1q | reinforce‐ | Preference finetuning: Further finetune the model to output responses that align with human preference. |
| https://oreil.ly/tbgTi | GPT-3.5 | with human preference. |
| https://arxiv.org/abs/2305.18290 | DPO | Let me highlight the difference between pre-training and post-training another way. |
| https://arxiv.org/abs/2309.00267 | reinforcement | Let me highlight the difference between pre-training and post-training another way. |
| https://arxiv.org/abs/2212.08073 | Claude | Let me highlight the difference between pre-training and post-training another way. |
| https://oreil.ly/9bbzX | InstructGPT | As post-training consumes a small portion of resources compared to pre-training (InstructGPT used only 2% of compute for post-training and 98% for pre-training) |
| https://en.wikipedia.org/wiki/Shoggoth | Shoggoth | If you squint, Figure 2-10 looks very similar to the meme depicting the monster Shoggoth with a smiley face in Figure 2-11: 1. |
| https://x.com/anthrupad/status/1622349563922362368 | anthrupad | Adapted from an original image shared by anthrupad. |
| https://oreil.ly/8U2z8 | InstructGPT | Figure 2-12 shows a distribu‐ tion of types of tasks OpenAI used to finetune their model InstructGPT. |
| https://arxiv.org/abs/2203.02155 | InstructGPT | Examples of demonstration data used for InstructGPT. |
| https://oreil.ly/SF_X9 | ~90% have at | Among those who labeled demonstration data for InstructGPT, ~90% have at least a college degree and more than one-third have a master’s degree. |
| https://arxiv.org/abs/2304.07327 | Köpf et al., 2023 | For example, in a self-reported survey, 90% of volunteer labelers identified as male (Köpf et al., 2023). |
| https://arxiv.org/abs/2112.11446 | simple heuristics | DeepMind used simple heuristics to filter for conversations from internet data to train their model Gopher. |
| https://oreil.ly/5oSEJ | may become boring | If a model is censored too much, your model may become boring, driving away users. |
| https://oreil.ly/D1S6y | driving away users | If a model is censored too much, your model may become boring, driving away users. |
| https://oreil.ly/h9oG6 | Anthropic | An example of comparison data from Anthropic’s HH-RLHF dataset. |
| https://arxiv.org/abs/2403.04132 | Chiang et al., 2024 | In a talk with my Discord community, Llama-2 author Thomas Scialom shared that each comparison cost them $3.50. |
| https://oreil.ly/P1MPQ | Thomas Scialom | In a talk with my Discord community, Llama-2 author Thomas Scialom shared that each comparison cost them $3.50. |
| https://oreil.ly/kYtBG | UI that OpenAI’s labelers | Figure 2-13 shows the UI that OpenAI’s labelers used to create comparison data for the reward model of InstructGPT. |
| https://oreil.ly/TpaGg | proximal policy optimization (PPO) | This training process is often done with proximal policy optimization (PPO), a reinforcement learning algorithm released by OpenAI in 2017. |
| https://oreil.ly/iYh-B | Stitch Fix | For example, Stitch Fix and Grab find that having the reward model alone is good enough for their applications. |
| https://oreil.ly/CSSed | Grab | For example, Stitch Fix and Grab find that having the reward model alone is good enough for their applications. |
| https://en.wikipedia.org/wiki/Arg_max | arg max function | 92 \| Chapter 2: Understanding Foundation Models |
| https://oreil.ly/jWEsP | logprobs | Anthropic doesn’t expose its models’ logprobs. |
| https://x.com/xuanalogue/status/1707757449900437984 | September | It used to let you get the logprobs of arbitrary user-provided text but discontinued this in September 2023. |
| https://oreil.ly/VAUl6 | logprobs | Logprobs, short for log probabilities, are probabilities in the log scale. |
| https://en.wikipedia.org/wiki/Arithmetic_underflow | underflow | Logprobs, short for log probabilities, are probabilities in the log scale. |
| https://github.com/huggingface/transformers/issues/27670 | min-p | A related sampling strategy is min-p, where you set the minimum probability that a token must reach to be considered during sampling. |
| https://en.wikipedia.org/wiki/Beam_search | beam search | pick one that works best. |
| https://oreil.ly/XYugZ | best_of | 30 As of this writing, in the OpenAI API, you can set the parameter best_of to a specific value, say 10, to ask OpenAI models to return the output with the high |
| https://oreil.ly/1Njeh | Stitch Fix | Recall that both Stitch Fix and Grab pick the outputs given high scores by their reward models or verifiers. |
| https://oreil.ly/l21nr | Grab | Recall that both Stitch Fix and Grab pick the outputs given high scores by their reward models or verifiers. |
| https://oreil.ly/-HQIB | Nextdoor | Nextdoor found that using a reward model was the key factor in improving their application’s performance (2023). |
| https://oreil.ly/R_uvq | Cobbe et al., 2021 | OpenAI also trained verifiers to help their models pick the best solutions to math problems (Cobbe et al., 2021). |
| https://arxiv.org/abs/2408.03314 | Snell et al., 2024 | The same paper asks an interesting question: If an LLM is allowed to use a fixed but non‐ trivial amount of inference-time compute, how much can it improve its |
| https://oreil.ly/8YNwQ | Brown et al., 2024 | “Monkey Business” (Brown et al., 2024) finds that the number of problems solved often increases log-linearly as the number of samples increases from 1 to 10,000 |
| https://arxiv.org/abs/2110.14168 | OpenAI | OpenAI (2021) found that sampling more outputs led to better perfor‐ mance, but only up to 400 outputs. |
| https://arxiv.org/abs/2203.11171 | Wang et al. (2023) | (2023) called this approach self-consistency. |
| https://github.com/guidance-ai/guidance | guidance | Frameworks that support structured outputs include guidance, outlines, instructor, and llama.cpp. |
| https://github.com/dottxt-ai/outlines | outlines | Frameworks that support structured outputs include guidance, outlines, instructor, and llama.cpp. |
| https://github.com/instructor-ai/instructor | instructor | Frameworks that support structured outputs include guidance, outlines, instructor, and llama.cpp. |
| https://github.com/ggerganov/llama.cpp/discussions/177 | llama.cpp | Frameworks that support structured outputs include guidance, outlines, instructor, and llama.cpp. |
| https://oreil.ly/NxZDF | JSON mode | OpenAI was the first model pro‐ vider to introduce JSON mode in their text generation API. |
| https://oreil.ly/ZTRaA | Bottaro and Ramgopal, 2020 | LinkedIn’s defensive YAML parser increased the percentage of correct YAML out‐ puts from 90% to 99.99% (Bottaro and Ramgopal, 2020). |
| https://oreil.ly/hNRf4 | Brandon T. Willard, 2024 | own grammar, constraint sampling is less generalizable. |
| https://oreil.ly/sljei | OpenAI’s finetuning services | OpenAI’s finetuning services used to let you add a classifier head when training, but as I write, this feature has been disabled. |
| https://x.com/OxfordDiplomat/status/1424388443010998277?lang=en | the chances are low, but never zero | In a panel I participated in with Drew Houston (CEO of Dropbox) and Harrison Chase (CEO of LangChain) in July 2023, we all agreed that hallucination is the bigg |
| https://oreil.ly/FCyyA | fined for submitting fictitious legal research to court | In June 2023, a law firm was fined for submitting fictitious legal research to court. |
| https://oreil.ly/cg0JY | Goyal et al., 2016 | Hallucination in the con‐ text of text generation was mentioned as early as 2016 (Goyal et al., 2016). |
| https://oreil.ly/ah9MT | Lee et al., 2018 | Detecting and measuring hallucinations has been a staple in natural language generation (NLG) since then (see Lee et al., 2018; Nie et al., 2019; and Zhou et al |
| https://oreil.ly/13wUD | Nie et al., 2019 | Detecting and measuring hallucinations has been a staple in natural language generation (NLG) since then (see Lee et al., 2018; Nie et al., 2019; and Zhou et al |
| https://arxiv.org/abs/2011.02593 | Zhou et al., 2020 | Detecting and measuring hallucinations has been a staple in natural language generation (NLG) since then (see Lee et al., 2018; Nie et al., 2019; and Zhou et al |
| https://arxiv.org/abs/2110.10819#deepmind | Ortega et al. at DeepMind in 2021 | The first hypothesis, originally expressed by Ortega et al. |
| https://arxiv.org/abs/2305.13534 | snowballing hallucinations | (2023) call this phenomenon snowballing hallucinations. |
| https://oreil.ly/9idN4 | Leo Gao | This view was first argued by Leo Gao, an OpenAI researcher. |
| https://oreil.ly/Fqo2S | UC Berkeley talk | In April 2023, John Schulman, an OpenAI co-founder, expressed the same view in his UC Berkeley talk. |
