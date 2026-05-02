---
title: "Memory"
type: concept
created: 2026-04-25
updated: 2026-04-25
tags: [memory, agents, context-management, infrastructure]
sources: [ai-engineering.pdf]
---

# Memory

Memory refers to mechanisms that allow a model to retain and utilize information. It is essential for knowledge-rich applications like [[RAG]] and multi-step applications like [[Agents]], but benefits any AI application that requires retaining information.

## Three Memory Mechanisms

### 1. Internal Knowledge
The model's parameters — knowledge from training data. Doesn't change unless the model is updated. Accessible in all queries.

*Human analogy: knowing how to breathe.*

### 2. Short-Term Memory (Context)
The model's context window. Previous messages in a conversation can be included, allowing the model to leverage them. Fast to access but capacity is limited by context length. Doesn't persist across tasks/sessions.

*Human analogy: the name of a person you just met.*

### 3. Long-Term Memory (External)
External data sources accessible via retrieval (as in RAG). Persists across sessions. Information can be added or deleted without updating the model. Capacity is practically unlimited.

*Human analogy: books, computers, notes.*

## Which Mechanism to Use

| Data Characteristic | Recommended Mechanism |
|----|-----|
| Essential for all tasks | Internal knowledge (training/finetuning) |
| Immediately relevant to current task | Short-term memory (context) |
| Rarely needed | Long-term memory (external storage) |

## Benefits of Memory Systems

1. **Manage information overflow** — when task execution generates information exceeding context limits, overflow goes to long-term memory
2. **Persist between sessions** — avoid re-explaining context every conversation (e.g., AI coach remembering your history)
3. **Boost consistency** — referencing previous answers lets the model calibrate future ones
4. **Maintain data structural integrity** — external memory can store structured data (tables, queues) that text context can't reliably preserve

## Memory Management

Two core operations: **add** and **delete** memory.

Long-term memory rarely needs deletion (cheap, extensible storage). Short-term memory is constrained by context length and requires a deletion strategy.

### Allocating Short-Term vs. Long-Term

The context window must be divided between:
- Short-term memory (conversation history, working information)
- Information retrieved from long-term memory

Example: if 30% of context is reserved for long-term retrieval, short-term memory gets at most 70%.

### FIFO (First In, First Out)

Simplest strategy. Oldest messages are removed first as context fills up. API providers like [[OpenAI]] may do this automatically; frameworks like LangChain allow retaining N last messages or N last tokens.

**Weakness**: Assumes early messages are least relevant — but early messages often carry the most information (e.g., stating the conversation's purpose). Can cause the model to lose track of critical information.

### Summarization-Based

Use a model to generate a **summary** of the conversation, replacing verbose history with compressed representation. Combined with named entity tracking, this approach can be highly effective.

Bae et al. (2022) extended this by developing a classifier that, for each sentence in the original memory and each sentence in the summary, determines whether one, both, or neither should be kept in the new memory.

### Reflection-Based

Liu et al. (2023) proposed that after each action, the agent:

1. **Reflects** on new information generated
2. **Decides** whether to:
   - **Insert** new information into memory
   - **Merge** with existing memory
   - **Replace** outdated/contradicting information

### Handling Contradictions

When encountering contradicting information:
- Some approaches keep the **newer** information
- Some use AI models to **judge** which to keep
- Some keep **both** to allow drawing from different perspectives
- How to handle contradiction depends on the use case

## Sources

- [[AI Engineering — Ch.6: RAG and Agents|AI Engineering Ch.6]]
