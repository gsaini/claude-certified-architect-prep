# Domain 4 — Evaluation, Testing & Optimisation

> **Exam weight: 16%** · *"Measuring quality, testing rigorously, tuning for cost and performance."*
> This is the domain most under-served by "I've built agents" experience. Plenty of engineers ship LLM systems with no evals; the exam assumes you don't.

## Why it matters

Non-deterministic systems can't be verified by "it worked when I tried it." The architect's job is to define **what good means numerically**, build the dataset that measures it, and give the team a way to tell an improvement from a regression. Every optimisation question downstream — cheaper model? lower effort? smaller k? — is unanswerable without this.

## Task statements

- Define **evaluation metrics** across accuracy, latency, cost, and safety
- **Design evaluation datasets**
- **Diagnose system failures** and attribute them to a component
- **Production monitoring** and continuous evaluation
- Optimise for cost and performance without regressing quality

## Key concepts

### The four metric families — always answer on all four

| Family | Examples | Watch out for |
| ------ | -------- | ------------- |
| **Accuracy / quality** | Exact match, F1, rubric score, task success, groundedness, citation validity | "Accuracy" is meaningless until you say *of what* — per step, or end-to-end? |
| **Latency** | p50 / **p95** / p99, time-to-first-token | Averages hide the tail; SLAs live at p95/p99 |
| **Cost** | $ per request, per resolved ticket, per document | Cost-per-*outcome* beats cost-per-call — a cheap model that fails twice isn't cheap |
| **Safety / reliability** | Refusal correctness, escalation rate, injection resistance, hallucination rate, error rate | Also track **over-escalation** — a 100% escalation agent is "safe" and worthless |

**The exam-grade answer names a business metric too:** deflection rate, hours saved, cost per claim. Ties back to [Solution Design](01-solution-design-architecture.md)'s value pillars.

### Building the eval set

- **Start small and real.** ~20 examples drawn from actual traffic beat 500 synthetic ones. Grow toward a few hundred as failure modes surface.
- **Stratify deliberately:** happy path, known edge cases, adversarial inputs, and the failures you've *actually seen in production* — every incident becomes a test case.
- **Hold out a test set** you don't tune against, or you're just overfitting your prompt.
- **Version it.** An eval set that drifts silently makes every historical comparison meaningless.
- **Label the hard way once.** Human labels on the ambiguous slice are the ground truth everything else calibrates against.

### Grading methods — pick the cheapest that works

| Method | Use when | Limits |
| ------ | -------- | ------ |
| **Exact / fuzzy match** | Classification, extraction, structured output | Useless for open-ended text |
| **Programmatic checks** | Schema validity, citation exists, no PII, latency budget met | Only catches what you can codify |
| **LLM-as-judge** | Open-ended quality, tone, groundedness, helpfulness | Needs its own rubric and its own validation — **judges have bias and drift** |
| **Human review** | The ambiguous slice, and calibrating the judge | Slow and expensive — sample, don't exhaust |

**Make the judge respectable:** give it a concrete rubric with a defined scale, not "rate this 1–10"; use a **stronger** model as judge than as generator; validate judge scores against human labels on a sample; run it at low temperature/effort variance and watch for position and verbosity bias.

### Testing an agentic system

- **Test components separately from the whole.** A bad end-to-end answer might be retrieval, tool selection, or the final synthesis — with only an end-to-end score you can't tell.
- **Trajectory vs. outcome:** did it get the right answer (outcome), and did it take a sane path (trajectory — tool choices, step count, cost)? Both matter; a right answer after 40 tool calls is still a defect.
- **Regression suite in CI.** Every prompt, model, tool, or retrieval change reruns it. This is what makes a prompt change safe to ship.
- **Adversarial tests as first-class:** prompt injection in retrieved documents and tool results, jailbreaks, malformed and hostile inputs, out-of-scope requests.
- **Offline vs. online:** offline evals gate deploys; online (A/B, canary, shadow) measures reality. Ship behind a flag, canary a slice, watch the metrics, then widen.
- **Determinism is not available** — evaluate distributions. Run N times and look at the spread, don't reason from one sample.

