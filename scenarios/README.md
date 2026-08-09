# 🎯 Design Drills

> ⚠️ **Format note (verified 2026-08-07):** the "4 scenarios drawn from a bank of 6" structure belongs to the **Foundations** exam. **CCAR-P is ~63 standalone items** — scenario-flavoured stems, but not a fixed scenario bank. So these are **practice drills, not an exam blueprint.** Their value is rehearsing the reasoning, not memorising six situations.

The exam rewards **judgment in context**: *given this situation, which architecture / integration / control / model fits — and why?* You can't drill that by reading. Drill it by designing.

## How to run a drill

1. Set a **10-minute timer**. No notes.
2. Write the design: components, data flow, patterns, integration mechanism, controls, escalation, feedback loop.
3. Then answer the three questions that follow every drill: **what does it cost, how do you know it works, and what would change your mind?**
4. Check yourself against the watch-outs, then log the gaps in [daily-log/](../daily-log/).

> **The habit that scores points:** every design answer should name a **trade-off** and a **measurement**. A design with no stated cost and no way to tell if it's working is incomplete, however elegant the architecture.

## The drills

| # | Drill | Primary domains |
| - | ----- | --------------- |
| 1 | Customer support resolution agent | 1 Solution Design · 5 Governance |
| 2 | Enterprise knowledge assistant (RAG) | 3 Integration · 4 Evaluation |
| 3 | Multi-agent research system | 1 Solution Design |
| 4 | High-volume document extraction | 2 Models/Prompting · 4 Evaluation |
| 5 | Regulated-industry deployment | 5 Governance · 6 Stakeholder |
| 6 | Developer productivity platform | 7 Dev Productivity · 3 Integration |
| 7 | The stakeholder conversation | 6 Stakeholder & Lifecycle |

---

## 1. Customer support resolution agent

**Goal:** resolve issues autonomously *while knowing when to escalate.*

- **Tests:** workflow-vs-agent choice, tool design, the **escalation boundary**, guardrails, long-conversation context.
- **Design it:** which tools, with which scopes? What triggers escalation (confidence, stakes, repeated failure)? Where's the human gate, and what does the human see? How do you keep 100 turns in budget?
- **Watch-outs:** over-automation (acting where it should escalate); no stopping condition; unbounded context; the refund cap living in the system prompt instead of in code.

## 2. Enterprise knowledge assistant (RAG)

**Goal:** answer questions over internal documents that different employees have different rights to see.

- **Tests:** chunking and indexing choices, retrieval matched to data shape *and* query pattern, **the authorization trap**, groundedness measurement.
- **Design it:** how are documents chunked, and why that way? Hybrid or dense? Does the agent inherit the *caller's* entitlements or a service account's? How do you prove answers are grounded?
- **Watch-outs:** union-of-all-permissions retrieval; chunk boundaries splitting the answer; no citations; stale index; retrieved content treated as instructions (injection).

## 3. Multi-agent research system

**Goal:** orchestrate specialised subagents for search, analysis, and synthesis.

- **Tests:** orchestrator–workers vs. parallelization, decomposition, when multi-agent is justified, cost control.
- **Design it:** lead decomposes → subagents with their own tools → explicit **merge strategy**. What's the token budget? What's the stopping condition?
- **Watch-outs:** multi-agent where a single agent would do; no merge strategy; runaway cost; no per-subagent guardrails.

## 4. High-volume document extraction

**Goal:** turn 50,000 messy documents a night into validated structured records.

- **Tests:** model routing, schema-enforced output, validation + repair, **Batch API**, cost per document.
- **Design it:** which model for which document class? Schema enforcement how? What happens on a validation failure? Real-time or batch, and why? What's the unit cost?
- **Watch-outs:** "please output JSON" instead of a schema; no repair path; missing the batch discount; ignoring `stop_reason: max_tokens` as a truncation cause.

## 5. Regulated-industry deployment

**Goal:** ship a Claude system where an auditor will eventually ask what happened and why.

- **Tests:** control layers, tenancy isolation, audit trail, retention/residency, human oversight, incident response.
- **Design it:** what's logged, what's redacted, kept how long? Where's the kill switch? Who owns the system? What's the rollback?
- **Watch-outs:** PII in traces; shared cache or index across tenants; secrets in prompts; no named owner; no way to stop it.

## 6. Developer productivity platform

**Goal:** roll Claude Code out to a 30-engineer org, plus MCP servers for codebase navigation.

- **Tests:** `CLAUDE.md` hierarchy, hooks vs. prompts, permission allowlists, MCP scope and transport, safe CI/CD.
- **Design it:** what's committed vs. personal? Which behaviours must be hooks? What's allowlisted? How does the CI step stay safe with no human watching?
- **Watch-outs:** blanket `Bash(*)`; bloated `CLAUDE.md`; secrets committed; an automated agent with write access and no review gate.

## 7. The stakeholder conversation

**Goal:** defend a design you already built. *(No diagram — say it out loud.)*

- **Tests:** discovery, audience translation, decision defence, expectation setting.
- **Drill it:** pitch drill #2 to a CFO in three sentences. Then answer, out loud: *"Can it be 100% accurate?"* · *"Why is it 3× the estimate?"* · *"The demo worked — ship it Monday."* · *"Just make it fully autonomous."*
- **Watch-outs:** promising determinism; quoting cost without saying what drives it; a flat "no" instead of a staged path; jargon that doesn't survive translation.
