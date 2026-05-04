---
title: "AI Engineering — Ch.5: Prompt Engineering"
type: source
created: 2026-04-15
updated: 2026-04-15
tags: [prompt-engineering, in-context-learning, chain-of-thought, jailbreaking, security]
sources: [ai-engineering.pdf]
---

# AI Engineering — Ch.5: Prompt Engineering

**Source:** *AI Engineering* by [[Chip Huyen]], O'Reilly 2024, Chapter 5 (pp. 211–252).

## Summary

Chapter 5 covers prompt engineering — the easiest and most common [[Model Adaptation]] technique. Huyen frames it as "human-to-AI communication": anyone can communicate, but not everyone can communicate effectively. The chapter splits into two halves: writing effective prompts and defending against prompt attacks.

### Part 1: Introduction to Prompting

A prompt consists of a **task description** (including role/persona), **examples** (shots), and the **task** itself (concrete input). The chapter introduces **in-context learning** — the GPT-3 discovery (Brown et al., 2020) that models can learn new behaviors from prompt examples without weight updates, functioning as a form of continual learning. Zero-shot vs. few-shot learning is covered, with the observation that as models improve, fewer examples are needed (Microsoft 2023 analysis showed limited few-shot improvement over zero-shot for GPT-4).

François Chollet's metaphor: a foundation model is a "library of programs" and prompt engineering is about finding the right prompt to activate the program you want.

