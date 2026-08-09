# Domain 3 — Integration

> **Exam weight: 19% — the largest domain.** · *"Wiring Claude into enterprise systems and data."*
> This is where the Foundations-level MCP knowledge is necessary but **not sufficient**: Professional adds RAG, auth/authz, observability, and protocol selection.

## Why it matters

Anyone can call the API. The architect's job is connecting Claude to systems that have **owners, credentials, rate limits, tenancy, and audit requirements** — and choosing *how* to connect (MCP vs. API/CLI vs. agent-to-agent) with a defensible reason. Nearly a fifth of the exam lives here.

## Task statements

- Evaluate tool/agent configuration for **capability bloat**
- Analyse **authentication and authorization** requirements to identify security gaps
- Evaluate **accuracy–latency trade-offs** and justify configuration decisions
- Design a **RAG pipeline** with appropriate chunking and indexing strategies
- Apply **retrieval strategies matched to data shape and query pattern**
- Evaluate connection protocols and select the integration mechanism (**MCP · API/CLI · agent-to-agent**)
- Analyse observability challenges and select **monitoring strategies at scale**
- Evaluate **progressive discovery vs. monolithic context** strategy

## Key concepts

### Choosing the integration mechanism

| Mechanism | Use when | Don't use when |
| --------- | -------- | -------------- |
| **MCP server** | The capability is **reused across hosts/teams**, you want a standard schema contract, or you're extending an MCP-aware host (Claude Code, Claude Desktop) | It's one bespoke function inside your own app — you've added a process boundary for nothing |
| **Direct API / SDK tool** | Single application, in-process, tight latency budget, custom control over the loop | You're rebuilding the same connector for the 4th host |
| **CLI invocation** | The capability already exists as a battle-tested command; the agent runs in a shell context | You need structured, typed results and error semantics |
| **Agent-to-agent** | A capability is owned by *another team's* agent with its own model, tools, and lifecycle | You just need a function call — this is the heaviest coupling |

> **The MCP rationale to state out loud:** MCP turns an **N×M** integration problem (N hosts × M systems) into **N+M**. If you're not getting reuse across hosts, you're not getting MCP's payoff.

### MCP essentials

- **Roles:** the **host** is the user-facing app; it spawns one **client** per server; each client holds a **1:1 stateful session** with a **server** that exposes capabilities.
- **The three primitives — know who controls each:**
  | Primitive | Controlled by | Example |
  | --------- | ------------- | ------- |
  | **Tools** | **Model** — Claude decides to invoke | `search_tickets`, `create_pr` |
  | **Resources** | **Application/host** — attached as context, addressed by URI | a file, a schema, a dashboard snapshot |
  | **Prompts** | **User** — explicitly selected | a slash command / template |
- **Transports:** **stdio** for local, single-user, same-machine (subprocess; no network; nothing to authenticate). **Streamable HTTP** for remote/shared/multi-user — now you own auth (**OAuth 2.x**), tenancy, scaling, and network failure modes.
- **Security posture:** **tool results are untrusted input.** A returned document can contain prompt injection. Never let a tool result silently authorize a privileged action.

### Tool design & capability bloat

A tool description is **the interface contract with the model**. Write it for a competent engineer with zero context:

- **Name** — specific, non-colliding (`search_support_tickets`, not `search`).
- **Description** — what it does, **when to use it**, and explicitly **when *not* to** (point at the right alternative tool).
- **Schema** — descriptive param names, `enum`s over free strings, stated defaults and units, required vs. optional correct.
- **Returns** — say what comes back and in what shape, so Claude can plan the next step.
- **Errors** — actionable strings the model can act on (`"No tickets matched; try broader terms"`), and distinguish *empty result* from *failure*.

**Capability bloat** is the exam's favourite integration smell: too many tools, or overlapping ones, degrade selection accuracy and burn context on every request. Fixes: consolidate overlapping tools, scope tool sets **per agent/task**, and prefer few well-described tools over many thin ones.

### Progressive discovery vs. monolithic context

**Monolithic:** load everything (all tools, all schemas, the whole KB) into context up front. Simple, predictable, expensive — and accuracy *degrades* as irrelevant material crowds the window.
**Progressive discovery:** expose an index/summary, let the agent pull detail on demand (list → describe → fetch). Cheaper, scales to large tool/data surfaces, costs an extra round trip and depends on good descriptions.

**Rule of thumb:** small, stable, always-needed → monolithic (and **cache** it). Large, sparse, or per-request-variable → progressive.

### RAG design

| Decision | Options | Choose on |
| -------- | ------- | --------- |
| **Chunking** | fixed-size + overlap · semantic/paragraph · structural (headings, sections) · whole-document | Document structure and whether answers span boundaries. Structural beats fixed size for docs with real headings |
| **Indexing** | dense/embedding · sparse/keyword (BM25) · **hybrid** · metadata filters | Query type — jargon, IDs, and codes need lexical; paraphrase needs dense; **hybrid is the safe default** |
| **Retrieval** | top-k · hybrid + **rerank** · query rewriting/expansion · multi-hop | Precision requirements and latency budget |
| **Context assembly** | k, ordering, dedup, citation metadata | Window budget and whether you must cite |

