# Domain 1 — Agentic Architecture & Orchestration

> **Exam weight: 27% (the largest)** · Give this the most time. It's the difference between "I can call the API" and "I can architect a production agent system."

## Why it matters

The exam tests whether you can **choose the right agentic design for a problem** and orchestrate it reliably — not just wire up a single prompt. Expect scenario questions of the form *"given this requirement, which architecture/pattern fits, and why?"*

## Key concepts

### Workflows vs. agents
- **Workflow** — LLM + tools follow a **predefined** path (you control the steps). Predictable, cheaper, easier to debug.
- **Agent** — the LLM **dynamically directs its own process**, choosing tools and steps at runtime. More flexible, more expensive, harder to constrain.
- **The rule:** use the *simplest* thing that works. Prefer a workflow; reach for a full agent only when the task genuinely needs open-ended, dynamic decision-making.

### The core building block
- The **augmented LLM** = model + retrieval + tools + memory. Everything else composes this.
- The agent loop: **gather context → decide/act (tool call) → observe result → repeat → stop**. Know what makes a *good stopping condition*.

### Orchestration patterns (know each + when to use it)
| Pattern | What | Use when |
| ------- | ---- | -------- |
| **Prompt chaining** | Output of step N feeds step N+1 | A task decomposes into fixed sequential subtasks |
| **Routing** | Classify input, send to a specialized handler | Distinct categories need different handling |
| **Parallelization** | Run subtasks concurrently (sectioning or voting) | Independent subtasks, or you want multiple votes |
| **Orchestrator–workers** | A lead model decomposes and delegates to workers | Subtask count/shape isn't known up front |
| **Evaluator–optimizer** | One model generates, another critiques + loops | Quality has clear criteria and iteration helps |

### Multi-agent systems
- **When multi-agent wins:** large parallelizable search/analysis, separation of concerns, distinct tool sets per agent.
- **Costs:** more tokens, coordination overhead, harder debugging. Don't reach for it by default.
- **Lead/subagent pattern:** an orchestrator decomposes the task, spins up specialized subagents (e.g. search → analyze → synthesize), and merges results.

### Reliability & control
- **Guardrails**, **human-in-the-loop** gates for consequential/irreversible actions, **bounded execution** (max steps/tokens/time), and clear **stopping conditions**.
- **Cost/latency trade-offs** — agents can spend a lot; design budgets and escalation.

## You must be able to

- [ ] Decide **workflow vs. agent** for a given requirement and justify it.
- [ ] Pick the right **orchestration pattern** (chaining / routing / parallelization / orchestrator–workers / evaluator–optimizer) for a scenario.
- [ ] Decompose a task into **subagents** with clear roles, tools, and hand-offs.
- [ ] Say **when multi-agent is *not* worth it**.
- [ ] Add **guardrails, human-in-the-loop, and stopping conditions** to an agent design.
- [ ] Reason about **cost, latency, and failure modes** of an agentic design.

## Resources

- Anthropic — *Building Effective Agents* (the canonical patterns reference).
- Anthropic — *How we built our multi-agent research system* (lead/subagent pattern in practice).
- Companion notes: [Loop Engineering](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/loop-engineering.md), [Durable Execution](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/durable-execution.md) (orchestration/reliability).

## 🔎 Self-check

1. A task has 4 fixed, ordered steps. Agent or workflow? Which pattern?
2. When would you choose **orchestrator–workers** over **parallelization (sectioning)**?
3. Name three costs of going multi-agent instead of single-agent.
4. What are three good **stopping conditions** for an agent loop?
5. Where in an agent design do you insert **human-in-the-loop**, and why there?
6. Give a scenario where **evaluator–optimizer** is the right pattern.
7. A support agent keeps looping without resolving. Which reliability controls were missing?
