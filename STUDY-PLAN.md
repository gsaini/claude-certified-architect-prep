# 📅 The 6-Week Daily Study Plan

> **Target: Claude Certified Architect – Professional (CCAR-P)** · 7 domains · 63 questions · 120 min · 720/1000 to pass.
> **Pace:** ~1 hr per session, **3 sessions per week**. **42 sessions total.**
> Each day has **one focused task**. After it, write a short entry in [daily-log/](daily-log/) (copy [TEMPLATE.md](daily-log/TEMPLATE.md)).
> Domain time is allocated **by exam weight** — Integration (19%) gets the most days, Developer Productivity (7%) the fewest.

**Legend:** 📖 read/study · 🛠️ hands-on · 🔁 review · 🎯 design drill · 🧪 mock exam · ☕ buffer/rest

## 📆 My schedule — 3 sessions per week

> **A "Day" below is a *session*, not a calendar day.** The numbering is kept so logs and links stay stable.
> Do **3 sessions a week**, on whichever days suit you. Missing a Tuesday breaks nothing; missing a *week* is what slips the date.

**Days 1–8 done** (Thu 06 Aug – Wed 16 Sep, with two pauses). **Resumed Wed 2026-09-30 at Day 9** at 3 sessions/week → exam **Fri 2026-12-18**. Not booked yet — see [exam-logistics.md](resources/exam-logistics.md).

| Week | Dates | Sessions | Focus |
| :--: | ----- | -------- | ----- |
| **1** | Wed 30 Sep – Tue 06 Oct | Days 9, 10, 11 | Integration: MCP fundamentals · tool design · RAG chunking |
| **2** | Wed 07 – Tue 13 Oct | Days 12, 13, 14 | Integration: retrieval & SLAs · auth + observability · 🛠️ build an MCP server |
| **3** | Wed 14 – Tue 20 Oct | Days 15, 16, 17 | Models, prompting & context engineering |
| **4** | Wed 21 – Tue 27 Oct | Days 18, 19, 20 | 🛠️ Extraction pipeline · eval metrics & datasets · grading methods |
| **5** | Wed 28 Oct – Tue 03 Nov | Days 21, 22, 23 | Testing agents · diagnosis & optimisation · 🛠️ eval harness |
| **6** | Wed 04 – Tue 10 Nov | Days 24, 25, 26 | Governance: control layers · prompt injection · compliance |
| **7** | Wed 11 – Tue 17 Nov | Days 27, 28, 29 | HITL & ethics · ☕ buffer · structured discovery |
| **8** | Wed 18 – Tue 24 Nov | Days 30, 31, 32 | Stakeholder: communicating · the hard conversations · lifecycle |
| **9** | Wed 25 Nov – Tue 01 Dec | Days 33, 34, 35 | Claude Code for teams · 🛠️ configure a repo · ☕ buffer |
| **10** | Wed 02 – Tue 08 Dec | Days 36, 37, 38 | 🧪 Mock #1 · review weakest · 🧪 Mock #2 |
| **11** | Wed 09 – Tue 15 Dec | Days 39, 40, 41 | Review 2nd weakest + retake self-assessment · 🧪 Mock #3 · final review |
| **12** | **Fri 18 Dec** | **Day 42** | 🏛️ **Exam** |

---

## Week 1 — Solution Design & Architecture ([D1](domains/01-solution-design-architecture.md), 17%)

