# Domain 1 — Solution Design & Architecture

> **Exam weight: 17%** · *"Model, architecture, and API-pattern selection for the problem at hand."*
> Together with [Integration](03-integration.md) (19%) and [Evaluation](04-evaluation-testing-optimization.md) (16%), this is over half the exam.

## Why it matters

CCAR-P is an **architect's** exam, not a builder's. The questions start from a *business* situation — a claims backlog, a support cost line, an SLA — and ask which design fits and **why**. The trap answer is almost always the most sophisticated one. You get points for choosing the *simplest* design that meets the requirement and being able to say what would change your mind.

## Task statements (what the guide says you'll be asked to do)

- Translate business problems into Claude-based AI solutions
- Align solutions to **business value pillars**: efficiency, transformation, productivity, cost, performance/SLAs
- Design **end-to-end** architectures: input → processing → output → **feedback loops**
- Select architectural patterns: **workflow · agentic · augmented LLM**
- Design multi-agent systems and orchestration strategies
- Apply decomposition techniques to complex problems

## Key concepts

### From business problem to architecture

Work in this order — skipping a step is how you end up with an agent nobody needed:

1. **What's the business outcome?** Cost per ticket, hours saved, revenue unblocked, risk reduced. Name the pillar.
2. **What's the success metric, and what's the failure cost?** A wrong answer in marketing copy ≠ a wrong answer in a refund decision. This sets your guardrail budget.
3. **What are the constraints?** Latency SLA, throughput, data residency, budget per unit of work, existing systems.
4. **What's the simplest architecture that clears all three?** Then justify anything more complex.
5. **How does it learn?** The feedback loop — logging, evals, human corrections flowing back — is part of the architecture, not an afterthought.

### The three pattern families

| Pattern | What it is | Choose when | Cost of choosing wrong |
| ------- | ---------- | ----------- | ---------------------- |
| **Augmented LLM** | One model + retrieval + tools + memory. The building block everything else composes. | A single well-scoped step: classify, extract, draft, answer-with-context | Too little structure → no reliability story |
| **Workflow** | LLM calls on a **predefined** path you control | Steps are known and ordered in advance | Rigid where the task genuinely varies |
| **Agentic** | The model **directs its own process** — chooses tools and steps at runtime | The path can't be known up front; open-ended investigation | Cost, latency, non-determinism, hard to debug |

**The rule: use the simplest thing that works.** And note that these compose — the exam-grade answer is often *"a workflow, with one agentic sub-step where the investigation is open-ended."*

### Orchestration patterns

| Pattern | What | Use when |
| ------- | ---- | -------- |
| **Prompt chaining** | Output of step N feeds step N+1 | Fixed sequential subtasks; add a programmatic gate between steps |
| **Routing** | Classify input → dispatch to a specialized handler | Distinct categories need different handling (and different *models*) |
| **Parallelization** | Fan out concurrently — *sectioning* (split the work) or *voting* (same work, N times) | Subtasks are independent, or you want consensus on a risky call |
| **Orchestrator–workers** | A lead decomposes and delegates at runtime | The number/shape of subtasks **isn't knowable up front** |
| **Evaluator–optimizer** | One model generates, another critiques, loop | Clear quality criteria exist and iteration measurably helps |

> **The most-tested distinction:** parallelization = subtasks known statically. Orchestrator–workers = subtasks decided at runtime. If you can write the list of subtasks before the request arrives, you don't need an orchestrator.

### Multi-agent: when it earns its cost

- **Wins:** large parallelizable search/analysis, genuinely separate concerns, distinct tool sets per agent, context isolation (a subagent's 200-file search doesn't pollute the lead's window).
- **Costs:** token spend multiplies (context is duplicated per subagent), coordination and result-merging are new failure surfaces, traces are non-deterministic and hard to debug, guardrails must be enforced in N loops instead of one.
- **Lead/subagent shape:** lead decomposes → specialized subagents (search / analyse / synthesize) run with their own tools → lead **merges**. Always have an explicit merge strategy; "the lead figures it out" is not one.
- **Say the quiet part:** for most enterprise tasks, a single well-equipped agent or a workflow beats multi-agent. Know *when it isn't worth it*.

### Decomposition techniques

- **By stage** (extract → validate → decide → act) — natural for chaining.
- **By concern** (retrieval vs. reasoning vs. formatting) — lets you use a cheap model where it's enough.
- **By entity** (per document, per customer, per repo) — natural fan-out for parallelization.
- **By confidence** (fast path vs. escalated path) — routing; most volume takes the cheap path.

Good decomposition is what makes a system **evaluable** — each piece gets its own metric and its own test set.

### Feedback loops (the part people forget)

Input → processing → output → **feedback**: log every decision with its inputs, capture human corrections and thumbs-down as labelled data, feed that into your [eval set](04-evaluation-testing-optimization.md), and close the loop with prompt/retrieval/model changes. An architecture with no path from production failure back to a fix is not a production architecture.

## You must be able to

- [ ] Turn a business problem into an architecture and **name the value pillar** it serves.
- [ ] Choose **augmented LLM vs. workflow vs. agentic** and defend it on cost, latency, predictability, and failure cost.
- [ ] Pick the right **orchestration pattern** for a scenario — especially parallelization vs. orchestrator–workers.
- [ ] Decompose a complex problem into evaluable pieces with clear hand-offs.
- [ ] Say **when multi-agent is *not* worth it**, with three concrete costs.
- [ ] Design the **feedback loop**, not just the happy path.
- [ ] State what would **change your mind** about the design — the exam rewards conditional judgment.

## Resources

- Anthropic — *Building Effective Agents* (the canonical pattern reference).
- Anthropic — *How we built our multi-agent research system* (lead/subagent in practice, with the honest cost numbers).
- Companion notes: [Loop Engineering](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/loop-engineering.md) · [Durable Execution](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/durable-execution.md)

## 🔎 Self-check

1. A task has 4 fixed, ordered steps. Agent or workflow? Which pattern — and what single change would flip your answer?
2. When would you choose **orchestrator–workers** over **parallelization (sectioning)**?
3. Name three costs of multi-agent, and one scenario where you'd still pay them.
4. A stakeholder asks for "an AI agent for our claims team." What five questions do you ask before designing anything?
5. Which **business value pillar** does each of these serve: a triage router, a code-review bot, a document-extraction pipeline?
6. Where does the **feedback loop** attach in an end-to-end design, and what flows through it?
7. Give a scenario where **evaluator–optimizer** is right — and one where it just burns tokens.
