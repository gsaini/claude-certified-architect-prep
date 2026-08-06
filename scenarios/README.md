# 🎯 Exam Scenarios

The exam embeds its questions in **real-world scenarios** — you'll see **4 randomly selected** from this pool of **6**. Each maps across multiple domains. For each, practice by **designing the solution out loud / on paper**, then checking your design against the "what it tests" and "watch-outs" below.

> The exam rewards **judgment in context**: "given *this* situation, which architecture / tool / config / prompt technique fits?" Practice reasoning, not recall.

| # | Scenario | Primary domains |
| - | -------- | --------------- |
| 1 | Customer Support Resolution Agent | 1 (agentic), 5 (reliability/escalation) |
| 2 | Code Generation with Claude Code | 2 (Claude Code) |
| 3 | Multi-Agent Research System | 1 (orchestration) |
| 4 | Developer Productivity Tools | 4 (MCP), 2 |
| 5 | Claude Code in CI/CD | 2 (Claude Code) |
| 6 | Structured Data Extraction | 3 (prompting/structured output) |

---

## 1. Customer Support Resolution Agent
**Goal:** resolve issues autonomously *while knowing when to escalate.*
- **Tests:** agent design + tools, the **escalation boundary**, guardrails, reliability.
- **Design it:** what tools? when to escalate (confidence, high-stakes, repeated failure)? how to avoid unsafe/irreversible actions? how to keep context over a long chat?
- **Watch-outs:** over-automation (acting when it should escalate); no stopping condition; unbounded context.

## 2. Code Generation with Claude Code
**Goal:** configure Claude Code for **team-wide** code generation and refactoring.
- **Tests:** `CLAUDE.md` hierarchy, commands, permissions, consistency across a team.
- **Design it:** what goes in project vs. user `CLAUDE.md`? which custom commands? what's allowlisted vs. prompted?
- **Watch-outs:** over-broad permissions; bloated CLAUDE.md; personal config where team config belongs.

## 3. Multi-Agent Research System
**Goal:** orchestrate specialized subagents for search, analysis, and synthesis.
- **Tests:** orchestrator–workers, task decomposition, when multi-agent is justified, cost.
- **Design it:** lead decomposes → subagents (search/analyze/synthesize) → merge. Define each role, its tools, and hand-offs.
- **Watch-outs:** multi-agent when single would do; no result-merging strategy; runaway token cost.

## 4. Developer Productivity Tools
**Goal:** codebase-navigation utilities via **MCP servers**.
- **Tests:** MCP integration, tool design, transport choice.
- **Design it:** what MCP server(s) expose which tools/resources? stdio or HTTP? how are tool descriptions written so Claude uses them right?
- **Watch-outs:** vague tool descriptions; too many overlapping tools; ignoring the untrusted-result risk.

## 5. Claude Code in CI/CD
**Goal:** automate code review and test generation in a pipeline.
- **Tests:** headless/non-interactive Claude Code, permissions, hooks, safety in automation.
- **Design it:** how does Claude Code run non-interactively? what permissions/allowlist? where do hooks enforce checks? how do you keep it safe with no human in the loop?
- **Watch-outs:** interactive assumptions in a pipeline; unsafe autonomy; no guardrails on what it can change.

## 6. Structured Data Extraction
**Goal:** process unstructured documents with **validation and batch operations**.
- **Tests:** structured output (tool use/schema), validation + retry, Batch API.
- **Design it:** tool-use schema for the target structure; validation + repair/retry on failure; real-time vs. Batch API for the volume.
- **Watch-outs:** "please output JSON" instead of tool use; no validation/repair; wrong batch-vs-realtime choice.

---

### How to drill (Weeks 5–6)
For each scenario: set a 10-minute timer, **write the architecture** (components, tools, patterns, guardrails, escalation), then critique it against the watch-outs. Log weak spots in [daily-log/](../daily-log/).