| Day | Focus | Task |
| --- | ----- | ---- |
| **Day 1** ✅ | 📖 Orientation & logistics | Read [README](README.md) + this plan. ~~Confirm whether Foundations is a prerequisite~~ → **it is not**. Pick a target exam date → ~~Wed 2026-09-16~~ **Tue 2026-10-20** (rescheduled 2026-09-13 after a pause). Take the [self-assessment](practice/self-assessment.md). → [log](daily-log/2026-08-06.md) |
| **Day 2** ✅ | 📖 Business problem → architecture | Translating a business problem into a Claude solution. The **value pillars** (efficiency, transformation, productivity, cost, performance/SLAs). The five discovery questions you ask *before* designing. → [log](daily-log/2026-08-07.md) |
| **Day 3** ✅ | 📖 The three pattern families | **Augmented LLM · workflow · agentic.** When each fits, what choosing wrong costs, and how they compose. "Use the simplest thing that works." → [log](daily-log/2026-08-08.md) |
| **Day 4** ✅ | 📖 End-to-end architecture | Input → processing → output → **feedback loop**. Where logging, evals, and human corrections attach. Designing for the failure path, not just the happy one. → [log](daily-log/2026-08-08-day-04.md) |
| **Day 5** ✅ | 📖 Orchestration patterns | Chaining, routing, parallelization, orchestrator–workers, evaluator–optimizer. Know *which pattern for which problem* — especially parallelization vs. orchestrator–workers. → [log](daily-log/2026-09-13.md) |
| **Day 6** ✅ | 🎯 Multi-agent + decomposition drill | When multi-agent wins and its three real costs. Then: decompose a real business problem end-to-end on paper, with the pattern, the guardrails, and the feedback loop. → [log](daily-log/2026-09-14.md) |
| **Day 7** | ☕ Buffer / 🔁 review | Catch up, or answer the [D1 self-check](domains/01-solution-design-architecture.md#-self-check) from memory. |

## Week 2 — Integration ([D3](domains/03-integration.md), 19% — the largest domain)

| Day | Focus | Task |
| --- | ----- | ---- |
| **Day 8** ✅ | 📖 Choosing the integration mechanism | **MCP vs. direct API/SDK vs. CLI vs. agent-to-agent.** The N×M → N+M argument, and when MCP is *not* worth the process boundary. → [log](daily-log/2026-09-16.md) |
| **Day 9** ✅ | 📖 MCP fundamentals | Host / client / server. The three primitives and **who controls each**. stdio vs. Streamable HTTP. Cross-ref: [MCP note](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/model-context-protocol.md). → [log](daily-log/2026-09-30.md) |
| **Day 10** ✅ | 📖 Tool design & capability bloat | Descriptions Claude won't misuse, enums and defaults, return shapes, error semantics. **Progressive discovery vs. monolithic context.** → [log](daily-log/2026-10-10.md) |
| **Day 11** | 📖 RAG: chunking & indexing | Fixed / semantic / structural chunking. Dense vs. sparse vs. **hybrid**, metadata filters. Matching strategy to *data shape*. |
| **Day 12** | 📖 Retrieval & accuracy–latency | Top-k, reranking, query rewriting, multi-hop. Matching strategy to *query pattern*. Justifying a configuration against an SLA. |
| **Day 13** | 📖 Auth/authz & observability | The service-account authorization trap, on-behalf-of identity, least privilege, multi-tenancy. Then: what to trace, what to sample, what to keep in full. |
| **Day 14** | ☕ Buffer / 🛠️ build a tiny MCP server | Build a stdio MCP server exposing **one** tool over a real API. Feel the schema + JSON-RPC loop end to end. |

## Week 3 — Models, Prompting & Context ([D2](domains/02-models-prompting-context-engineering.md), 13%) + Evaluation ([D4](domains/04-evaluation-testing-optimization.md), 16%)

| Day | Focus | Task |
| --- | ----- | ---- |
| **Day 15** | 📖 Model selection & routing | Opus / Sonnet / Haiku on cost, latency, capability. `effort` and adaptive thinking as finer dials. **Design the router, don't pick one model.** |
| **Day 16** | 📖 Prompting & structured output | Techniques and what each *costs*. Schema-enforced output, validation, repair-retry, `stop_reason`. (Note: assistant prefill is gone.) |
| **Day 17** | 📖 Context engineering | The window as a budget. **Prompt caching and the prefix-match rule** — plus the silent invalidators. Compaction, context editing, memory, retrieval, subagent offload. |
| **Day 18** | 🛠️ Structured extraction pipeline | Build one: schema → validation → retry on schema-fail. Add prompt caching and measure the hit rate. |
| **Day 19** | 📖 Metrics & eval datasets | Accuracy / latency / cost / safety — plus a business metric. Dataset size, stratification, holdout, versioning. |
| **Day 20** | 📖 Grading methods | Exact match, programmatic checks, **LLM-as-judge** (and how to keep the judge honest), human review. |
| **Day 21** | 📖 Testing agentic systems | Component vs. end-to-end, **trajectory vs. outcome**, regression suites in CI, adversarial tests, offline vs. online. |

## Week 4 — Evaluation finish + Governance, Safety & Risk ([D5](domains/05-governance-safety-risk.md), 14%)

| Day | Focus | Task |
| --- | ----- | ---- |
| **Day 22** | 📖 Diagnosis & optimisation | Attributing a failure to the right component from a trace. Then the optimisation ladder — and stating the trade-off every time. |
| **Day 23** | 🛠️ Build an eval harness | Twenty real examples, a grader, a score you can re-run. Break something on purpose and watch it catch it. |
| **Day 24** | 📖 Control layers & guardrails | Input / model / tool / output / human / audit. **Why a prompt instruction is never the control for a consequential action.** |
| **Day 25** | 📖 Prompt injection & agentic risk | Untrusted tool results, excessive agency, data leakage, over-automation, runaway cost. Cross-ref: OWASP Top 10 for LLM Apps. |
| **Day 26** | 📖 Compliance & data governance | Minimisation, residency, retention, tenancy isolation, auditability, on-behalf-of access. GDPR/sector obligations at the shape level. |
| **Day 27** | 📖 HITL, incident response & ethics | Where the approval gate sits and what the human must see. Kill switch, rollback, disclosure. Fairness, contestability, appropriate use. |
| **Day 28** | ☕ Buffer / 🔁 review | Catch up, or run the [D4](domains/04-evaluation-testing-optimization.md#-self-check) + [D5](domains/05-governance-safety-risk.md#-self-check) self-checks. |

## Week 5 — Stakeholder & Lifecycle ([D6](domains/06-stakeholder-communication-lifecycle.md), 14%) + Developer Productivity ([D7](domains/07-developer-productivity-operational-enablement.md), 7%)

| Day | Focus | Task |
| --- | ----- | ---- |
| **Day 29** | 📖 Structured discovery | The seven questions before any architecture. Finding the approvers early. The anti-patterns that lose points. |
| **Day 30** | 📖 Communicating & defending decisions | The same design pitched to an exec, a CFO, a CISO, an engineer. Decision → alternatives → trade-off → **what would change my mind**. |
| **Day 31** | 🎯 The hard conversations | Drill them out loud: "can it be 100% accurate?", "why is it 3× the estimate?", "the demo worked, ship it Monday", "just make it fully autonomous." |
| **Day 32** | 📖 Lifecycle management | Discovery → design → PoC → pilot → production → iterate, with **exit criteria**. The PoC-to-production trap. Model deprecation and migration planning. |
| **Day 33** | 📖 Claude Code for teams | `CLAUDE.md` hierarchy; command vs. skill vs. subagent vs. **hook**; permission allowlists; MCP scope. |
| **Day 34** | 🛠️ Configure a real repo + CI/CD | Set up one of your own repos properly. Then design a safe non-interactive CI review step. |
| **Day 35** | ☕ Buffer / 🎯 drills | Catch up, or run design drills on your two weakest domains. |

## Week 6 — Mock Exams & Final Review

| Day | Focus | Task |
| --- | ----- | ---- |
| **Day 36** | 🧪 Mock exam #1 | **63 questions, 120 minutes, closed-book.** Grade by domain; log what you missed. |
| **Day 37** | 🔁 Target weakest domain | Deep-review your #1 gap from mock #1; redo its self-check. |
| **Day 38** | 🧪 Mock exam #2 | Timed. Compare to #1; note what didn't move. |
| **Day 39** | 🔁 2nd weakest + re-diagnose | Review your #2 gap. **Retake the [Day 1 self-assessment](practice/self-assessment.md)** closed-book and compare. |
| **Day 40** | 🧪 Mock exam #3 + drills | Timed. Then talk through a design drill per domain, out loud. |
| **Day 41** | 🔁 Final review + logistics | One-page cheat-sheet of all 7 domains. **Confirm:** Pearson VUE booking, ID, quiet room, system check, closed-book rules. |
| **Day 42** | 🏛️ Exam day | Light review only. Trust the prep. Pass. ✅ |

---

## 📈 Pacing notes

- **Missed a session?** Just do it at the next one — the schedule counts *sessions per week*, not fixed weekdays.
- **Missed a whole week?** Slide the week table down by one week and move the exam date with it. That's the only thing that actually slips the date.
- **Behind schedule?** Use the ☕ buffer sessions (14, 28, 35) — don't skip the hands-on ones.
- **Already strong on a domain?** Convert that day into extra design drills or an additional mock.
- **Target ≥ 80% on mocks** before exam day; under that, push the date and repeat Week 6.
- **The exam is closed-book and scenario-based** — prioritise *judgment* ("which pattern/control/model fits this situation, and why?") over memorising syntax.
- **Weight your effort:** Integration + Solution Design + Evaluation are **52%** of the exam. Developer Productivity is 7% — don't over-study it.

## 🔁 The daily loop (every study day)

1. Open today's row above → work through it using the linked [domain guide](domains/).
2. Copy [daily-log/TEMPLATE.md](daily-log/TEMPLATE.md) → `daily-log/YYYY-MM-DD.md`, fill it in (what you learned, confidence 1–5, questions).
3. Tick any completed item in the [README progress tracker](README.md#-progress-tracker).
4. Commit: `git commit -m "Day N: <topic>"`.
