---
title: "Agent Planning"
type: concept
created: 2026-04-25
updated: 2026-04-25
tags: [agents, planning, reasoning, react, reflexion]
sources: [ai-engineering.pdf]
---

# Agent Planning

Planning is the core intellectual challenge of [[Agents]]. The output of planning is a **plan** — a roadmap outlining the steps needed to accomplish a task. Effective planning requires the model to understand the task, consider different options, and choose the most promising one.

## Decoupling Planning from Execution

Planning should be **separated** from execution to avoid fruitless runs. The three-stage process:

1. **Plan generation** — the model proposes a plan
2. **Plan validation** — heuristics or AI judges evaluate the plan
   - Eliminate plans with invalid actions (tools not in inventory)
   - Eliminate plans exceeding a step threshold
   - Ask an AI judge if the plan is reasonable
3. **Execution** — only validated plans are executed via function calling

This creates a **multi-agent system**: one agent generates plans, one validates, one executes.

To speed up: generate several plans in parallel and have the evaluator pick the best one (latency/cost trade-off).

## Intent Classification

An **intent classifier** helps agents choose the right tools by understanding what the user is trying to do. It can:
- Be a separate prompt or a trained classification model
- Classify requests as IRRELEVANT to avoid wasting compute on impossible tasks
- Route queries to appropriate tool sets (billing query → payment tools; password query → documentation tools)

## FM vs. RL Planners

| Aspect | Foundation Model Planner | Reinforcement Learning Planner |
|--------|-------------------------|-------------------------------|
| Planner | The model itself | Trained by RL algorithm |
| Training cost | Lower (prompting or finetuning) | High (extensive training) |
| Flexibility | Can be steered via prompts | Requires retraining for new tasks |

> "I suspect that in the long run, FM agents and RL agents will merge."

### Can Foundation Models Plan?

An open question. Yann LeCun states "autoregressive LLMs can't plan." Kambhampati (2023) argues LLMs extract knowledge but don't truly plan — "The plans that come out of LLMs may look reasonable to the lay user, and yet lead to execution time interactions and errors."

Counterarguments:
- Autoregressive models **can** effectively backtrack by revising plans when a path fails
- LLMs may be poor planners because they lack proper **tooling** (state tracking, outcome prediction)
- An LLM can be a **world model** that predicts action outcomes (Hao et al., 2023)
- Even if AI can't plan alone, it can be **part of** a planner (augmented with search tools and state tracking)

## Plan Generation

### Via Prompting

Provide the model with:
- Available actions/tools with descriptions
- Examples of (task → plan) mappings
- Ask it to output a sequence of actions

### Via Function Calling

Model APIs that support tool use effectively turn models into agents. Each tool is declared with its function name, parameters, and documentation. The model generates tool calls automatically.

### Improving Planning

- Write better system prompts with more examples
- Give better tool/parameter descriptions
- Refactor complex functions into simpler ones
- Use a stronger model
- Finetune for plan generation

## Planning Granularity

A plan can be generated at different levels of detail:

### Fine-grained (Function Names)
```
[get_time, fetch_top_products, fetch_product_info, generate_query, generate_response]
```
- Easier to execute, harder to generate
- Brittle: tool API changes require prompt/example/finetuning updates

### Coarse-grained (Natural Language)
```
1. get current date
2. retrieve the best-selling product last week
3. retrieve product information
```
- Easier to generate, more robust to API changes
- Requires a **translator** to convert to executable commands (simpler task than planning itself)

### Hierarchical Planning
Generate high-level plan first, then expand each step into sub-plans. Circumvents the planning/execution trade-off.

## Complex Plans (Control Flows)

Plans beyond simple sequential execution:

- **Sequential** — task B after task A (dependency)
- **Parallel** — tasks A and B simultaneously (e.g., fetching prices for 100 products)
- **If statement** — task B or C depending on previous output (e.g., buy or sell based on earnings report)
- **For loop** — repeat task A until condition met (e.g., generate random numbers until prime)

Non-sequential control flows are harder to generate and translate. When evaluating agent frameworks, check what control flows they support — parallel execution can significantly reduce latency.

## Reflection and Error Correction

### ReAct (Yao et al., 2022)

Interleaves **reasoning and action** at each step:
```
Thought 1: [planning/reasoning]
Act 1: [take action]
Observation 1: [analyze result/reflect]
...
Thought N: [final reasoning]
Act N: Finish [response]
```

### Reflexion (Shinn et al., 2023)

Separates reflection into two modules:
1. **Evaluator** — scores the outcome (e.g., "failed ⅓ of test cases")
2. **Self-reflection** — analyzes what went wrong (e.g., "didn't handle negative arrays")
3. Agent generates a new plan (trajectory) incorporating the reflection

**Trade-off**: Both ReAct and Reflexion add significant cost and latency from generating thoughts, observations, and actions. They also require many examples in the prompt, reducing context space.

## Human-in-the-Loop

Humans can participate at any stage:
- **Provide** a high-level plan for the agent to expand
- **Validate** plans before execution
- **Execute** risky operations (database updates, code merges)

Requires clearly defining the **level of automation** for each action.

## Planning Failures

The most common failure modes:

1. **Invalid tool** — plan references a tool not in the inventory
2. **Valid tool, invalid parameters** — wrong number or type of parameters
3. **Valid tool, incorrect parameter values** — right tool, right parameters, wrong values
4. **Goal failure** — plan doesn't solve the task or ignores constraints
5. **Reflection errors** — agent believes it succeeded when it hasn't

### Evaluation Metrics

Create a planning dataset of (task, tool inventory) tuples. For K generated plans, measure:
- % of valid plans
- Average plans needed to get a valid one
- % of valid tool calls
- Frequency of each failure type
- Analyze patterns: what tasks fail most? What tools are hardest to use?

## Sources

- [[AI Engineering — Ch.6: RAG and Agents|AI Engineering Ch.6]]
