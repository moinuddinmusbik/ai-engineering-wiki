---
title: "AI Engineering Architecture"
type: concept
created: 2026-04-27
updated: 2026-04-27
tags: [architecture, guardrails, router, gateway, caching, observability, monitoring, orchestration, drift]
sources: [ai-engineering-ch10.md]
---

# AI Engineering Architecture

The architecture of a production AI system is not a single design but a **progressive build-up**: start with the minimal viable loop and add components only as failure modes demand. This principle avoids premature complexity while providing a clear upgrade path as the system matures.

## The Progressive Architecture

### Stage 1: Bare Minimum
```
User Query → Model → Response
```
Works for simple, single-turn, low-stakes use cases. Establishes baseline performance. Fast to build, easy to debug.

### Stage 2: Context Enhancement
Add [[RAG]] to inject relevant knowledge:
```
User Query → Retriever → [Query + Context] → Model → Response
```
Reduces [[Hallucination]] on domain-specific questions. Introduces new failure modes: retrieval misses, irrelevant context injection, hallucination on retrieved content.

### Stage 3: Guardrails
Add input and output safety layers:
```
User Query → [Input Guardrails] → Retriever → Model → [Output Guardrails] → Response
                                                                              ↓ (on failure)
                                                                         Retry / Human Fallback
```
**Input guardrails**: filter, transform, or reject problematic inputs before they reach the model.
**Output guardrails**: validate model outputs before delivery to the user; trigger retry or fallback on failure.

### Stage 4: Router and Gateway
Add routing intelligence and a unified API layer:
```
User Query → Gateway → Router → [Specialized Model A / B / C] → Guardrails → Response
```
Router selects the appropriate model/path; gateway manages all model API calls centrally.

### Stage 5: Caching
Add caching layers to reduce latency and cost:
```
User Query → Cache Lookup → (hit) → Response
                         → (miss) → full pipeline → cache write → Response
```

### Stage 6: Agent Patterns
Add iterative loops and write actions for complex tasks. The model can call tools, modify external state, and iterate until a stopping condition.

## Guardrails in Detail

### Input Guardrails

**PII Detection and Masking**
Personally identifiable information (names, emails, SSNs, etc.) must not be sent to external model APIs in many compliance contexts. The **reverse PII map pattern**:
1. Detect PII in the user query
2. Replace each PII entity with a placeholder: "John Smith" → `[PERSON_1]`
3. Store the original→placeholder mapping in a reverse PII map
4. Send the masked query to the model
5. In the model response, replace placeholders back with originals using the reverse map

Clean, auditable, and keeps sensitive data off the wire.

**Prompt Injection Detection**
Detect attempts by malicious input to override system instructions. See [[Defensive Prompt Engineering]] for the broader defense framework.

### Output Guardrails

| Check | What It Catches |
|---|---|
| **Format validation** | Model output in wrong format (missing JSON fields, wrong structure) |
| **Factual consistency** | Claims inconsistent with retrieved context or known facts |
| **Toxicity detection** | Harmful, offensive, or policy-violating content |
| **Sensitivity checks** | PII in output, confidential information leakage |
| **Brand safety** | Mentions of competitors, off-brand claims |

**Failure handling**: on guardrail failure, the system can:
1. **Retry**: re-run the model (with or without modified prompt)
2. **Human fallback**: route to a human agent
3. **Safe response**: return a canned "I can't help with that" response

Multiple guardrails with different confidence thresholds allow nuanced policies (soft pass, uncertain → retry, definite violation → block).

## Router

The router is an **intent classifier** that sits upstream of the model pool. Functions:

- **Task routing**: direct different query types to specialized models (code model, customer service model, medical model, general model)
- **Out-of-scope detection**: identify queries outside the system's intended domain → graceful decline
- **Clarification requests**: detect ambiguous queries and ask for clarification before routing
- **Cost-based routing**: route simple queries to cheap/fast models; complex queries to powerful models

The router is itself a model (often a small classifier or embedding-based retriever) — its latency must be low relative to the main model's TTFT.

## Gateway

The gateway provides a **unified interface** for all model API calls within the system. Benefits:

- **Abstraction**: application code calls the gateway, not specific model APIs. Switching providers requires only gateway config changes.
- **Access control**: API key management, per-team or per-user quotas
- **Cost management**: budget caps, cost attribution per request
- **Rate limiting**: prevent runaway costs from bugs or attacks
- **Fallback policies**: if Model A returns an error or exceeds latency threshold, automatically fall back to Model B
- **Load balancing**: distribute requests across multiple API endpoints or providers
- **Logging**: centralized record of every model call

