# 🩺 Day 1 Self-Assessment — 10 Diagnostic Questions

> **Purpose:** find your weak spots *before* you spend 41 days studying. This is a **diagnostic, not a mock exam** — it is deliberately short-answer, because the real exam tests *judgment* ("which pattern fits here, and why?"), and multiple choice lets you recognise an answer you couldn't have produced.
>
> **How to take it (~25 min):**
>
> 1. Closed-book. No Claude, no docs, no tabs. Set a **20-minute timer** for all 10.
> 2. Write 2–4 sentences per question in your own words (paper or a scratch file — don't type into this file yet).
> 3. Only then open each **Model answer** and self-score **1–5** using the rubric.
> 4. Fill in the [scorecard](#-scorecard) at the bottom → that ranking sets your study priorities.
>
> **Be honest.** A 3 you talked yourself into is worse than a 2 you admitted. The whole value here is finding the gaps.

**Self-score rubric:** `1` no idea · `2` vague gist, couldn't defend it · `3` got the core, missed the *why* or the trade-off · `4` solid, minor gaps · `5` could teach it, including when *not* to use it.

> **Coverage note:** this diagnostic was written on Day 1, before the repo moved to the CCAR-P blueprint. It maps onto **five of the seven** Professional domains. It does **not** probe Governance/Safety/Risk (14%) or Stakeholder & Lifecycle (14%) — assume those are gaps until Weeks 4–5 prove otherwise.

| Q | Topic | CCAR-P domain (weight) |
| - | ----- | ---------------------- |
| 1–2 | Workflow vs. agent · orchestration patterns | **1** Solution Design & Architecture (17%) |
| 3–4 | Claude Code config · commands/skills/subagents | **7** Developer Productivity & Ops Enablement (7%) |
| 5–6 | Structured output · prompt techniques | **2** Models, Prompting & Context Engineering (13%) |
| 7–8 | Tool descriptions · MCP architecture | **3** Integration (19%) |
| 9–10 | Context strategies · escalation & cost | **2** + **5** Governance, Safety & Risk (14%) |
| — | *not covered* | **4** Evaluation (16%) · **6** Stakeholder & Lifecycle (14%) |

---

## Solution Design & Architecture — CCAR-P D1 (17%)

### Q1. Workflow or agent?

> A finance team processes ~2,000 invoices a night. Every invoice goes through the same four steps: extract fields → validate against the PO → flag mismatches → write to the ledger. They want to add Claude. Do you build an **agent** or a **workflow**, which orchestration pattern, and what would change your mind?

<details><summary>Model answer</summary>

**Workflow — prompt chaining.** The steps are fixed, ordered, and known in advance; nothing needs runtime decision-making. A workflow is cheaper, lower-latency, debuggable step-by-step, and each step can be independently evaluated and retried. Reaching for an agent here buys nothing and costs tokens, latency, and predictability. Add a **gate after validation** (programmatic check, not a model call) so bad extractions don't reach the ledger.

**What would change my mind:** if the "flag mismatches" step required open-ended investigation — chasing a mismatch across contracts, emails, and prior invoices where the number and order of lookups isn't knowable up front. *That* sub-step justifies an agent, embedded inside the workflow. The rule is: **use the simplest thing that works**, and escalate only the sub-step that genuinely needs dynamic control.

*Score 5 if you named the pattern, justified it on predictability/cost/debuggability, AND gave a concrete condition that would flip you to an agent.*
</details>

**My score:** ⬜

### Q2. Orchestrator–workers vs. parallelization

> When do you choose **orchestrator–workers** over **parallelization (sectioning)**? Name three real costs of going multi-agent.

<details><summary>Model answer</summary>

**The dividing line is whether you know the subtasks up front.** Parallelization/sectioning is for a **fixed, known** set of independent subtasks you can fan out statically (e.g. "run these 5 checks on this document"). Orchestrator–workers is for when the **number and shape of subtasks is decided at runtime** by a lead model that reads the task and decomposes it (e.g. "research this topic" — how many sources, and which, is unknowable in advance).

**Three costs of multi-agent:** (1) **token cost** — often several times a single agent, since context is duplicated into every subagent; (2) **coordination overhead** — decomposition, hand-off, and result-merging are extra failure surfaces, and subagents can duplicate or contradict each other; (3) **debuggability/observability** — non-deterministic, interleaved traces are far harder to reproduce and evaluate; also (4) latency variance and (5) harder guardrail enforcement across N loops.

*Score 5 if you named "subtask shape known up front?" as the criterion and gave three distinct, real costs.*
</details>

**My score:** ⬜

---

## Developer Productivity & Ops Enablement — CCAR-P D7 (7%)

### Q3. Which config lives where?

> A 30-engineer team wants consistent Claude Code behaviour. For each of these, say whether it belongs in **project `CLAUDE.md`**, **user `CLAUDE.md`**, **`.claude/settings.json`** (checked in), or **`settings.local.json`** (not checked in) — and why:
> (a) "run `pnpm test`, never `npm test`" · (b) "I prefer terse answers" · (c) allowlisting `gh pr view` so it stops prompting · (d) a personal API token for a local MCP server · (e) "always run the linter after editing a file, automatically"

<details><summary>Model answer</summary>

(a) **Project `CLAUDE.md`** — a repo fact every teammate needs. (b) **User `CLAUDE.md`** (`~/.claude/`) — a personal preference; putting it in the repo imposes your taste on 29 other people. (c) **`.claude/settings.json`, checked in** — a permission allowlist the whole team benefits from; scope it narrowly (`Bash(gh pr view:*)`), never a blanket `Bash(*)`. (d) **`settings.local.json`** (git-ignored) — secrets never get committed; better still, reference an env var. (e) **A hook** (`PostToolUse` on Edit/Write) in `settings.json` — *automated, deterministic* behaviour is the harness's job, not a memory instruction; anything phrased "always / every time / after X" is a hook, because `CLAUDE.md` requests are advisory and the model may skip them.

**Precedence to know:** enterprise → project → user, with more-specific/deeper directories winning. **The trap:** a bloated `CLAUDE.md` — it's loaded into *every* request, so it costs tokens continuously and dilutes attention. Keep it short and high-signal.

*Score 5 if you got (e) as a hook and articulated why (deterministic enforcement vs. advisory instruction).*
</details>

**My score:** ⬜

### Q4. Subagent, skill, or slash command?

> Explain the difference between a **custom slash command**, a **Skill**, and a **subagent** in Claude Code — and give one scenario where each is clearly the right choice.

<details><summary>Model answer</summary>

- **Slash command** — a stored prompt the *user* fires explicitly (`/deploy-check`). Runs in the **current context**. Right when a human wants to invoke a repeatable prompt on demand.
- **Skill** — packaged instructions (+ optional scripts/resources) that Claude loads **when the task matches its description**, progressively. Right for "whenever we touch the payments module, here's our review checklist and the invariants" — knowledge that should appear automatically but shouldn't sit in context all the time.
- **Subagent** — a *separate context window* with its own system prompt and tool allowlist, delegated a task; it returns a result, not its transcript. Right when the work would **flood the main context** (searching 200 files) or needs **isolation/parallelism** (three independent reviewers at once).

**The discriminator: context.** Command = same context, user-triggered. Skill = same context, model-triggered, loaded on demand. Subagent = **separate** context, returns only a summary. Choose a subagent for context preservation and parallelism; pay for it with a fresh agent that doesn't know your conversation.

*Score 5 if you framed the distinction around context isolation and trigger mechanism, not just "one is a file in a different folder".*
</details>

**My score:** ⬜

---

## Models, Prompting & Context Engineering — CCAR-P D2 (13%)

### Q5. Reliable JSON

> A pipeline asks Claude to "respond only with JSON matching this shape". ~4% of responses fail to parse — a stray preamble, a trailing comma, a truncated object. Give the *architectural* fix, not a prompt tweak.

<details><summary>Model answer</summary>

**Stop asking for JSON in prose — use the API's structured mechanisms.** Define the output as a **tool with a JSON Schema** (or use structured outputs), so the shape is enforced by the API rather than by the model's goodwill. Then, defensively: **(1) validate** every response against the schema in code (Pydantic/zod), **(2) on failure, retry once feeding back the validator's error** — a repair turn fixes most malformed output; **(3) prefill** the assistant turn with `{` if you truly must parse free text; **(4)** check `stop_reason` — a truncated object usually means you hit `max_tokens`, which no prompt change will fix; raise the limit or reduce the requested output; **(5)** log failures and treat the 4% as an eval signal, not noise.

*Score 5 if you led with tool-use/schema enforcement AND named validate-then-repair-retry AND spotted that truncation is a `max_tokens`/`stop_reason` problem, not a prompting one.*
</details>

**My score:** ⬜

### Q6. Prompt techniques

> Name five prompt techniques and, for each, one situation where it's the *wrong* tool.

<details><summary>Model answer</summary>

Any five of: **clear/direct instruction** (wrong when the task genuinely needs reasoning space and you've over-constrained it), **multishot examples** (wrong when examples are unrepresentative — the model overfits their format and edge-case handling; also costly in tokens), **chain-of-thought / extended thinking** (wrong for simple, latency-sensitive classification — you pay tokens and time for no accuracy gain), **XML tags to delimit sections** (wrong as decoration on a two-line prompt — clutter), **role/system prompt** (wrong when it becomes a personality instead of a task frame), **prefill** (wrong when you need the model to decide the format, and unavailable with extended thinking), **decomposing into chained prompts** (wrong when a single call suffices — adds latency and failure surface).

**The through-line:** every technique costs tokens, latency, or flexibility; the exam rewards knowing the trade-off, not reciting the list.

*Score 5 if each "wrong tool" case was concrete and about cost/latency/overfitting, not just "when you don't need it".*
</details>

**My score:** ⬜

---

## Integration — CCAR-P D3 (19%)

### Q7. Tool description review

> A team ships an MCP tool: `name: "search"`, `description: "Searches for stuff"`, params `{q: string, t?: string}`. Claude keeps calling it with the wrong arguments and sometimes doesn't call it at all. List every problem and rewrite it.

<details><summary>Model answer</summary>

**Problems:** the **name** is generic and collides with every other search tool in the context; the **description** doesn't say *what corpus* it searches, *when to use it*, or *when not to*; **`q`** is un-described (is it keywords? natural language? a query DSL?); **`t`** is an unreadable abbreviation with no enum, no default, no stated units; nothing describes the **return shape**, so Claude can't plan its next step; there are no **error semantics** (empty results vs. failure); no example call.

**Rewrite:** `search_support_tickets` — *"Search the Zendesk support-ticket archive by natural-language query. Use when the user asks about past customer issues, bug reports, or how a previous ticket was resolved. Do NOT use for live ticket status — use `get_ticket` for that. Returns up to 20 tickets with id, title, status, resolved_at, and a snippet."* Params: `query` (natural-language description of what to find), `status` (**enum**: `open | pending | solved`, optional, default: all). Return a **structured, token-efficient** result and a clear, actionable error string on failure (`"No tickets matched; try broader terms"` ≠ an exception).

**The principle:** the description is the *interface contract with the model*. Write it for a competent new engineer with no context — name says what, description says when and when-not, schema says exactly what's legal, errors tell the model what to do next. Also: **too many overlapping tools** is itself a bug (capability bloat) — it degrades selection accuracy.

*Score 5 if you covered naming, when-to-use **and when-not-to**, enums/defaults, return shape, and error semantics.*
</details>

**My score:** ⬜

### Q8. MCP architecture

> Explain MCP's host / client / server roles and its three primitives. When is a **direct API tool** the better choice than an MCP server? stdio or Streamable HTTP?

<details><summary>Model answer</summary>

**Roles:** the **host** is the application the user interacts with (Claude Desktop, Claude Code, your app); it spawns one **client** per server, and each client holds a 1:1 stateful session with one **server**, which exposes capabilities. **Primitives:** **tools** (model-controlled — actions Claude may invoke), **resources** (application-controlled — contextual data the host can attach, addressed by URI), **prompts** (user-controlled — templates the user picks, e.g. slash commands). *Knowing who controls each is the commonly-tested bit.*

**Direct tool beats MCP** when the integration is single-app, in-process, and won't be reused — MCP's payoff is the **N×M → N+M** decoupling of reusable, cross-host servers; for one bespoke function inside your own agent, an MCP server is a process boundary and a schema round-trip you don't need.

**Transports:** **stdio** for local, single-user, same-machine servers (subprocess, no network, no auth to configure — the default for local dev tooling). **Streamable HTTP** for remote/shared/multi-user servers, where you need network access, scaling, and **OAuth 2.x** authorization. Remote means you now own auth, tenancy, and the risk that tool *results* are untrusted input.

*Score 5 if you got the control-ownership of the three primitives and gave a real N×M rationale for MCP.*
</details>

**My score:** ⬜

---

## Context & Escalation — CCAR-P D2 + D5

### Q9. Long-conversation context

> A support agent handles chats that run 100+ turns and blow the context window. Give four distinct strategies, with the trade-off of each.

<details><summary>Model answer</summary>

(1) **Compaction / rolling summarization** — summarize older turns into a compact state and drop the raw transcript; *trade-off:* lossy, and details dropped early can't be recovered. (2) **External memory** — persist facts (customer id, entitlements, decisions taken) to a store/file and re-inject only what's relevant; *trade-off:* you must decide what's relevant, and stale memory misleads. (3) **Retrieval instead of stuffing** — keep KB articles and history out of context, fetch on demand; *trade-off:* retrieval quality becomes the ceiling, plus a latency hop. (4) **Subagent offloading** — do research/search in a separate context that returns only its conclusion; *trade-off:* the subagent lacks conversational context and costs an extra model call. Also: (5) **prompt caching** to make the stable prefix cheap, (6) **tool-result pruning** of superseded outputs, (7) **session hand-off** with a structured summary.

**The framing that scores:** context is a *budget you actively manage*, not a limit you hit. Every strategy trades fidelity for room.

*Score 5 if you gave four genuinely different mechanisms, each with a real cost.*
</details>

**My score:** ⬜

### Q10. Escalation & cost

> The same agent must know when to hand off to a human. Define the escalation boundary and the reliability controls. Separately: it runs on Opus and costs too much — name three levers.

<details><summary>Model answer</summary>

**Escalate on:** (a) **low confidence** or ambiguous intent after a clarifying attempt; (b) **high-stakes / irreversible** actions — refunds over a threshold, account deletion, anything legal, medical, or financial (human-in-the-loop **approval gate**, not post-hoc review); (c) **repeated failure** — N unsuccessful attempts or a detected loop; (d) **out-of-scope** requests, and (e) explicit user request for a human, or detected distress. **Controls:** bounded execution (max steps/tokens/time), an explicit **stopping condition**, idempotent + reversible tool design, retries with backoff on transient failure, graceful degradation to a safe default, and full logging/tracing for evaluation. **The failure mode to name:** over-automation — acting confidently where it should have escalated is worse than escalating too often.

**Three cost levers:** (1) **right-size the model** — route by difficulty; Haiku for classification/routing, Sonnet for most turns, Opus only for genuinely hard reasoning; (2) **prompt caching** on the stable system prompt / tool definitions / KB prefix — large savings on repeated long prefixes, and lower latency; (3) **shrink the loop** — fewer tools, tighter context, fewer turns, capped output, and batch the non-interactive work through the **Batch API** (~50% cheaper) where latency doesn't matter.

*Score 5 if escalation covered confidence, stakes, AND repeated failure, and your cost levers included model routing + caching.*
</details>

**My score:** ⬜

---

## 📊 Scorecard

Fill this in immediately after scoring — while you still remember which answers you *guessed*.

| CCAR-P domain | Weight | Q | Score (1–5) | Domain avg |
| ------------- | :----: | - | :---------: | :--------: |
| 3 Integration | 19% | Q7, Q8 | ⬜ ⬜ | ⬜ |
| 1 Solution Design & Architecture | 17% | Q1, Q2 | ⬜ ⬜ | ⬜ |
| 2 Models, Prompting & Context | 13% | Q5, Q6, Q9 | ⬜ ⬜ ⬜ | ⬜ |
| 5 Governance, Safety & Risk | 14% | Q10 | ⬜ | ⬜ |
| 7 Developer Productivity & Ops | 7% | Q3, Q4 | ⬜ ⬜ | ⬜ |
| *4 Evaluation (16%) · 6 Stakeholder (14%)* | *30%* | — | *not probed* | — |

**Weighted readiness** (over the 70% this covers) = Σ (domain avg × weight) ÷ 5 ÷ 0.70 × 100 = ⬜ %

### What the number means

| Weighted readiness | Read it as |
| ------------------ | ---------- |
| **< 40%** | Broad gaps. Follow the 6-week plan as written; don't skip buffer days. |
| **40–65%** | Typical strong-engineer starting point. Follow the plan, but **double** the days on your two lowest domains. |
| **65–85%** | You mostly need *exam framing*, not new concepts. Compress reading days, spend the time on scenarios and mocks. |
| **> 85%** | Consider booking sooner and going straight to Week 5–6 (scenarios + timed mocks). |

### Turn it into a plan change

1. **Rank the domains** by `weight × (5 − score)` — that product, not the raw score, is where your points are. A 3 in Integration (19%) costs more than a 2 in Dev Productivity (7%).
2. Write your top-2 gaps into the [Day 1 log](../daily-log/2026-08-06.md) and revisit them on the buffer days (7, 14, 21, 28, 35).
3. Any question you scored **≤ 2** → make it a flashcard today.
4. **Retake this exact assessment on Day 39.** Same questions, closed-book. Every score should be ≥ 4; anything still ≤ 3 is what you review on Day 41.