**Match the strategy to data shape and query pattern** — that phrasing is straight from the blueprint:

- Long structured docs, narrow factual questions → structural chunking + hybrid + rerank.
- Short records (tickets, rows) → whole-record chunks + **metadata filtering** first, then search.
- Code → structural (function/class) chunking; lexical matters a lot.
- Highly volatile data → don't RAG it, **call the system of record as a tool**.

**Failure modes to name:** stale index, chunk boundaries splitting the answer, retrieval that's fine but the model ignores it (put retrieved context in a delimited block and instruct grounding), no citations, and "retrieval hides a permissions bug" — see below.

### Auth & authz — where the security gaps hide

- **Authentication** (who is calling) vs. **authorization** (what they may do). Exam scenarios usually break on the second.
- **The classic gap:** the agent authenticates as a **service account with union-of-all-users permissions**, so a user can retrieve documents they can't see in the source system. Fix: **propagate the end-user identity** (OAuth on-behalf-of / token exchange) and filter retrieval by the *user's* entitlements, at query time, in the index.
- **Least privilege per tool** — scope credentials to the narrowest capability; read and write tools get different scopes.
- **Secrets** never in prompts, `CLAUDE.md`, or committed config; inject via environment/secret manager.
- **Multi-tenancy:** partition indexes/keys per tenant; a shared cache or index is a cross-tenant leak waiting to happen.
- **Human approval gates** on irreversible or high-value actions, enforced in code — not by asking the model nicely.

### Accuracy–latency trade-offs

Every accuracy lever costs time: reranking (+100–300 ms), multi-hop retrieval (+a round trip each), extended thinking, self-consistency/voting (×N), a bigger model, larger k. **Justify the configuration against the SLA:** pay for accuracy where the failure cost is high (a refund decision), buy latency back where it isn't (autocomplete). Standard moves: cheap model to route → expensive model only on the hard path; cache the stable prefix; stream so *perceived* latency drops; async/batch anything not interactive.

### Observability at scale

- **Trace every request end-to-end**: prompt version, model, tool calls + arguments + results, retrieval hits, tokens in/out, cache hits, latency per hop, stop reason, final decision.
- **Metrics:** p50/p95/p99 latency, cost per request/task, tool error and retry rates, retrieval hit rate, escalation rate, task success.
- **The hard part at scale:** volume (sample intelligently, but keep **100%** of failures and escalations), **PII in traces** (redact at capture), non-determinism (you need the exact prompt+context to reproduce), and multi-agent traces that interleave — correlate with a request/trace ID through every subagent and tool call.
- Wire alerts to **behaviour**, not just errors: escalation-rate spike, cost-per-task drift, retrieval hit-rate drop, output-length anomalies.

## You must be able to

- [ ] Choose **MCP vs. direct API/CLI vs. agent-to-agent** and defend it with the N×M argument.
- [ ] Explain MCP host/client/server and **who controls** tools vs. resources vs. prompts.
- [ ] Pick **stdio vs. Streamable HTTP** and name what remote adds (OAuth, tenancy, scaling).
- [ ] Spot **capability bloat** and rewrite a bad tool description.
- [ ] Design a RAG pipeline: chunking, indexing, retrieval, assembly — **matched to data shape and query pattern**.
- [ ] Find the **authorization gap** in an integration (the service-account trap) and fix it.
- [ ] Justify an accuracy–latency configuration against a stated SLA.
- [ ] Choose a **monitoring strategy at scale** and say what you'd sample vs. keep in full.
- [ ] Argue **progressive discovery vs. monolithic context** for a given tool/data surface.

## Resources

- [Model Context Protocol](https://modelcontextprotocol.io/) — spec, transports, auth, SDKs, quickstart.
- Anthropic docs — *Tool use*, *MCP*, *Agent SDK*, *Contextual retrieval* / RAG guidance, *Prompt caching*.
- Companion note: [Model Context Protocol](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/model-context-protocol.md)

## 🔎 Self-check

1. Your team has built the same Jira connector three times for three apps. What's the argument for MCP, in one sentence?
2. Which MCP primitive is **model-controlled**, which is **application-controlled**, which is **user-controlled**?
3. An agent can retrieve HR documents its caller isn't cleared to see. What's the root cause and the fix?
4. Contracts are 80-page PDFs with numbered clauses; users ask "what's the termination notice period in the Acme contract?" Design the retrieval.
5. An agent has 40 tools and picks the wrong one ~15% of the time. Give three fixes.
6. When is **progressive discovery** worse than just loading everything?
7. You must cut p95 latency by 40% and can lose 2% accuracy. Name four levers, in the order you'd pull them.
8. What do you log for a multi-agent run so you can reproduce a bad answer next week?
