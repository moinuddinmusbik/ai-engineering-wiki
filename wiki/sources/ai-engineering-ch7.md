---
title: "AI Engineering — Ch.7: Fine-Tuning"
type: source
created: 2026-04-27
updated: 2026-04-27
tags: [finetuning, lora, quantization, model-merging, peft, transfer-learning]
sources: [ai-engineering.pdf]
---

# AI Engineering — Ch.7: Fine-Tuning

**Book:** *AI Engineering* by Chip Huyen (O'Reilly, 2024)
**Chapter:** 7 — Fine-Tuning
**Pages:** ~331–386

---

## Summary

Chapter 7 covers the full spectrum of adapting pre-trained models to specific tasks through fine-tuning. It begins by framing fine-tuning within the broader context of [[Model Adaptation]] — the third pillar alongside [[Prompt Engineering]] and [[RAG]]. The chapter then dives deep into the engineering challenges of actually running fine-tuning at scale: memory bottlenecks, numerical representation formats, and the family of parameter-efficient techniques that make fine-tuning practical on commodity hardware.

The central thesis on when to fine-tune is crisp: **"Fine-tuning is for form, RAG is for facts."** Fine-tuning is the right tool when the model exhibits behavioral problems — wrong output format, wrong tone, wrong task structure — not when it simply lacks knowledge. If the problem is missing or stale information, [[RAG]] is the correct intervention.

A key cautionary data point: when Bloomberg built [[BloombergGPT]], a domain-specific 50B-parameter model trained on 363B finance tokens, it was outperformed by GPT-4 on many financial tasks. General models with better training can outperform purpose-built domain models. This underscores the importance of exhausting prompt engineering and RAG before investing in fine-tuning.

The chapter's treatment of memory arithmetic is thorough. Training is far more memory-intensive than inference — during backpropagation, the system must store not just weights but also activations (for the backward pass), gradients, and optimizer states. The Adam optimizer stores two additional values per parameter (first and second moment estimates), tripling the per-parameter memory footprint beyond the weights alone. A 7B-parameter model in FP32 requires ~28 GB just for weights, but full fine-tuning demands multiples more for the full training state.

Parameter-efficient fine-tuning ([[LoRA]] and its variants) resolves this by updating only a small fraction of parameters. [[LoRA]] specifically decomposes the weight update matrix into two low-rank matrices: W' = W + (α/r)×AB, where A and B together hold perhaps 0.003% of the original parameter count. QLoRA extends this by storing the base model in 4-bit NF4 format, enabling 65B-parameter models to be fine-tuned on a single 48 GB GPU.

The chapter closes with [[Model Merging]] — an emerging technique for combining capabilities from multiple fine-tuned checkpoints without retraining. Methods range from linear interpolation (model soups, task arithmetic) to geometric interpolation (SLERP) to layer stacking (frankenmerging, MoE creation).

---

## Key Takeaways

- **When to fine-tune**: behavioral/form problems → fine-tune; knowledge/information problems → RAG. Fine-tuning cannot reliably inject new factual knowledge.
- **Fine-tuning types**: supervised fine-tuning (SFT), preference fine-tuning (RLHF/DPO), continued pre-training, infilling. Most production fine-tuning is SFT.
- **Memory bottleneck**: backpropagation requires storing activations for the entire forward pass. For training: weights + activations + gradients + optimizer states (Adam adds 2× per-parameter overhead).
- **Numerical representations**: FP32 (4B/param), FP16/BF16 (2B/param, BF16 preferred for training due to wider dynamic range), TF32 (19-bit hybrid), INT8, INT4. Mixed precision training typically uses FP16/BF16 for compute and FP32 for optimizer state.
- **Quantization**: Post-training quantization (PTQ) is the most common approach. QAT (quantization-aware training) is more accurate but costlier.
- **PEFT landscape**: Two families — adapter-based (LoRA dominant) and soft prompt-based (prefix-tuning, P-Tuning, prompt tuning).
- **LoRA**: rank r typically 4–64; for GPT-3 scale, LoRA uses 0.0027% of full fine-tuning parameters. At inference, adapters can be merged into base weights (no latency cost) or kept separate for multi-LoRA serving.
- **QLoRA**: 4-bit NF4 base model + LoRA adapters + paged optimizers → 65B model on one 48 GB GPU.
- **Model merging**: Combining multiple fine-tuned checkpoints (model soups, task vectors, SLERP, TIES/DARE for pruning, frankenmerging, MoE creation). Avoids catastrophic forgetting and enables capability combination.
- **Practical path**: Try prompt engineering first → RAG if knowledge gap → fine-tuning if behavior gap → full pre-training only if truly necessary.

---

## Notable Quotes

> "Finetuning is for form, RAG is for facts."

> "When Bloomberg built BloombergGPT, a 50B-parameter model trained on 363B tokens of financial data, it was still outperformed by GPT-4 on many financial tasks."

---

## Pages Created / Updated

**Created:**
- [[Finetuning]] — when to finetune, types, memory math, finetuning tactics
- [[LoRA]] — low-rank adaptation, QLoRA, multi-LoRA serving
- [[Model Merging]] — summing methods, layer stacking, concatenation
- [[Quantization]] — numerical representations, PTQ, QAT, mixed precision

**Updated:**
- [[Model Adaptation]] — added finetuning depth from Ch.7

---

## References (109 links)

All hyperlinks embedded by the author in this chapter, extracted from the book PDF. See also the [complete URL reference](../analyses/ai-engineering-urls.md) across all chapters.


| URL | Name / Link Text | Description |
|-----|-----------------|-------------|
| https://oreil.ly/Udw0Z | Bozinov‐ | Finetuning is one way to do transfer learning, a concept first introduced by Bozinov‐ ski and Fulgosi in 1976. |
| https://arxiv.org/abs/1611.04558 | Johnson et. al, 2016 | piano can make it easier to learn another musical instrument. |
| https://oreil.ly/5-5lw | InstructGPT paper | OpenAI’s InstructGPT paper (2022) suggested viewing finetuning as unlocking the capabilities a model already has but that are difficult for users to access via |
| https://arxiv.org/abs/2308.12950 | Rozière et al., 2024 | Figure 7-1 shows the making of different Code Llama models (Rozière et al., 2024), from the base model Llama 2, using different finetuning techniques. |
| https://arxiv.org/abs/2204.05862 | Bai et al., 2020 | 1 Some people call this phenomenon an alignment tax (Bai et al., 2020), but this term can be confused with penalties against human preference alignment. |
| https://oreil.ly/iPwB_ | Wang and Russa‐ | One especially interesting use case of finetuning is bias mitigation. |
| https://oreil.ly/RoPL4 | Gari‐ | fully curated data during finetuning can counteract these biases (Wang and Russa‐ kovsky, 2023). |
| https://arxiv.org/abs/2210.11416 | Chung et al., 2022 | A small model, finetuned on a specific task, might outperform a much larger out-of- the-box model on that task. |
| https://arxiv.org/abs/2303.17564 | Wu et al., 2023 | The estimated cost of the compute was between $1.3 million and $2.6 million, excluding data costs (Wu et al., 2023). |
| https://arxiv.org/abs/2305.05862 | Li et al. (2023) | cost of the compute was between $1.3 million and $2.6 million, excluding data costs (Wu et al., 2023). |
| https://oreil.ly/J-soV | Claude 3.5 Sonnet | Since then, several mid-size models with performance comparable to GPT-4 have been released, including Claude 3.5 Sonnet (70B parameters), Llama 3-70B-Instruct, |
| https://oreil.ly/6lt6- | Llama 3-70B-Instruct | Since then, several mid-size models with performance comparable to GPT-4 have been released, including Claude 3.5 Sonnet (70B parameters), Llama 3-70B-Instruct, |
| https://oreil.ly/HZnfa | Qwen2-72B-Instruct | Since then, several mid-size models with performance comparable to GPT-4 have been released, including Claude 3.5 Sonnet (70B parameters), Llama 3-70B-Instruct, |
| https://oreil.ly/t9HTH | “Fine-Tuning or Retrieval?” | The paper “Fine-Tuning or Retrieval?” by Ovadia et al. |
| https://oreil.ly/Ny1WI | OpenAI | This figure is inspired by an example workflow shown by OpenAI (2023). |
| https://oreil.ly/B59ci | Maheswaranathan et al. | One example, described by Maheswaranathan et al., combines random search with surrogate gradients, instead of using real gradients, to update model weights. |
| https://arxiv.org/abs/1609.01596 | Arild Nøkland, 2016 | Another interesting approach is direct feedback alignment (Arild Nøkland, 2016). |
| https://oreil.ly/u7wYx | “Transformer Inference Arith‐ | Memory Bottlenecks \| 323 |
| https://oreil.ly/Xe7h6 | “Transformer Math 101” | 10 To learn more about training memory calculation, check out EleutherAI’s “Transformer Math 101” (Anthony et al., April 2023). |
| https://arxiv.org/abs/2205.05198 | “Reducing Activation | memory needed for the model’s weights. |
| https://en.wikipedia.org/wiki/Floating-point_arithmetic | float numbers | Numerical values in neural networks are tra‐ ditionally represented as float numbers. |
| https://en.wikipedia.org/wiki/IEEE_754 | IEEE 754 | ditionally represented as float numbers. |
| https://oreil.ly/atIgi | “the secret to high performance on Cloud TPUs” | Precision bits are called significands. |
| https://oreil.ly/BGXtn | TPUs | BF16 was designed by Google to optimize AI performance on TPUs and TF32 was designed by NVIDIA for GPUs.11 Numbers can also be represented as integers. |
| https://oreil.ly/0pZgw | GPUs | BF16 was designed by Google to optimize AI performance on TPUs and TF32 was designed by NVIDIA for GPUs.11 Numbers can also be represented as integers. |
| https://x.com/abacaj/status/1695334296792264792?s=20 | x.com | See X and Threads discussions: 1; 2, 3, 4; and llama.cpp’s benchmark between BF16 and FP16, Bloke’s writeup, and Raschka’s writeup. |
| https://oreil.ly/U8L4d | oreil.ly | See X and Threads discussions: 1; 2, 3, 4; and llama.cpp’s benchmark between BF16 and FP16, Bloke’s writeup, and Raschka’s writeup. |
| https://oreil.ly/8ush1 | oreil.ly | See X and Threads discussions: 1; 2, 3, 4; and llama.cpp’s benchmark between BF16 and FP16, Bloke’s writeup, and Raschka’s writeup. |
| https://github.com/ggerganov/llama.cpp/pull/7150 | benchmark between BF16 and FP16 | See X and Threads discussions: 1; 2, 3, 4; and llama.cpp’s benchmark between BF16 and FP16, Bloke’s writeup, and Raschka’s writeup. |
| https://oreil.ly/0vuze | Bloke’s writeup | See X and Threads discussions: 1; 2, 3, 4; and llama.cpp’s benchmark between BF16 and FP16, Bloke’s writeup, and Raschka’s writeup. |
| https://oreil.ly/WK_zT | Raschka’s writeup | See X and Threads discussions: 1; 2, 3, 4; and llama.cpp’s benchmark between BF16 and FP16, Bloke’s writeup, and Raschka’s writeup. |
| https://arxiv.org/abs/2208.07339 | Dettmers et al. (2022) | For example, Dettmers et al. |
| https://arxiv.org/abs/2305.14314 | Dettmers et al., 2023 | For example, Dettmers et al. |
| https://oreil.ly/lqLfv | Apple | To serve models on the devices, Apple (2024) leveraged a quantization scheme that uses a mixture of 2-bit and 4-bit formats, averaging 3.5 bits-per-weight. |
| https://oreil.ly/FIP9V | Blackwell | Also in 2024, in anticipation of 4-bit neural networks, NVIDIA announced their new GPU architecture, Blackwell, that supports model inference in 4-bit float. |
| https://en.wikipedia.org/wiki/Minifloat | minifloat | that supports model inference in 4 bit float. |
| https://oreil.ly/V4pma | In early | In early 2020, it was acquired by Apple for a reported $200M. |
| https://arxiv.org/abs/1511.00363 | Courbariaux et al., 2015 | Quantization is effective, but there’a limit to how far it can go. |
| https://arxiv.org/abs/1603.05279 | Rastegari et al., 2016 | Quantization is effective, but there’a limit to how far it can go. |
| https://arxiv.org/abs/2310.11453 | Wang et al., 2023 | than 1 bit per value, and some have attempted the 1-bit representation, e.g., Binary‐ Connect (Courbariaux et al., 2015), Xnor-Net (Rastegari et al., 2016), and |
| https://arxiv.org/abs/2402.17764 | Ma et al. | Connect (Courbariaux et al., 2015), Xnor Net (Rastegari et al., 2016), and BitNet (Wang et al., 2023).19 In 2024, Microsoft researchers (Ma et al.) declared tha |
| https://oreil.ly/D-wIG | Hubara et al. (2016) | People attempted to train models in reduced precision as early as 2016; see Hubara et al. |
| https://arxiv.org/abs/1712.05877 | Jacob et al. (2017) | (2016) and Jacob et al. |
| https://oreil.ly/J7kVB | Character.AI (2024) | On the other hand, training a model directly in lower precision can help with both goals. |
| https://oreil.ly/pBaQM | mixed precision | However, training in lower precision is harder to do, as backpropgation is more sen sitive to lower precision.20 Lower-precision training is often done in mixed |
| https://oreil.ly/QL2gL | “Mixed Preci‐ | See “Mixed Preci‐ sion Training for NLP and Speech Recognition with OpenSeq2Seq” (Huyen et al., NVIDIA Developer Tech‐ nical Blog, October 2018). |
| https://arxiv.org/abs/2305.17888 | Liu et al., 2023 | For example, LLM-QAT (Liu et al., 2023) quantizes weights and activations into 4 bits but keeps embeddings in 16 bits. |
| https://oreil.ly/JZRsd | automatic mixed precision | The portions of the model that should be in lower precision can be set automatically using the automatic mixed precision (AMP) functionality offered by many ML |
| https://oreil.ly/Np1Hn | Rasley et al., 2020 | Instead of trying to fit the whole model on GPUs, you can offload the excess memory onto CPUs, as demonstrated by DeepSpeed (Rasley et al., 2020). |
| https://arxiv.org/abs/1902.00751 | Houlsby et al. (2019) | A study by Houlsby et al. |
| https://arxiv.org/abs/2106.09685 | Hu et al., 2021 | As of this writing, LoRA (Hu et al., 2021) is by far the most popular adapter-based method, and it will be the topic of the following section. |
| https://arxiv.org/abs/2106.10199 | Zaken et al., 2021 | Other adapter-based meth‐ ods include BitFit (Zaken et al., 2021), which came out around the same time LoRA did. |
| https://oreil.ly/avDPk | Liu et al., 2022 | Newer adapter methods include IA3 (Liu et al., 2022), whose efficient mixed-task batching strategy makes it particularly attractive for multi-task finetuning. |
| https://arxiv.org/abs/2309.12307 | Chen | LongLoRA (Chen et al., 2023) is a LoRA variant that incorporates attention-modification techniques to expand context length. |
| https://arxiv.org/abs/2101.00190 | Li and Liang, 2021 | For example, prefix tuning prepends soft prompt tokens to the input at every transformer layer, whereas prompt tuning |
| https://arxiv.org/abs/2103.10385 | Liu et al., 2021 | For example, prefix tuning prepends soft prompt tokens to the input at every transformer layer, whereas prompt tuning prepends soft prompt tokens to only the em |
| https://arxiv.org/abs/2104.08691 | Lester et al., 2021 | For example, prefix tuning prepends soft prompt tokens to the input at every transformer layer, whereas prompt tuning prepends soft prompt tokens to only the em |
| https://github.com/huggingface/peft | GitHub repository huggingface/peft | To get a sense of what PEFT methods are being used, I analyzed over 1,000 open issues on the GitHub repository huggingface/peft in October 2024. |
| https://arxiv.org/abs/1804.08838 | Li et al. (2018) | but only a few hundreds or thousands of examples to finetune it? |
| https://arxiv.org/abs/2012.13255 | Aghajanyan et al. (2020) | (2018); Aghajanyan et al. |
| https://oreil.ly/xzdiG | Sainath et al., 2013 | Throughout the 2010s, many people tried training low-rank neural networks, exem‐ plified in studies such as “Low-Rank Matrix Factorization for Deep Neural Netwo |
| https://oreil.ly/LHLNz | Povey et al., | Low-rank factorization has proven to be effective at smaller scales. |
| https://oreil.ly/BR63I | Jaderberg et al., 2014 | Orthogonal Low-Rank Matrix Factorization for Deep Neural Networks” (Povey et al., 2018), and “Speeding up Convolutional Neural Networks with Low Rank Expan‐ sio |
| https://arxiv.org/abs/1602.07360 | Iandola et al., 2016 | Low-rank factorization has proven to be effective at smaller scales. |
| https://arxiv.org/abs/2307.05695 | Lialin et al., 2023 | More recent attempts to train low-rank LLMs include ReLoRA (Lialin et al., 2023) and GaLore (Zhao et al., 2024). |
| https://arxiv.org/abs/2403.03507 | Zhao et al., 2024 | More recent attempts to train low-rank LLMs include ReLoRA (Lialin et al., 2023) and GaLore (Zhao et al., 2024). |
| https://arxiv.org/abs/2305.08252 | Dutt et al., 2023 | While there have been examples of LoRA with other architectures, such as convolu‐ tional neural networks (Dutt et al., 2023; Zhong et al., 2024; Aleem et al., 2 |
| https://arxiv.org/abs/2401.17868 | Zhong et al., 2024 | While there have been examples of LoRA with other architectures, such as convolu‐ tional neural networks (Dutt et al., 2023; Zhong et al., 2024; Aleem et al., 2 |
| https://arxiv.org/abs/2402.04964 | Aleem et al., 2024 | While there have been examples of LoRA with other architectures, such as convolu‐ tional neural networks (Dutt et al., 2023; Zhong et al., 2024; Aleem et al., 2 |
| https://oreil.ly/mqHMU | Williams et al., 2017 | Table 7-5 shows their results. |
| https://oreil.ly/82-jJ | Fireworks | How‐ ever, this constraint is unlikely due to performance and more likely due to their hardware’s memory con‐ straint. |
| https://oreil.ly/zzREV | Sooriyarachchi, 2023 | For example, Databricks showed that the biggest performance boost they got was from applying LoRA to all feedfor‐ ward layers (Sooriyarachchi, 2023). |
| https://arxiv.org/html/2404.05086v1 | Fomenko et al. (2024) | ing the feedforward matrices, yields better results. |
| https://oreil.ly/A-d5f | Raschka (2023) | Raschka (2023) found that r = 256 achieved the best performance on his tasks. |
| https://oreil.ly/vfXqE | LoRA adapters | Instead of having one big powerful model for multiple tasks, you can have one LoRA adapter for each task. |
| https://oreil.ly/T08JJ | Hugging Face | You can find them on Hugging Face26 or initia‐ tives like AdapterHub. |
| https://adapterhub.ml | AdapterHub | You can find them on Hugging Face26 or initia‐ tives like AdapterHub. |
| https://github.com/axolotl-ai-cloud/axolotl | Axolotl | PEFT frameworks—such as Hugging Face’s PEFT, Axolotl, unsloth, and LitGPT—likely support LoRA for popular base models right out of the box. |
| https://github.com/unslothai/unsloth | unsloth | PEFT frameworks—such as Hugging Face’s PEFT, Axolotl, unsloth, and LitGPT—likely support LoRA for popular base models right out of the box. |
| https://github.com/Lightning-AI/litgpt | LitGPT | PEFT frameworks—such as Hugging Face’s PEFT, Axolotl, unsloth, and LitGPT—likely support LoRA for popular base models right out of the box. |
| https://arxiv.org/abs/2309.14717 | Xu et al., 2023 | Other than QLoRA, quantized LoRA works include QA-LoRA (Xu et al., 2023), ModuLoRA (Yin et al., 2023), and IR-QLoRA (Qin et al., 2024). |
| https://arxiv.org/abs/2309.16119 | Yin et al., 2023 | Other than QLoRA, quantized LoRA works include QA-LoRA (Xu et al., 2023), ModuLoRA (Yin et al., 2023), and IR-QLoRA (Qin et al., 2024). |
| https://arxiv.org/abs/2402.05445 | Qin et al., 2024 | Other than QLoRA, quantized LoRA works include QA-LoRA (Xu et al., 2023), ModuLoRA (Yin et al., 2023), and IR-QLoRA (Qin et al., 2024). |
| https://oreil.ly/u_cVP | Designing Machine Learning Systems | 28 My book, Designing Machine Learning Systems has a section on “ML on the Cloud and on the Edge.” 348 \| Chapter 7: Finetuning |
| https://arxiv.org/abs/1612.00796 | Kirkpatrick et al., 2016 | Unfortunately, neural networks are prone to catastrophic forgetting (Kirkpatrick et al., 2016). |
| https://arxiv.org/abs/1602.05629 | McMahan et al., 2016 | device deployment can also significantly reduce inference costs. |
| https://en.wikipedia.org/wiki/Ensemble_learning | Wikipedia | the learning of all constituent models. |
| https://www.oreilly.com/library/view/designing-machine-learning/9781098107956/ | Designing Machine Learning Systems | Finetuning Techniques \| 349 |
| https://oreil.ly/hRV9P | Hugging Face’s Open | Just like model ensembles used to dominate leaderboards, many models on top of the Hugging Face’s Open LLM Leaderboard are merged models. |
| https://oreil.ly/eXC02 | Perrone, 1993 | Linear combination is often used in federated learning (Wang et al., 2020). |
| https://oreil.ly/ZKRPR | Wang et al., 2020 | Linear combination is often used in federated learning (Wang et al., 2020). |
| https://arxiv.org/abs/2203.05482 | Wortsman | Model soups (Wortsman et al., 2022) showed how averaging the entire weights of multiple finetuned models can improve accuracy without increasing inference time. |
| https://arxiv.org/abs/2212.04089 | Ilharco et al., 2022 | Task vectors allow us to do task arithmetic (Ilharco et al., 2022), such as adding two task vectors to combine task capabilities or subtracting a task vector to |
| https://arxiv.org/abs/1910.05653 | Singh and Jaggi, 2020 | While it makes sense to combine aligned parameters, aligning parameters can be challenging to do, and, therefore, this |
| https://arxiv.org/abs/2209.04836 | Ainsworth et al., 2022 | While it makes sense to combine aligned parameters, aligning parameters can be challenging to do, and, therefore, this approach is less common on naive linear c |
| https://arxiv.org/abs/2312.04339 | Tam et al., 2023 | While it makes sense to combine aligned parameters, aligning parameters can be challenging to do, and, therefore, this approach is less common on naive linear c |
| https://arxiv.org/abs/2306.01708 | Yadav | In the paper “TIES-Merging: Resolving Interference When Merging Models”, Yadav et al. |
| https://arxiv.org/abs/2311.03099 | Yu et al., 2023 | These redundant parameters, while not harmful to one model, might be harmful to the merged model. |
| https://oreil.ly/IM0Jc | Goliath-120B | One early success of frankenmerging is Goliath-120B (alpindale, 2023), which was merged from two finetuned Llama 2-70B models, Xwin and Euryale. |
| https://oreil.ly/URfbk | Xwin | One early success of frankenmerging is Goliath-120B (alpindale, 2023), which was merged from two finetuned Llama 2-70B models, Xwin and Euryale. |
| https://oreil.ly/Ftnxd | Euryale | One early success of frankenmerging is Goliath-120B (alpindale, 2023), which was merged from two finetuned Llama 2-70B models, Xwin and Euryale. |
| https://arxiv.org/abs/2212.05055 | Komatsuzaki et al., 2022 | Layer stacking can be used to train mixture-of-experts (MoE) models, as introduced in “Sparse Upcycling: Training Mixture-of-Experts from Dense Checkpoints” (Ko |
| https://arxiv.org/abs/2406.04692 | Wang et al., | MoE models trained from scratch. |
| https://arxiv.org/abs/2312.15166 | Kim et al. (2023) | which allows you to serve a bigger model. |
| https://oreil.ly/7I6Ch | OpenAI’s finetuning | OpenAI’s finetuning best practices document gives examples of two development paths: the progression path and the distillation path. |
| https://github.com/hiyouga/LLaMA-Factory | LLaMA-Factory | You can also finetune using one of many great finetuning frameworks available, such as LLaMA-Factory, unsloth, PEFT, Axolotl, and LitGPT. |
| https://github.com/microsoft/DeepSpeed | DeepSpeed | To finetune a model using more than one machine, you’ll need a framework that helps you do distributed training, such as DeepSpeed, PyTorch Distributed, and Col |
| https://oreil.ly/hxUAk | PyTorch Distributed | To finetune a model using more than one machine, you’ll need a framework that helps you do distributed training, such as DeepSpeed, PyTorch Distributed, and Col |
| https://oreil.ly/GFeC7 | “Ako: Decentralised Deep Learning with Partial Gradient Exchange” | explanations for why that’s the case. |