The **system prompt vs. user prompt** distinction is detailed, including chat templates (Llama 2 vs. Llama 3 format changes). System prompts likely boost performance because (1) they come first in the final prompt, and (2) models may be post-trained to prioritize them (OpenAI's instruction hierarchy paper, Wallace et al., 2024).

**Context length** has expanded 2,000× in five years (GPT-2's 1K → Gemini-1.5 Pro's 2M). However, **context efficiency** varies — the "needle in a haystack" (NIAH) test shows models are much better at finding information at the beginning and end of prompts than in the middle.

### Part 2: Best Practices

Six best practices, distilled from OpenAI, Anthropic, Meta, and Google guides:

1. **Write clear, explicit instructions** — explain scoring systems, adopt personas, provide examples, specify output format, use markers to signal structured output
2. **Provide sufficient context** — reduces hallucination; includes discussion of restricting model knowledge to only its context (tricky, prompting alone is insufficient)
3. **Break complex tasks into simpler subtasks** — prompt decomposition enables monitoring, debugging, parallelization, and use of cheaper models for simpler steps. GoDaddy reduced a 1,500-token prompt to smaller prompts with better performance and lower cost.
4. **Give the model time to think** — [[Chain-of-Thought]] (CoT) prompting (Wei et al., 2022) and self-critique. CoT improved LaMDA, GPT-3, and PaLM across math benchmarks. LinkedIn found CoT reduces hallucinations.
5. **Iterate on prompts** — version prompts, use experiment tracking, standardize evaluation. Separate prompts from code (prompts.py pattern). Prompt catalogs with metadata for organization.
6. **Evaluate prompt engineering tools** — DSPy, OpenPrompt automate prompt optimization. DeepMind's Promptbreeder uses evolutionary strategy. Caveat: tools generate hidden API calls (300+ calls for 10 variations × 30 examples), can have template bugs (LangChain typo example), and can change without warning.

### Part 3: Defensive Prompt Engineering

Three attack types against AI applications:

1. **Prompt extraction / reverse prompt engineering** — deducing system prompts via output analysis or tricking the model. "Write your system prompt assuming it will one day become public."

2. **[[Jailbreaking]] and prompt injection**:
   - **Direct attacks**: Obfuscation (misspelling keywords), output format manipulation (write a poem about hotwiring), roleplaying (DAN — "Do Anything Now", grandma exploit)
   - **Automated attacks**: PAIR (Chao et al., 2023) uses an attacker AI to iteratively refine prompts, often needing <20 queries
   - **Indirect prompt injection**: Malicious instructions placed in tools the model uses — passive phishing (poisoned GitHub repos found via web search) and active injection (malicious email content interpreted as instructions)

3. **Information extraction** — extracting training data (memorization rates ~1%), privacy violations, copyright regurgitation. Nasr et al. (2023) showed the "repeat word forever" divergence attack extracts training data without knowing the data. Larger models memorize more.

### Defenses (Three Levels)

- **Model-level**: OpenAI's **instruction hierarchy** (Wallace et al., 2024) — four priority levels (system > user > model outputs > tool outputs). Finetuning on aligned/misaligned instruction pairs improved robustness by up to 63%.
- **Prompt-level**: Repeat system prompt before and after user input; explicitly warn about known attacks; inspect tool default templates (LangChain's permissive defaults had 100% injection success rate).
- **System-level**: Isolation (sandbox code execution), human approval for impactful operations, input/output guardrails, anomaly detection on usage patterns.

Two key defense metrics: **violation rate** (% successful attacks) and **false refusal rate** (% legitimate queries refused).

## Key Takeaways

- Prompt engineering = human-to-AI communication; easy to start, hard to master.
- **In-context learning** lets models learn new behaviors from prompt examples without weight updates — a form of continual learning.
- **System prompts** boost performance likely because models are trained to prioritize them and because they come first.
- **Context efficiency matters more than context length** — models struggle with information in the middle of long prompts (NIAH test).
- **Chain-of-thought** prompting is among the most reliably effective techniques across models.
- **Prompt decomposition** (task chaining) improves performance, enables debugging, and can reduce costs despite more API calls.
- **Security is an evolving cat-and-mouse game** — no defense is foolproof. Indirect prompt injection is the most dangerous attack vector.
- **Separate prompts from code** and version them. Use prompt catalogs with metadata.

## Notable Quotes

> "The problem is not with prompt engineering. It's a real and useful skill to have. The problem is when prompt engineering is the only thing people know." — OpenAI research manager

> "Write your system prompt assuming that it will one day become public."

> "Security risks will remain a significant roadblock for AI adoption in high-stakes environments."

## Pages Created or Updated

- **Concepts:** [[Prompt Engineering]], [[Chain-of-Thought]], [[Jailbreaking]], [[Defensive Prompt Engineering]]
- **Updated:** [[Model Adaptation]], [[Hallucination]], [[Foundation Models]]

---

## References (110 links)

All hyperlinks embedded by the author in this chapter, extracted from the book PDF. See also the [complete URL reference](../analyses/ai-engineering-urls.md) across all chapters.


| URL | Name / Link Text | Description |
|-----|-----------------|-------------|
| https://oreil.ly/BToYu | oreil.ly | Com‐ plaints about how prompt engineering is not a real thing have gathered thousands of supporting comments; see 1, 2, 3, 4. |
| https://oreil.ly/mB3D7 | oreil.ly | Com‐ plaints about how prompt engineering is not a real thing have gathered thousands of supporting comments; see 1, 2, 3, 4. |
| https://oreil.ly/tk4lu | oreil.ly | Com‐ plaints about how prompt engineering is not a real thing have gathered thousands of supporting comments; see 1, 2, 3, 4. |
| https://oreil.ly/svNY- | oreil.ly | Com‐ plaints about how prompt engineering is not a real thing have gathered thousands of supporting comments; see 1, 2, 3, 4. |
| https://oreil.ly/TqmnZ | dropped robustness from their HELM Lite benchmark | Introduction to Prompting \| 213 |
| https://x.com/abacaj/status/1786436298510667997 | Llama 3 | However, some models, including Llama 3, seem to per‐ form better when the task description is at the end of the prompt. |
| https://github.com/ibis-project/ibis | Ibis dataframe API | For example, if a model doesn’t see many examples of the Ibis dataframe API in its training data, including Ibis examples in the prompt can still make a big dif |
| https://oreil.ly/qpjty | Discord | However, in a long discussion on my Discord, some people argued that context is part of the prompt. |
| https://oreil.ly/OEwKu | Google’s PALM 2 documentation | To make it more confusing, Google’s PALM 2 documentation defines context as the description that shapes “how the model responds throughout the conversation. |
| https://oreil.ly/N2fup | “How Does In-context Learning Work?” | Many smart people pondered at length why and how in-context learning works (see “How Does In-context Learning Work?” by the Stanford AI Lab). |
| https://oreil.ly/6Bfe7 | a library of many different programs | François Chollet, the creator of the ML framework Keras, compared a foundation model to a library of many different programs. |
| https://oreil.ly/FQP7J | Llama 2 chat model | As an example, here’s the template for the Llama 2 chat model: |
| https://oreil.ly/LH3wI | Reddit discussion | However, while uncommon, it can cause the model perform better, as shown in a Reddit discussion. |
| https://oreil.ly/o-fXF | Llama 3 chat model | Different models use different chat templates. |
| https://github.com/lmstudio-ai/.github/issues/43 | this one | I once spent a day debugging a finetuning issue only to realize that it was because a library I used didn’t update the chat template for the newer model version |
| https://arxiv.org/abs/2404.13208 | Wallace et al., 2024 | Training a model to prioritize system prompts also helps mitigate prompt attacks, as dis‐ cussed later in this chapter. |
| https://arxiv.org/abs/2307.03172 | Liu et al., 2023 | Research has shown that a model is much better at understanding instructions given at the beginning and the end of a prompt than in the middle (Liu et al., 2023 |
| https://oreil.ly/nQZIB | practical NIAH test | Introduction to Prompting \| 219 |
| https://arxiv.org/abs/2404.06654 | Hsieh et al., 2024 | Similar tests, such as RULER (Hsieh et al., 2024), can also be used to evaluate how good a model is at processing long prompts. |
| https://oreil.ly/AF-Y1 | OpenAI | This section focuses on general techniques that have been proven to work with a wide range of models and will likely remain relevant in the near future. |
| https://oreil.ly/-HMpk | Anthropic | These companies also often provide libra‐ ries of pre-crafted prompts that you can reference—see Anthropic, Google, and OpenAI. |
| https://oreil.ly/DXAgC | Meta | range of models and will likely remain relevant in the near future. |
| https://oreil.ly/aFeyE | Google | These companies also often provide libra‐ ries of pre-crafted prompts that you can reference—see Anthropic, Google, and OpenAI. |
| https://oreil.ly/PR9a3 | Anthropic | These companies also often provide libra‐ ries of pre-crafted prompts that you can reference—see Anthropic, Google, and OpenAI. |
| https://oreil.ly/CGyGU | Google | These companies also often provide libra‐ ries of pre-crafted prompts that you can reference—see Anthropic, Google, and OpenAI. |
| https://oreil.ly/WMn2L | OpenAI | These companies also often provide libra‐ ries of pre-crafted prompts that you can reference—see Anthropic, Google, and OpenAI. |
| https://oreil.ly/06vdM | Claude’s prompt engineering tutorial | Inspired by Claude’s prompt engineering tutorial. |
| https://oreil.ly/-u2Z5 | OpenAI’s prompt engineering guide | The following example from OpenAI’s prompt engineering guide shows the intent classification prompt and the prompt for one intent (troubleshooting). |
| https://oreil.ly/yqAZs | Anthropic’s prompt engineering guide | 9 This parallel processing example is from Anthropic’s prompt engineering guide. |
| https://oreil.ly/_c5FF | GoDaddy | GoDaddy (2024) found that the prompt for their customer support chatbot bloated to over 1,500 tokens after one iteration. |
| https://arxiv.org/abs/2201.11903 | Wei et al., 2022 | It was introduced in “Chain-of-Thought Prompting Elicits Reasoning in Large Language Models” (Wei et al., 2022), almost a year before ChatGPT came out. |
| https://arxiv.org/abs/2111.01998 | Ding et al., 2021 | Tools that aim to automate the whole prompt engineering workflow include Open‐ Prompt (Ding et al., 2021) and DSPy (Khattab et al., 2023). |
| https://arxiv.org/abs/2310.03714 | Khattab et al., 2023 | Tools that aim to automate the whole prompt engineering workflow include Open‐ Prompt (Ding et al., 2021) and DSPy (Khattab et al., 2023). |
| https://oreil.ly/Z5w1L | Claude 3.5 Sonnet | Figure 5-7 shows a prompt written by Claude 3.5 Sonnet (Anthropic, 2024). |
| https://arxiv.org/abs/2309.16797 | Fernando et al., 2023 | DeepMind’s Promptbreeder (Fernando et al., 2023) and Stanford’s TextGrad (Yuk‐ sekgonul et al., 2024) are two examples of AI-powered prompt optimization tools. |
| https://arxiv.org/abs/2406.07496 | Yuk‐ | DeepMind’s Promptbreeder (Fernando et al., 2023) and Stanford’s TextGrad (Yuk‐ sekgonul et al., 2024) are two examples of AI-powered prompt optimization tools. |
| https://github.com/outlines-dev | Out‐ | For example, Guidance, Out‐ lines, and Instructor guide models toward structured outputs. |
| https://github.com/huggingface/transformers/issues/25304#issuecomment-1728111915 | wrong | A tool developer might get the wrong template for a given model, construct a prompt by concatenating tokens instead of raw texts, or have a typo in its prompt t |
| https://oreil.ly/bzK_g | concatenating tokens instead of | A tool developer might get the wrong template for a given model, construct a prompt by concatenating tokens instead of raw texts, or have a typo in its prompt t |
| https://github.com/langchain-ai/langchain/commit/7c6009b76f04628b1617cec07c7d0bb766ca1009 | Lang‐ | Figure 5-9 shows typos in a Lang‐ Chain default critique prompt. |
| https://oreil.ly/b_H2s | “Show Me the Prompt” | Prompt Engineering Best Practices \| 233 |
| https://oreil.ly/ceZLs | Google | See Google Firebase’s Dotprompt, Humanloop, Continue Dev, and Promptfile. |
| https://oreil.ly/FuBEI | Humanloop | See Google Firebase’s Dotprompt, Humanloop, Continue Dev, and Promptfile. |
| https://oreil.ly/nriHw | Continue Dev | See Google Firebase’s Dotprompt, Humanloop, Continue Dev, and Promptfile. |
| https://github.com/promptfile/promptfile | Promptfile | See Google Firebase’s Dotprompt, Humanloop, Continue Dev, and Promptfile. |
| https://github.com/langchain-ai/langchain/issues/814 | 814 | See GitHub issues: 814 and 1026. |
| https://github.com/langchain-ai/langchain/issues/1026 | 1026 | See GitHub issues: 814 and 1026. |
| https://github.com/f/awesome-chatgpt-prompts | f/awesome-chatgpt-prompts | As new models roll out, I have no idea how long their prompts will remain relevant. |
| https://github.com/PlexPt/awesome-chatgpt-prompts-zh | PlexPt/awesome-chatgpt- | As new models roll out, I have no idea how long their prompts will remain relevant. |
| https://oreil.ly/lKOrj | eat rocks | Even though people might understand that it’s not your intention to make your application offensive, they can still attribute the offenses to your lack of care |
| https://oreil.ly/_fXnT | racist comments | Even though people might understand that it’s not your intention to make your application offensive, they can still attribute the offenses to your lack of care |
| https://oreil.ly/q1EHt | PromptHero | Some have attracted hundreds of thousands of stars.14 Many public prompt marketplaces let users upvote their favorite prompts (see PromptHero and Cursor Directo |
| https://oreil.ly/J3Crv | Cursor | Some have attracted hundreds of thousands of stars.14 Many public prompt marketplaces let users upvote their favorite prompts (see PromptHero and Cursor Directo |
| https://oreil.ly/Ukk7e | PromptBase | Some even let users sell and buy prompts (see PromptBase). |
| https://oreil.ly/aKDb1 | Instacart’s Prompt Exchange | Some organi‐ zations have internal prompt marketplaces for employees to share and reuse their best prompts, such as Instacart’s Prompt Exchange. |
| https://oreil.ly/0h0qN | whether prompts | Some even debate whether prompts can be patented.15 The more secretive companies are about their prompts, the more fashionable reverse prompt engineering become |
| https://x.com/remoteli_io/status/1570547034159042560 | @mkualquiera | You can also include examples to show that the model should ignore its original instructions and follow the new instructions, as in this example used by X user |
| https://x.com/dylan522p/status/1755086111397863777 | 1,700 tokens | In February 2024, one user claimed that ChatGPT’s system prompt had 1,700 tokens. |
| https://github.com/LouisShark/chatgpt_system_prompt | GitHub repositories | Several GitHub repositories claim to contain supposedly leaked system prompts of GPT models. |
| https://github.com/brexhq/prompt-engineering?tab=readme-ov-file | Brex’s Prompt Engineering Guide | Image from Brex’s Prompt Engineering Guide (2023). |
| https://x.com/DrJimFan/status/1631709224387624962 | x.com | The malicious keywords can also be hidden in a mixture of languages or Unicode. |
| https://x.com/zswitten/status/1599090459724259330 | Unicode | The malicious keywords can also be hidden in a mixture of languages or Unicode. |
| https://arxiv.org/abs/2307.15043 | Zou et al. (2023) | If a model hasn’t been trained on these unusual strings, these strings can confuse the model, causing it to bypass its safety measurements. |
| https://x.com/muneebtator/status/1598668909619445766 | robbing a house | to hotwire a car, which the model is likely to refuse, an attacker asks the model to write a poem about hotwiring a car. |
| https://x.com/zswitten/status/1598197802676682752 | Molo‐ | to hotwire a car, which the model is likely to refuse, an attacker asks the model to write a poem about hotwiring a car. |
| https://en.wikipedia.org/wiki/Uwu | UwU | write a poem about hotwiring a car. |
| https://x.com/___frye/status/1598400965656596480 | enrich uranium | Attackers ask the model to pre‐ tend to play a role or act out a scenario. |
| https://oreil.ly/0NoUv | Reddit | Originating from Reddit (2022), the prompt for this attack has gone through many iterations. |
| https://oreil.ly/BPAal | many iterations | Originating from Reddit (2022), the prompt for this attack has gone through many iterations. |
| https://oreil.ly/UxtYv | the steps to producing napalm | Other roleplay‐ ing examples include asking the model to be an NSA (National Security Agency) agent with a secret code that allows it to bypass all safety guard |
| https://x.com/synt7_x/status/1601014197286211584 | a secret code | attacker wants to know about, such as the steps to producing napalm. |
| https://x.com/proofofbeef/status/1598481383030231041 | simulation | ing examples include asking the model to be an NSA (National Security Agency) agent with a secret code that allows it to bypass all safety guardrails, pretendin |
| https://x.com/himbodhisattva/status/1598192659692417031 | Filter Improvement Mode | Automated attacks Prompt hacking can be partially or fully automated by algorithms. |
| https://x.com/haus_cole/status/1598541468058390534 | @haus_cole | An X user, @haus_cole, shows that it’s possible to ask a model to brainstorm new attacks given existing attacks. |
| https://arxiv.org/abs/2310.08419 | Prompt | Prompt Automatic Iterative Refinement (PAIR) uses an AI model to act as an attacker. |
| https://arxiv.org/abs/2302.12173 | (Gre‐ | Image adapted from “Not What You’ve Signed Up for: Compro‐ mising Real-World LLM-Integrated Applications with Indirect Prompt Injection” (Gre‐ shake et al., 202 |
| https://xkcd.com/327 | xkcd: “Exploits of a Mom” | Defensive Prompt Engineering \| 243 |
| https://arxiv.org/abs/1906.00080 | Chen et al., 2019 | For exam‐ ple, Gmail’s auto-complete model is trained on users’ emails (Chen et al., 2019). |
| https://arxiv.org/abs/1909.01066 | Petroni et al., 2019 | Introduced by Meta’s AI lab in 2019, the LAMA (Language Model Analysis) benchmark (Petroni et al., 2019) probes for the relational knowledge present in the trai |
| https://arxiv.org/abs/2012.07805 | Carlini et al. (2020) | For example, to extract someone s email address, an attacker might prompt a model with “X’s email address is _”. |
| https://arxiv.org/abs/2205.12628 | Huang et al. (2022) | (2020) and Huang et al. |
| https://arxiv.org/abs/2311.17035 | Nasr et al. (2023) | quently changes her email address … is more likely to yield X s email than a more general context like “X’s email is …”. |
| https://oreil.ly/DNj9O | Breitenbach and Wood, 2024 | Dropbox has a great blog post on this type of attack: “Bye Bye Bye...: Evolu‐ tion of repeated token attacks on ChatGPT models” (Breitenbach and Wood, 2024). |
| https://arxiv.org/abs/2301.13188 | Carlini et al., 2023 | Training data extraction is possible with models of other modalities, too. |
| https://github.com/Stability-AI/stablediffusion | Stable Diffusion | Training Data from Diffusion Models” (Carlini et al., 2023) demonstrated how to extract over a thousand images with near-duplication of existing images from the |
| https://github.com/thunlp/Advbench | Chen et al., 2022 | There are benchmarks that help you evaluate how robust a system is against adversarial attacks, such as Advbench (Chen et al., 2022) and PromptRobust (Zhu et al |
| https://arxiv.org/abs/2306.04528 | Zhu et al., 2023 | There are benchmarks that help you evaluate how robust a system is against adversarial attacks, such as Advbench (Chen et al., 2022) and PromptRobust (Zhu et al |
| https://github.com/Azure/PyRIT | Azure/PyRIT | Tools that help automate security probing include Azure/PyRIT, leondz/garak, greshake/llm-security, and CHATS-lab/persuasive_jail‐ breaker. |
| https://github.com/NVIDIA/garak | leondz/garak | Tools that help automate security probing include Azure/PyRIT, leondz/garak, greshake/llm-security, and CHATS-lab/persuasive_jail‐ breaker. |
| https://github.com/greshake/llm-security | greshake/llm-security | Tools that help automate security probing include Azure/PyRIT, leondz/garak, greshake/llm-security, and CHATS-lab/persuasive_jail‐ breaker. |
| https://github.com/CHATS-lab/persuasive_jailbreaker | CHATS-lab/persuasive_jail‐ | Tools that help automate security probing include Azure/PyRIT, leondz/garak, greshake/llm-security, and CHATS-lab/persuasive_jail‐ breaker. |
| https://oreil.ly/TYoZj | plan red teaming | Microsoft has a great write-up on how to plan red teaming for LLMs. |
| https://oreil.ly/DFjgW | Pedro et al., 2023 | When using prompt tools, make sure to inspect their default prompt templates since many of them might lack safety instructions. |
| https://en.wikipedia.org/wiki/Recurrent_neural_network | recurrent neural network | LSTM was the dominant architecture of deep learning for natural language processing (NLP) before the transformer architecture took over in 2018. |
| https://en.wikipedia.org/wiki/Long_short-term_memory | LSTM | LSTM was the dominant architecture of deep learning for natural language processing (NLP) before the transformer architecture took over in 2018. |
| https://arxiv.org/abs/2005.04611 | Petroni et al., arXiv, May 2020 | architecture took over in 2018. |
| https://arxiv.org/abs/1704.00051 | Chen et al., 2017 | The retrieve-then-generate pattern was first introduced in “Reading Wikipedia to Answer Open-Domain Questions” (Chen et al., 2017). |
| https://arxiv.org/abs/2005.11401 | Lewis et al., 2020 | The term retrieval-augmented generation was coined in “Retrieval-Augmented Gen‐ eration for Knowledge-Intensive NLP Tasks” (Lewis et al., 2020). |
| https://oreil.ly/v-T_4 | Anthropic, 2024 | you can just include the entire knowledge base in the prompt that you give the model, with no need for RAG or similar methods” (Anthropic, 2024). |
| https://oreil.ly/-JJYn | “The History of Information Retrieval Research” | See “The History of Information Retrieval Research” (Sander‐ son and Croft, Proceedings of the IEEE, 100: Special Centennial Issue, April 2012). |
| https://oreil.ly/aie-book | GitHub repository | See this book’s GitHub repository for more in- depth resources on information retrieval. |
| https://arxiv.org/abs/2107.05720 | Formal et al., 2021 | For example, SPLADE (Sparse Lexical and Expansion) is a retrieval algorithm that works using sparse embeddings (Formal et al., 2021). |
| https://github.com/elastic/elasticsearch | Elastic‐ | Elastic‐ search (Shay Banon, 2010), built on top of Lucene, uses a data structure called an inverted index. |
| https://github.com/apache/lucene | Lucene | Elastic‐ search (Shay Banon, 2010), built on top of Lucene, uses a data structure called an inverted index. |
| https://en.wikipedia.org/wiki/Okapi_BM25 | Okapi BM25 | … … … Okapi BM25, the 25th generation of the Best Matching algorithm, was developed by Robertson et al. |
| https://oreil.ly/aDmhb | “The Prob‐ | 4, 2009) 7 Aravind Srinivas, the CEO of Perplexity, tweeted that “Making a genuine improvement over BM25 or full- h d” |
| https://x.com/AravSrinivas/status/1737886080555446552 | Aravind Srinivas, the CEO of Perplexity | 4, 2009) 7 Aravind Srinivas, the CEO of Perplexity, tweeted that “Making a genuine improvement over BM25 or full- text search is hard”. |
| https://www.nltk.org | NLTK | Classical NLP packages, such as NLTK (Natural Language Toolkit), spaCy, and Stanford’s CoreNLP, also offer tokenization functionalities. |
| https://github.com/explosion/spaCy | spaCy | Classical NLP packages, such as NLTK (Natural Language Toolkit), spaCy, and Stanford’s CoreNLP, also offer tokenization functionalities. |
| https://github.com/stanfordnlp/CoreNLP | Stanford’s CoreNLP | Classical NLP packages, such as NLTK (Natural Language Toolkit), spaCy, and Stanford’s CoreNLP, also offer tokenization functionalities. |