### Diagnosing failures

Attribute before you fix. Walk the pipeline and ask **where the information was lost**:

| Symptom | Usual culprits |
| ------- | -------------- |
| Right documents retrieved, wrong answer | Prompt doesn't instruct grounding; context ordering; the model ignored the evidence |
| Wrong documents retrieved | Chunking, index type, query rewriting, or a missing metadata filter |
| Wrong tool chosen | Tool description, capability bloat, overlapping tools |
| Malformed output | Not using schema enforcement; or `stop_reason: max_tokens` (truncation, not a prompt bug) |
| Agent loops | No stopping condition, no step budget, no progress check |
| Fine in dev, broken in prod | Real data distribution differs from your eval set — a **dataset** bug |

**You need traces to do any of this.** Log prompt version, model, effort, tool calls with arguments and results, retrieval hits, tokens, cache hits, latency per hop, and `stop_reason`. Without them, diagnosis is guessing. See [Integration → Observability](03-integration.md).

### Optimisation — in this order

1. **Measure first.** Optimising without an eval baseline is how quality regressions ship silently.
2. **Route by difficulty** — cheap model on the easy path, expensive model only where it's needed.
3. **Prompt caching** on the stable prefix (≈0.1× read cost) — see [Domain 2](02-models-prompting-context-engineering.md).
4. **Batch API** (~50% off) for anything not user-facing.
5. **Shrink the loop** — fewer tools, tighter context, fewer turns, capped output.
6. **Lower `effort`** before downgrading the model; it's a finer-grained dial.
7. **Re-run the evals.** An optimisation that isn't quality-checked is a regression you haven't found yet.

**Always state the trade-off explicitly:** every accuracy lever (reranking, multi-hop retrieval, extended thinking, self-consistency, bigger model) buys quality with latency or cost. The exam wants you to say *which* you're spending and *why the SLA allows it*.

### Continuous evaluation in production

Offline evals go stale. In production: sample live traffic into the eval set, capture thumbs-down and human corrections as labelled data, monitor for **drift** (input distribution, cost per task, escalation rate, retrieval hit rate), and alert on *behaviour* changes rather than only errors. That feedback loop is part of the architecture — not an afterthought.

## You must be able to

- [ ] Define metrics across **accuracy, latency, cost, and safety** for a given scenario — plus one business metric.
- [ ] Design an eval dataset: size, stratification, holdout, versioning, and where the labels come from.
- [ ] Choose a **grading method** per metric and defend an LLM-judge design.
- [ ] Test components *and* end-to-end, trajectory *and* outcome.
- [ ] Design a **regression suite** that gates prompt/model changes in CI.
- [ ] Diagnose a failure to the right component from a trace.
- [ ] Sequence cost/latency optimisations and state each trade-off.
- [ ] Explain **offline vs. online** evaluation and when you'd canary.

## Resources

- Anthropic docs — evaluation guidance, the Console **Evaluate** tool, *Reduce latency*, *Batch processing*, *Prompt caching*.
- Companion note: [Building an Agent Evaluator](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/building-agent-evaluators.md)

## 🔎 Self-check

1. Define four metric families for a customer-support agent, with one concrete metric each.
2. You have 20 labelled examples and no budget for more. Is that enough? How do you grow the set?
3. When is LLM-as-judge appropriate, and how do you keep the judge honest?
4. End-to-end accuracy dropped 8% after a release. Walk your diagnosis.
5. What's the difference between trajectory and outcome evaluation — and when does trajectory matter more?
6. Name five cost levers in the order you'd pull them, with the trade-off of each.
7. What do you log on every request so a bad answer is reproducible next week?
8. Your evals pass but production quality is worse. What's the most likely cause?