The gateway is the single entry point for all model interactions — every model call flows through it.

## Caching

### Exact Caching
Store (query hash → response) pairs. On cache hit, return stored response instantly.
- **Pros**: zero latency, zero model cost, perfectly deterministic
- **Cons**: requires exact query match; any variation misses the cache

Best for: FAQ systems, documentation lookup, fixed-template queries.

### Semantic Caching
Use [[Embeddings]] to find semantically similar cached queries:
1. Embed the incoming query
2. Search the cache with [[Vector Search]] (ANN, cosine similarity)
3. If a similar cached query exists (similarity > threshold), return its response

- **Pros**: much higher cache hit rate than exact matching
- **Cons**: adds retrieval latency; risk of false positives (wrong cached response for a different question)

Best for: chatbots and Q&A systems where users phrase the same question differently.

### Prompt Caching (Server-Side)
Providers like Anthropic cache the KV pairs for repeated prompt prefixes. The system prompt, few-shot examples, and retrieved documents that appear at the start of every request are computed once, cached, and reused. See [[Inference Optimization]].

- Up to **90% cost savings** for 100K-token prompts
- Up to **79% latency reduction**
- Requires structured prompts with stable prefixes

## Observability

Observability is a first-class architectural concern — not an afterthought.

### Key Reliability Metrics
| Metric | Definition |
|---|---|
| **MTTD** | Mean Time To Detect an incident |
| **MTTR** | Mean Time To Recover from an incident |
| **CFR** | Change Failure Rate — fraction of deploys that cause incidents |

### System Metrics to Track
- Format failure rate (output guardrail trigger rate)
- Factual consistency score (from [[AI as a Judge]])
- Toxicity rate
- TTFT and TPOT by percentile (p50, p95, p99)
- Cost per request and per user

### Logs vs Traces

**Logs**: append-only records of events within a single component. Efficient, queryable, but fragmented across components.

**Traces**: end-to-end records of a single request's journey across all components. A trace follows one user query from input guardrail → retriever → model call → output guardrail → response. Essential for debugging failures in multi-step [[Agents|agentic]] pipelines where the failure could have occurred at any step.

Tools: LangSmith, Weights & Biases, custom solutions. Traces are typically visualized as directed acyclic graphs (DAGs) or timeline views.

## Drift Detection

The system can degrade without any deployment changes. Three independent drift sources:

1. **System prompt drift**: the system prompt is modified (by an engineer, an experiment, a policy change). Even minor wording changes can shift model behavior significantly.

2. **User behavior drift**: the distribution of user queries changes over time — seasonality, new user cohorts, external events. A model optimized for last quarter's queries may be misaligned with this quarter's.

3. **Model drift**: the underlying model changes under you. Chen et al. (2023) found **significant behavioral differences between GPT-4 in March 2023 and June 2023** for the same prompts. Model providers update models without always notifying customers of behavioral changes.

Each drift source requires independent monitoring. A/B testing and shadow deployment are standard tools.

## User Feedback

### Feedback Signal Hierarchy

| Signal Type | Examples | Reliability |
|---|---|---|
| **Explicit** | Thumbs up/down, star ratings, corrections | High signal, low volume |
| **Implicit conversational** | Generation stop (user copied), turn count, retry behavior | Medium signal, high volume |
| **Implicit token** | Output length patterns, format adherence | Low signal, very high volume |

**Generation stop** (user stops the streaming response mid-generation) is a strong positive signal — the user found what they needed and copied it.

**Conversation turn count**: more turns on a single task → harder task, possibly model struggling. Tracking turn count identifies tasks the model handles poorly.

**Output token length**: unexpectedly short or long responses may indicate model confusion or instruction following failures.

## AI Pipeline Orchestration

Frameworks like **LangChain**, **LlamaIndex**, and **LangGraph** handle component definition and chaining. They abstract away:
- Prompt template management
- Retriever integration
- Tool call orchestration
- Memory management
- Retry logic

**Trade-off**: reduced boilerplate vs. added abstraction overhead. Debugging through multiple abstraction layers is harder. Understanding what the framework does under the hood is essential — treat frameworks as productivity tools, not black boxes.

## Connections

- [[RAG]] — the retrieval layer in the progressive architecture
- [[Agents]] — the loop and write-action patterns in Stage 6
- [[Defensive Prompt Engineering]] — guardrails in detail
- [[Inference Optimization]] — the performance layer supporting the architecture
- [[Evaluation Pipeline]] — observability metrics draw from evaluation methodology
- [[Memory]] — memory management within agent patterns
- [[Hallucination]] — output guardrails catch hallucinations before delivery
