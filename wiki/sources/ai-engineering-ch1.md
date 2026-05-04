---
title: "AI Engineering — Ch.1: Introduction to Building AI Applications with Foundation Models"
type: source
created: 2026-04-07
updated: 2026-04-07
tags: [ai-engineering, foundation-models, llm, overview, chip-huyen]
sources: [ai-engineering.pdf]
---

# Ch.1: Introduction to Building AI Applications with Foundation Models

**Book:** AI Engineering by [[Chip Huyen]] (O'Reilly, 2024)
**Pages:** 25–71

## Summary

This chapter frames the emergence of **[[AI Engineering]]** as a new discipline born from the convergence of three forces: increasingly capable [[Foundation Models]], massive investment in AI, and dramatically lower barriers to building AI applications via model-as-a-service APIs.

Huyen traces the evolution from early [[Language Models]] through [[Large Language Models]] (enabled by [[Self-Supervision]]) to multimodal [[Foundation Models]], and finally to the current era where engineers build applications *on top of* these models rather than training them from scratch.

The chapter surveys eight categories of AI use cases — [[Coding with AI]], image/video production, writing, education, [[Conversational Bots]], [[Information Aggregation]], data organization, and [[Workflow Automation]] — with data from 205 open-source GitHub projects and 50+ enterprise interviews.

It closes with a detailed comparison of the **[[AI Engineering Stack]]** (three layers: application development, model development, infrastructure) and how AI engineering differs from traditional [[ML Engineering]].

## Key Takeaways

- **Scale defines post-2020 AI.** Models are consuming nontrivial portions of global electricity and we're at risk of running out of public internet data to train them.
- **Self-supervision was the unlock.** Language models can infer labels from input data itself, removing the data-labeling bottleneck and enabling massive scale-up.
- **Foundation models are general-purpose.** Unlike task-specific models, a single foundation model can do sentiment analysis, translation, coding, and more. Adapt via [[Prompt Engineering]], [[RAG]], or [[Finetuning]].
- **The "last mile" problem is real.** Going 0→60 is easy (weekend demo), 60→100 is exceedingly hard. LinkedIn took 1 month for 80%, then 4 more months to reach 95%.
- **AI engineering ≠ ML engineering.** AI engineering is less about model training, more about *adapting and evaluating* models. Evaluation is harder because outputs are open-ended.
- **The AI stack has three layers:** Application Development (prompts, eval, interfaces), Model Development (training, datasets, inference optimization), Infrastructure (serving, compute, monitoring).
- **Full-stack engineers have an edge** in the new paradigm — the ability to quickly prototype and iterate is more valuable than deep ML knowledge for many use cases.
- **Maintenance is a moving target.** The AI space moves so fast that the best option today may be the worst tomorrow. Cost, regulation, and IP risks add complexity.

## Notable Data Points

- GitHub Copilot crossed $100M ARR within 2 years of launch.
- Midjourney hit $200M ARR at 1.5 years old (late 2023).
- Goldman Sachs estimated AI investment approaching $200B globally by 2025.
- McKinsey: AI makes developers 2x productive for documentation, 25–50% for code generation, minimal improvement for highly complex tasks.
- Eloundou et al. (2023): occupations with ~100% AI exposure include interpreters, tax preparers, web designers, writers.

## Notable Quotes

> "The journey from 0 to 60 is easy, whereas progressing from 60 to 100 becomes exceedingly challenging." — Ding et al. (UltraChat, 2023)

> "AI engineering is just software engineering with AI models thrown in the stack." — Anton Bacaj

## Connections

- Sets up the rest of the book: Ch.2 (foundation models), Ch.3–4 (evaluation), Ch.5 (prompt engineering), Ch.6 (RAG & agents), Ch.7 (finetuning), Ch.8 (datasets), Ch.9 (inference), Ch.10 (architecture & feedback).
- The [[Model Adaptation]] trichotomy (prompt engineering / RAG / finetuning) is the central organizing principle of the book.

---

## References (94 links)

All hyperlinks embedded by the author in this chapter, extracted from the book PDF. See also the [complete URL reference](../analyses/ai-engineering-urls.md) across all chapters.


| URL | Name / Link Text | Description |
|-----|-----------------|-------------|
| https://oreil.ly/J0IyO | a nontrivial portion | If I could use only one word to describe AI post-2020, it’d be scale. |
| https://arxiv.org/abs/2211.04325 | running out of publicly available internet data | The scaling up of AI models has two major consequences. |
| https://en.wikipedia.org/wiki/The_Adventure_of_the_Dancing_Men | “The Adventure of the Dancing Men” | In the 1905 story “The Adventure of the Dancing Men”, Sherlock Holmes leveraged simple statistical information of English to decode sequences of mysterious stic |
| https://oreil.ly/G_HBp | “Prediction and Entropy of Printed English” | His work on how to model English was published in his 1951 landmark paper “Prediction and Entropy of Printed English”. |
| https://oreil.ly/0QI91 | OpenAI website | You can see how different OpenAI models tokenize text on the OpenAI website. |
| https://oreil.ly/EYccr | ¾ the length of a word | For GPT-4, an average token is approximately ¾ the length of a word. |
| https://oreil.ly/bxMcW | Mixtral 8x7B | The Mixtral 8x7B model has a vocabulary size of 32,000. |
| https://github.com/openai/tiktoken/blob/main/tiktoken/model.py | 100,256 | GPT-4’s vocabulary size is 100,256. |
| https://oreil.ly/h0Y8x | causal language models | 3 Autoregressive language models are sometimes referred to as causal language models. |
| https://arxiv.org/abs/1810.04805 | Devlin et al., 2018 | A well-known example of a masked language model is bidirectional encoder representations from transform‐ ers, or BERT (Devlin et al., 2018). |
| https://oreil.ly/WEQFj | Krizhevsky et al., 2012 | The model that started the deep learning revolution, AlexNet (Krizhevsky et al., 2012), was supervised. |
| https://oreil.ly/EVXJl | Amazon SageMaker Ground Truth | For example, as of September 2024, Amazon SageMaker Ground Truth charges 8 cents per image for labeling fewer than 50,000 images, but only 2 cents per image for |
| https://oreil.ly/NoGX7 | noted in their GPT-4V system card | even understand videos, 3D assets, protein structures, and so on. |
| https://arxiv.org/abs/2108.07258 | foundation models | Foundation models mark a breakthrough from the traditional structure of AI research. |
| https://oreil.ly/zcqdu | CLIP (OpenAI, 2021) | For example, OpenAI used a variant of self- supervision called natural language supervision to train their language-image model CLIP (OpenAI, 2021). |
| https://arxiv.org/abs/2204.07705 | Wang et al., 2022 | Figure 1-4 shows the tasks used by the Super-NaturalInstructions benchmark to eval‐ uate foundation models (Wang et al., 2022), providing an idea of the types o |
| https://oreil.ly/okMw6 | Goldman Sachs Research | Goldman Sachs Research estimated that AI investment could approach $100 bil‐ lion in the US and $200 billion globally by 2025.9 AI is often mentioned as a com‐ |
| https://oreil.ly/tgm-a | FactSet | FactSet found that one in three S&P 500 companies mentioned AI in their earnings calls for the second quarter of 2023, three times more than did so the year ear |
| https://oreil.ly/fK5uh | an average of a 4.6% | 9 For comparison the entire US expenditures for public elementary and secondary schools are around $900 According to WallStreetZen, companies that mentioned AI |
| https://oreil.ly/r86Qz | Japan | In a September 2022 interview, Sam Altman, CEO of OpenAI, said that the biggest opportunity for the vast majority of people will be to adapt these models for sp |
| https://oreil.ly/IUcVg | UAE | In a September 2022 interview, Sam Altman, CEO of OpenAI, said that the biggest opportunity for the vast majority of people will be to adapt these models for sp |
| https://oreil.ly/D9QBM | Sam Altman, CEO of OpenAI | In a September 2022 interview, Sam Altman, CEO of OpenAI, said that the biggest opportunity for the vast majority of people will be to adapt these models for sp |
| https://oreil.ly/m8SvB | on average 75% each month | ComputerWorld declared that “teaching AI to behave is the fastest-growing career skill”. |
| https://oreil.ly/47sGE | ComputerWorld | ComputerWorld declared that “teaching AI to behave is the fastest-growing career skill”. |
| https://theresanaiforthat.com/ | theresanaiforthat.com | 10 Fun fact: as of September 16, 2024, the website theresanaiforthat.com lists 16,814 AIs for 14,688 tasks and 4,803 jobs. |
| https://oreil.ly/-k_QX | Amazon Web Services (AWS) | For example, Amazon Web Services (AWS) has categorized enterprise generative AI use cases into three buckets: customer experience, employee productivity, and pr |
| https://oreil.ly/Kul5E | 2024 O’Reilly survey | A 2024 O’Reilly survey categorized the use cases into eight categories: programming, data analysis, customer support, marketing copy, other copy, research, web |
| https://oreil.ly/T272_ | Deloitte | Some organizations, like Deloitte, have categorized use cases by value capture, such as cost reduction, process efficiency, growth, and accelerating innovation. |
| https://oreil.ly/OyIUP | Gartner | For value capture, Gartner has a category for business continuity, meaning an organization might go out of business if it doesn’t adopt generative AI. |
| https://arxiv.org/abs/2303.10130 | Eloundou et al. (2023) | (2023) has excellent research on how exposed different occupations are to AI. |
| https://huyenchip.com/llama-police | list of open source AI applications | You can find the list of open source AI applications that I track. |
| https://en.wikipedia.org/wiki/Memex | Memex | Talk-to-your-docs Market research Data organization Image search Memex Knowledge management Document processing Workflow automation Travel planning Event planni |
| https://oreil.ly/XWeDt | 2024 a16z Growth report | The enterprise world generally prefers applications with lower risks. |
| https://oreil.ly/Xamik | annual recurring revenue crossed $100 million | One of the earliest successes of foundation models in production is the code comple‐ tion tool GitHub Copilot, whose annual recurring revenue crossed $100 milli |
| https://oreil.ly/t0xDf | Magic raising $320 million | As of this writing, AI-powered coding startups have raised hundreds of millions of dollars, with Magic raising $320 million and Anysphere rais‐ ing $60 million, |
| https://oreil.ly/BW5Hk | Anysphere rais‐ | As of this writing, AI-powered coding startups have raised hundreds of millions of dollars, with Magic raising $320 million and Anysphere rais‐ ing $60 million, |
| https://github.com/gpt-engineer-org/gpt-engineer | gpt-engineer | Open source coding tools like gpt-engineer and screenshot-to-code both got 50,000 stars on GitHub within a year, and many more are being rapidly introduced. |
| https://github.com/abi/screenshot-to-code | screenshot-to-code | Open source coding tools like gpt-engineer and screenshot-to-code both got 50,000 stars on GitHub within a year, and many more are being rapidly introduced. |
| https://github.com/reworkd/AgentGPT | AgentGPT | Here are examples of these tasks: • Extracting structured data from web pages and PDFs (AgentGPT) • Converting English to code (DB-GPT, SQL Chat, PandasAI) • Gi |
| https://github.com/eosphoros-ai/DB-GPT | DB-GPT | • Extracting structured data from web pages and PDFs (AgentGPT) • Converting English to code (DB-GPT, SQL Chat, PandasAI) • Given a design or a screenshot, gene |
| https://github.com/sqlchat/sqlchat | SQL Chat | • Extracting structured data from web pages and PDFs (AgentGPT) • Converting English to code (DB-GPT, SQL Chat, PandasAI) • Given a design or a screenshot, gene |
| https://github.com/Sinaptik-AI/pandas-ai | PandasAI | • Extracting structured data from web pages and PDFs (AgentGPT) • Converting English to code (DB-GPT, SQL Chat, PandasAI) • Given a design or a screenshot, gene |
| https://github.com/sawyerhood/draw-a-ui | draw-a-ui | • Converting English to code (DB-GPT, SQL Chat, PandasAI) • Given a design or a screenshot, generating code that will render into a website that looks like the |
| https://github.com/joshpxyne/gpt-migrate | GPT- | • Given a design or a screenshot, generating code that will render into a website that looks like the given image (screenshot-to-code, draw-a-ui) • Translating |
| https://github.com/mckaywrigley/ai-code-translator | AI Code Translator | that looks like the given image (screenshot-to-code, draw-a-ui) • Translating from one programming language or framework to another (GPT- Migrate, AI Code Trans |
| https://github.com/context-labs/autodoc | Autodoc | • Translating from one programming language or framework to another (GPT- Migrate, AI Code Translator) • Writing documentation (Autodoc) • Creating tests (Pente |
| https://github.com/GreyDGL/PentestGPT | PentestGPT | g , ) • Writing documentation (Autodoc) • Creating tests (PentestGPT) • Generating commit messages (AI Commits) |
| https://github.com/Nutlope/aicommits | AI Commits | g ( ) • Creating tests (PentestGPT) • Generating commit messages (AI Commits) |
| https://oreil.ly/zUpGu | Jensen | At one end of the spectrum, Jensen Huang, CEO of NVIDIA, predicts that AI will replace human software engineers and that we should stop saying kids should learn |
| https://oreil.ly/Hz_3i | AWS | In a leaked recording, AWS CEO Matt Garman shared that in the near future, most developers will stop coding. |
| https://oreil.ly/aqUmX | McKin‐ | McKin‐ sey researchers found that AI can help developers be twice as productive for docu‐ mentation, and 25–50% more productive for code generation and code ref |
| https://oreil.ly/D0-yA | “Marketing Budgets Vary by | See “Marketing Budgets Vary by Industry” (Christine Moorman, WSJ, 2017). |
| https://oreil.ly/EAzCl | Midjourney | In late 2023, at one and a half years old, Midjourney had already gener‐ ated $200 million in annual recurring revenue. |
| https://oreil.ly/fZLVg | increase their chances of landing a job | Many candidates believe that AI-generated headshots can help them put their best foot forward and increase their chances of landing a job. |
| https://oreil.ly/WNqUw | Facebook | In 2019, Facebook banned accounts using AI-generated profile photos for safety reasons. |
| https://oreil.ly/IzQ6F | Noy and | It’s not a surprise that LLMs are good at writing, given that they are trained for text completion. |
| https://arxiv.org/abs/2305.09857 | Grammarly | Grammarly, a writing assistant app, finetunes a model to make users’ writing more fluent, coherent, and clear. |
| https://oreil.ly/LB72P | New York Times | In 2023, the New York Times reported that Amazon was flooded with shoddy AI-generated travel guidebooks, each outfitted with an author bio, a website, and rave |
| https://oreil.ly/mZKjr | NewsGuard | In June 2023, NewsGuard identified almost 400 ads from 141 popular brands on junk AI-generated websites. |
| https://oreil.ly/pqI5z | ban ChatGPT | plaining about being unable to complete their homework. |
| https://oreil.ly/nxtzw | reversed their decisions | Instead of banning AI, schools could incorporate it to help students learn faster. |
| https://oreil.ly/C8kmI | Pajak and Bicknell (Duolingo, 2022) | Pajak and Bicknell (Duolingo, 2022) found that out of four stages of course creation, lesson personalization is the stage that can benefit the most from AI, as |
| https://oreil.ly/tC7-g | Khan Academy | For example, Khan Academy offers AI-powered teaching assistants to students and course assistants to teachers. |
| https://oreil.ly/_N1JR | AI-powered | For example, Khan Academy offers AI-powered teaching assistants to students and course assistants to teachers. |
| https://oreil.ly/Y-hBW | students have been turning to AI for | their lunches taken by AI. |
| https://oreil.ly/dZbym | here | Many are already spending more time talking to bots than to humans (see the discus‐ sions here and here). |
| https://oreil.ly/svWj8 | here | Many are already spending more time talking to bots than to humans (see the discus‐ sions here and here). |
| https://oreil.ly/SNme7 | ruin | Some are worried that AI will ruin dating. |
| https://oreil.ly/Jbt4R | dating | Some are worried that AI will ruin dating. |
| https://arxiv.org/abs/2304.03442 | Park et | In research, people have also found that they can use a group of conversational bots to simulate a society, enabling them to conduct studies on social dynamics |
| https://oreil.ly/yn-DN | Inworld | One use case of AI-powered 3D characters is smart NPCs, non-player characters (see NVIDIA’s demos of Inworld and Convai).16 NPCs are essential for advancing the |
| https://oreil.ly/zAHwz | Convai | One use case of AI-powered 3D characters is smart NPCs, non-player characters (see NVIDIA’s demos of Inworld and Convai).16 NPCs are essential for advancing the |
| https://oreil.ly/74soT | Salesforce’s 2023 | According to Salesforce’s 2023 Generative AI Snapshot Research, 74% of generative AI users use it to distill complex ideas and summarize information. |
| https://oreil.ly/Qq5-g | Instacart | When Instacart launched an internal prompt marketplace, it discovered that one of the most popular prompt tem‐ plates is “Fast Breakdown”. |
| https://oreil.ly/vnDNK | $12.81 billion by 2030 | It’s estimated that the IDP, intelligent data processing, industry will reach $12.81 billion by 2030, growing 32.9% each year. |
| https://oreil.ly/gqi3d | Gartner study | In the 2023 Gartner study, 7% cited business continuity as their reason for embracing AI. |
| https://oreil.ly/Dz1HE | Apple | Apple has a great document explaining different ways AI can be used in a product. |
| https://oreil.ly/JW4_A | Crawl-Walk-Run | Microsoft (2023) proposed a framework for gradually increasing AI automation in products that they call Crawl-Walk-Run: 1. |
| https://arxiv.org/abs/2305.14233 | Ding et al. (2023) | However, a good initial demo doesn t promise a good end product. |
| https://www.linkedin.com/blog/engineering/generative-ai/musings-on-building-a-generative-ai-product | LinkedIn (2024) | (2023) shared that “the journey from 0 to 60 is easy, whereas progressing from 60 to 100 becomes exceedingly challenging.” LinkedIn (2024) shared the same senti |
| https://arxiv.org/abs/2009.03300 | Hendrycks et al., | Model inference, the process of computing an output given an input, is getting faster and cheaper. |
| https://oreil.ly/UyL8r | Katrina | Image from Katrina Nguyen (2024). |
| https://oreil.ly/eDfB8 | $9 billion | The introduction of Europe’s General Data Protection Regulation (GDPR), for exam‐ ple, was estimated to cost businesses $9 billion to become compliant. |
| https://oreil.ly/eYTmr | US October 2023 Executive Order | Compute avail‐ ability can change overnight as new laws put more restrictions on who can buy and sell compute resources (see the US October 2023 Executive Order |
| https://oreil.ly/AhANP | incredible compensation packages | The AI Engineering Stack \| 41 |
| https://oreil.ly/G3LUh | 98% of the overall compute and data resources | For the InstructGPT model, pre-training takes up to 98% of the overall compute and data resources. |
| https://oreil.ly/0VqmX | Business Insider article | I read a Business Insider article where the author said she trained ChatGPT to mimic her younger self. |
| https://oreil.ly/gGXZ- | 100 ms latency | As users are getting notoriously impatient, getting AI applications’ latency down to the 100 ms latency expected for a typical internet application is a huge ch |
| https://oreil.ly/VDwaR | CoT@32 | Goo‐ gle had evaluated Gemini using a prompt engineering technique called CoT@32. |
| https://github.com/langchain-ai/langchainjs | LangChain.js | lar, but there is also increasing support for JavaScript APIs, with LangChain.js, Transformers.js, OpenAI’s Node library, and Vercel’s AI SDK. |
| https://github.com/huggingface/transformers.js | Transformers.js | lar, but there is also increasing support for JavaScript APIs, with LangChain.js, Transformers.js, OpenAI’s Node library, and Vercel’s AI SDK. |
| https://github.com/openai/openai-node | OpenAI’s Node library | lar, but there is also increasing support for JavaScript APIs, with LangChain.js, Transformers.js, OpenAI’s Node library, and Vercel’s AI SDK. |
| https://github.com/vercel/ai | Vercel’s AI SDK | lar, but there is also increasing support for JavaScript APIs, with LangChain.js, Transformers.js, OpenAI’s Node library, and Vercel’s AI SDK. |
| https://oreil.ly/OOZK- | (Shawn Wang, 2023 | Image recreated from “The Rise of the AI Engineer” (Shawn Wang, 2023). |
