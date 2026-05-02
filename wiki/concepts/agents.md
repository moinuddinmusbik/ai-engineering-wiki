---
title: "Agents"
type: concept
created: 2026-04-25
updated: 2026-04-25
tags: [agents, tools, autonomy, core-pattern]
sources: [ai-engineering.pdf]
---

# Agents

An agent is anything that can **perceive its environment and act upon it**. In AI engineering, agents are AI-powered systems that use [[Foundation Models]] as their "brain" to process information, plan actions, and determine task completion. Agents are characterized by two things:

1. **Environment** — defined by the use case (a game, the internet, a codebase, a kitchen)
2. **Actions** — augmented by the **tools** the agent has access to

> The field of artificial intelligence research is "the study and design of rational agents." — Russell & Norvig, *AI: A Modern Approach* (1995)

## RAG as a Simple Agent

[[RAG]] systems are simple agents where retrievers (text, image, SQL) are the tools. The agentic pattern extends beyond context construction to enable models to take actions in the world.

## Why Agents Need Powerful Models

Two reasons agents require stronger models than non-agent use cases:

1. **Compound mistakes** — accuracy degrades multiplicatively across steps:
   - 95% per-step accuracy → 60% over 10 steps → **0.6% over 100 steps**
2. **Higher stakes** — with tool access, failures can have more severe consequences

## Tool Categories

An agent's **tool inventory** determines its capabilities. Three categories:

### 1. Knowledge Augmentation
Tools that provide context — retrievers, web browsing, APIs (search, news, GitHub, social media), internal people search, inventory APIs, Slack retrieval, email readers.

Web browsing prevents models from going **stale** (when training data becomes outdated). But it also opens agents to internet-sourced risks.

### 2. Capability Extension
Tools that address inherent model limitations:
- **Simple**: Calculator, calendar, timezone/unit converter, translator
- **Code interpreters**: Execute code, analyze results — enables coding assistant, data analyst, research assistant roles. Carries code injection risk (see [[Defensive Prompt Engineering]])
- **Multimodal tools**: Text-to-image (DALL-E), image captioning, transcription, OCR for PDFs
- **Chameleon** (Lu et al., 2023): GPT-4 + 13 tools improved ScienceQA by **11.37%** and TabMWP by **17%** over GPT-4 alone

### 3. Write Actions
Tools that **modify** the environment (vs. read-only actions):
- SQL executor can retrieve data (read) or change/delete tables (write)
- Email API can read or respond to emails
- Banking API can check balance or initiate transfers

Write actions enable full workflow automation (customer outreach, order processing, etc.) but require serious trust and security measures:

> "Just as you shouldn't give an intern the authority to delete your production database, you shouldn't allow an unreliable AI to initiate bank transfers."

## Function Calling

Many model providers support **function calling** (tool use):
1. **Declare** tools with function name, parameters, documentation
2. **Specify** which tools are available per query (required / none / auto)
3. Model generates tool calls with parameters automatically

Some APIs guarantee valid function names but **cannot guarantee correct parameter values**.

> Always ask the system to report parameter values for each function call. Inspect them to make sure they are correct.

## Tool Selection

No foolproof guide exists. Approaches to finding the right set:
- Compare agent performance with different tool sets
- **Ablation studies** — remove tools one at a time to measure impact
- Identify tools the agent frequently makes mistakes on
- Plot **tool call distributions** — different models have different preferences (GPT-4 prefers wider tool sets; ChatGPT favors image captioning)

**Tool transition**: After tool X, how likely is tool Y? Frequently co-used tools can be combined into bigger tools.

**Voyager** (Wang et al., 2023): A skill manager that tracks new skills (tools) the agent acquires. Successfully used skills are added to a skill library for reuse on future tasks.

## Agent Failure Modes

See [[Agent Planning]] for planning-specific failures. Additional failure modes:

### Tool Failures
- Tool gives wrong output (e.g., wrong image caption, wrong SQL query)
- Translation errors (when converting natural language plans to executable commands)
- Missing tools for the task domain

### Efficiency
Track to evaluate efficiency:
- Steps to complete a task (average)
- Cost per task (average)
- Time per action (identify bottlenecks)

Compare with baselines (other agents or human operators), keeping in mind that humans and AI have different operational modes.

## Sources

- [[AI Engineering — Ch.6: RAG and Agents|AI Engineering Ch.6]]
