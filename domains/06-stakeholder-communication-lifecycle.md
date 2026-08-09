# Domain 6 — Stakeholder Communication & Lifecycle Management

> **Exam weight: 14%** · *"Explaining and defending decisions; managing the system over time."*
> The other domain absent from Foundations — and the one that most distinguishes Professional. The exam guide's own framing of the role is *"defend those design decisions to the stakeholders who fund and approve them."*

## Why it matters

An architect's output isn't only a diagram — it's a decision other people agree to fund, approve, and operate. These questions read differently from the technical ones: *"the CFO asks why this costs 3× the last quote — what do you tell them?"*, *"the pilot works and the business wants production next week — what do you say?"* The right answer is almost never purely technical, and almost never "just build it."

## Task statements

- Conduct **structured discovery** and requirements gathering
- **Communicate architectural decisions** to technical and non-technical stakeholders
- Manage the **full lifecycle** — discovery → design → pilot → production → iteration
- Set expectations about capability, cost, and risk

## Key concepts

### Discovery — the questions before the architecture

Scenario stems usually hide the real requirement. Structured discovery surfaces it:

1. **What's the business outcome, and how is it measured today?** If nobody can state the current baseline, you can't prove improvement later.
2. **Who does this work now, and what does it cost?** Your baseline for ROI and for change management.
3. **What does "good enough" look like?** The accuracy bar and the **cost of being wrong** — these set your guardrail and eval budgets.
4. **What are the hard constraints?** Latency SLA, volume, budget, data residency, systems of record, security review.
5. **Who has to approve this, and what will they ask?** Security, legal, compliance, procurement — find them in discovery, not at go-live.
6. **What data exists, who owns it, and can we use it for this?** Access, quality, and permission are three separate questions.
7. **What happens when it fails?** Defines your escalation path and your human-in-the-loop design.

**Anti-patterns the exam punishes:** designing before the outcome is defined; accepting "make it an AI agent" as a requirement; skipping the "who approves" question; promising an accuracy number before seeing data.

### Communicating to each audience

| Audience | What they care about | Lead with |
| -------- | -------------------- | --------- |
| **Executive sponsor** | Business value, cost, risk, timeline | The outcome and the number — "cuts handling time 40%, ~$X/month, human approval on refunds over $Y" |
| **Finance** | Unit economics, predictability, what drives the bill | **Cost per outcome** (per ticket, per document) and the levers that move it |
| **Security / compliance** | Data flow, access, retention, auditability, blast radius | The data-flow diagram and the control at each layer |
| **Engineering** | Architecture, interfaces, failure modes, operability | Patterns, contracts, trade-offs, and how it's monitored |
| **End users / ops** | Does it help me? Do I lose control? Is my job changing? | What it does, when it hands back to them, and how to override it |

**The technique that scores:** translate up, don't dumb down. "We use orchestrator–workers with subagents" becomes "we split research across parallel workers, which is why it's faster but costs more per query — here's the trade-off, and here's the cap."

### Defending a design decision

Have the structure ready:

> **Decision** → **the requirement it serves** → **the alternatives considered and why they lost** → **the trade-off accepted** → **what would change my mind**.

That last clause is what separates a defensible decision from a dogmatic one, and it's what a scenario asking "how do you respond?" is looking for. **Write the decisions down.** An architecture decision record — context, decision, alternatives, consequences — is how a design survives the architect leaving.

### The hard conversations, and the correct answers

| They say | You say |
| -------- | ------- |
| "Can it be 100% accurate?" | No — it's probabilistic. Here's the measured accuracy on our eval set, here's what happens on the 3% (escalation path), and here's the cost of being wrong. |
| "Why is it 3× the estimate?" | Here's cost-per-outcome vs. the baseline, what drives it (model tier, tokens, retries), and the levers: routing, caching, batch. Then show the ROI, not just the bill. |
| "The demo worked — ship it Monday." | A demo isn't an eval. Here's what's still needed: eval set, security review, monitoring, rollback. Here's a scoped pilot we *can* do Monday. |
| "Just make the agent fully autonomous." | For these actions, yes. For irreversible/high-value ones, a human gate — here's the threshold and the cost of removing it. |
| "Our competitor's AI does X." | Here's what X would require in our data and constraints, and what it would cost. Then a recommendation. |
| "Why not just use the cheapest model?" | For 80% of traffic we do — here's the router. The expensive path exists because these cases fail measurably on the cheap one; here's the eval that shows it. |

**Never** promise deterministic behaviour, hide the failure rate, or quote a cost without saying what drives it.

### Lifecycle — the stages and their exit criteria

| Stage | Goal | Don't leave until |
| ----- | ---- | ----------------- |
| **Discovery** | Understand outcome, constraints, data, approvers | Success metric and baseline are written down |
| **Design** | Architecture, patterns, controls, cost model | Security/compliance have seen it; cost is estimated per outcome |
| **PoC** | Is it feasible at all? | Feasibility answered — **PoCs are meant to be thrown away** |
| **Pilot** | Does it work on real traffic, at limited scope? | Evals pass, real users have used it, cost per outcome is measured |
| **Production** | Scale safely | Monitoring, alerting, rollback, on-call owner, runbook all exist |
| **Iterate** | Improve on evidence | Feedback loop running: production failures → eval set → fix → re-deploy |

**The PoC-to-production trap is a favourite scenario:** a working demo is not a production system. What's missing is evals, monitoring, guardrails, error handling, cost controls, security review, and an owner. Say so, then offer a staged path — canary a slice, watch the metrics, widen — rather than a flat no.

### Operating it over time

- **Named owner** per system. Unowned agents rot.
- **Change management** — prompts, tools, and models are versioned artifacts; changes go through the regression suite ([Domain 4](04-evaluation-testing-optimization.md)) and roll out behind a flag.
- **Model deprecation** is scheduled, not surprising: track the calendar, test the successor against your evals, migrate deliberately, keep a rollback.
- **Cost review cadence** — usage grows; re-check routing, caching, and batch eligibility periodically.
- **Human handover** — the people whose work changed need training, an override path, and a way to report bad output that reaches the eval set.
- **Decommissioning** — how the system is retired, and what happens to its data.

## You must be able to

- [ ] Run **structured discovery** — name the seven questions and why each one matters.
- [ ] Pitch the same architecture to an exec, a CFO, a CISO, and an engineer.
- [ ] **Defend a decision**: requirement → alternatives → trade-off → what would change your mind.
- [ ] Handle the accuracy, cost, and ship-it-Monday conversations without over-promising.
- [ ] Name the lifecycle stages and the **exit criteria** for each.
- [ ] List what a PoC is missing before it can be production.
- [ ] Plan a model migration and a rollback.
- [ ] Design the human handover: training, override, feedback into evals.

## Resources

- Anthropic Partner Academy — the Professional prep path is heavily oriented to this domain; it's the best available source.
- Architecture decision records (ADR) — any standard template; the discipline is the point.
- Companion note: [Durable Execution](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/durable-execution.md) (operating long-lived systems).

## 🔎 Self-check

1. A VP says "build us an AI agent for claims." What do you ask before designing anything?
2. Explain an orchestrator–workers design to a CFO in three sentences.
3. The pilot hit 91% accuracy. The sponsor wants 100%. How do you respond?
4. What's in a PoC that must *not* go to production, and what's missing that must be added?
5. Name the lifecycle stages and one exit criterion for each.
6. Security asks: "what happens if the model is tricked into calling the delete tool?" Answer it.
7. Costs tripled in three months with flat volume. What do you investigate, and what do you tell finance?
8. Your pinned model is being deprecated in 90 days. Write the plan.
