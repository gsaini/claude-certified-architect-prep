# 📅 The 6-Week Daily Study Plan

> **Pace:** ~1 hr/day, balanced, with weekly buffer/review days. **42 days total.**
> Each day has **one focused task**. After it, write a short entry in [daily-log/](daily-log/) (copy [TEMPLATE.md](daily-log/TEMPLATE.md)).
> Domain time is allocated **by exam weight**: D1 (27%) → most days, D5 (15%) → fewest.

**Legend:** 📖 read/study · 🛠️ hands-on · 🔁 review · 🎯 scenario · 🧪 mock exam · ☕ buffer/rest

**Weekly map:** W1 Domain 1 · W2 Domain 1 finish + Domain 4 (MCP) · W3 Domain 2 (Claude Code) · W4 Domain 3 (Prompting) · W5 Domain 5 + scenarios · W6 mocks + final review.

---

## Week 1 — Agentic Architecture & Orchestration (Domain 1, 27%)

| Day | Focus | Task |
| --- | ----- | ---- |
| **Day 1** | 📖 Orientation & logistics | Read [README](README.md) + this plan. **Confirm whether Foundations is a prerequisite.** Register / pick a target exam date (~6 weeks out). Skim the [Domain 1 guide](domains/01-agentic-architecture-orchestration.md). Do a 10-question honest self-assessment across the 5 domains to find your weak spots. |
| **Day 2** | 📖 Agents vs. workflows | When to use an agent vs. a fixed workflow. Read Anthropic's *"Building Effective Agents."* Cross-ref: [Loop Engineering note](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/loop-engineering.md). |
| **Day 3** | 📖 Orchestration patterns | Prompt chaining, routing, parallelization, orchestrator–workers, evaluator–optimizer. Know *which pattern for which problem*. |
| **Day 4** | 📖 Multi-agent systems | Task decomposition, lead/subagent patterns, when multi-agent beats single-agent (and its costs). |
| **Day 5** | 🛠️ Design a multi-agent research system | On paper: decompose "research a topic" into search/analysis/synthesis subagents. Define each agent's role, tools, and hand-offs. |
| **Day 6** | 🔁 Review Domain 1 (part 1) | Make flashcards for the patterns. Answer the [self-check questions](domains/01-agentic-architecture-orchestration.md#-self-check). |
| **Day 7** | ☕ Buffer / rest | Catch up or lightly review flashcards. |

## Week 2 — Domain 1 finish + Tool Design & MCP (Domain 4, 18%)

| Day | Focus | Task |
| --- | ----- | ---- |
| **Day 8** | 📖 Reliability in agents | Guardrails, human-in-the-loop, stopping conditions, cost/latency trade-offs, failure handling. Finish Domain 1. Cross-ref: [Agent Evaluator note](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/building-agent-evaluators.md). |
| **Day 9** | 📖 MCP fundamentals | Host/client/server, the 3 primitives (tools/resources/prompts), transports. Cross-ref: [MCP note](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/model-context-protocol.md). Read [Domain 4 guide](domains/04-tool-design-mcp.md). |
| **Day 10** | 📖 Tool design | Writing tool descriptions Claude *won't* misuse, clear JSON Schemas, good param naming, error semantics. |
| **Day 11** | 🛠️ Build a tiny MCP server | Build a stdio MCP server exposing **one** tool over a real API. Feel the JSON-RPC + schema loop end to end. |
| **Day 12** | 📖 MCP in practice | stdio vs. Streamable HTTP, auth (OAuth), when MCP vs. direct tool use. Finish Domain 4. |
| **Day 13** | 🔁 Review Domains 1 + 4 | Mixed mini-quiz (self-written or from [practice/](practice/)). |
| **Day 14** | ☕ Buffer / 🛠️ end-to-end agent | Build one small agent end-to-end (tool + loop + a stopping condition). |

## Week 3 — Claude Code Configuration & Workflows (Domain 2, 20%)

| Day | Focus | Task |
| --- | ----- | ---- |
| **Day 15** | 📖 Claude Code fundamentals | What it is, the CLI, permission modes, when to use it. Read [Domain 2 guide](domains/02-claude-code-config-workflows.md). |
| **Day 16** | 📖 CLAUDE.md & memory | Project / user / enterprise `CLAUDE.md` hierarchy and precedence; what belongs in memory; best practices. |
| **Day 17** | 📖 Skills, commands, subagents | Custom slash commands, skills, and subagents in Claude Code — when and how. |
| **Day 18** | 📖 Hooks, settings, permissions, MCP | `settings.json`, hooks (automated behaviors), the permission/allowlist model, wiring MCP servers into Claude Code. |
| **Day 19** | 🛠️ Configure a real project | Set up Claude Code for one of your own repos: a `CLAUDE.md`, a custom command, a permission allowlist. |
| **Day 20** | 🔁 Review Domain 2 | Flashcards + [self-check](domains/02-claude-code-config-workflows.md#-self-check). |
| **Day 21** | 🎯 Scenario: Claude Code in CI/CD | Design automated code review + test generation in a pipeline. |

## Week 4 — Prompt Engineering & Structured Output (Domain 3, 20%)

| Day | Focus | Task |
| --- | ----- | ---- |
| **Day 22** | 📖 Prompting core | Clarity, examples (multishot), roles, chain-of-thought, XML tags, prefilling. Read [Domain 3 guide](domains/03-prompt-engineering-structured-output.md). |
| **Day 23** | 📖 Structured output | Reliable JSON via tool use / schemas, output validation, handling malformed output. |
| **Day 24** | 📖 Output control & batch | Stop sequences, prefill, temperature, streaming, and the Batch API for bulk jobs. |
| **Day 25** | 📖 Prompt evaluation | Writing evals for prompts; test-driven prompting. Cross-ref: [Agent Evaluator note](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/building-agent-evaluators.md). |
| **Day 26** | 🛠️ Structured extraction pipeline | Build a prompt that extracts validated JSON from messy documents, with a retry on schema-fail. |
| **Day 27** | 🔁 Review Domain 3 | Flashcards + [self-check](domains/03-prompt-engineering-structured-output.md#-self-check). |
| **Day 28** | 🎯 Scenario: Structured Data Extraction | Design a batch document-processing system with validation. |

## Week 5 — Context Management & Reliability (Domain 5, 15%) + Scenarios

| Day | Focus | Task |
| --- | ----- | ---- |
| **Day 29** | 📖 Context management | Context windows, compaction/summarization, memory across turns, long-conversation strategies. Read [Domain 5 guide](domains/05-context-management-reliability.md). |
| **Day 30** | 📖 Reliability & escalation | When Claude should defer/escalate to a human, confidence handling, retries, graceful degradation. |
| **Day 31** | 📖 Cost, latency & caching | Token optimization, **prompt caching**, model selection (Haiku/Sonnet/Opus), latency budgets. Finish Domain 5. |
| **Day 32** | 🎯 Scenario: Customer Support Agent | Autonomous resolution + *knowing when to escalate*. |
| **Day 33** | 🎯 Scenario: Multi-Agent Research | Orchestrate search/analysis/synthesis subagents. |
| **Day 34** | 🎯 Scenario: Developer Productivity (MCP) | Codebase-navigation utilities via MCP servers. |
| **Day 35** | ☕ Buffer / 🎯 remaining scenario | Practice **Code Generation with Claude Code** + revisit any weak scenario. |

## Week 6 — Mock Exams & Final Review

| Day | Focus | Task |
| --- | ----- | ---- |
| **Day 36** | 🧪 Mock exam #1 | 60 questions, **timed**, closed-book. Grade it; log which domains you missed. |
| **Day 37** | 🔁 Target weakest domain | Deep-review your #1 weak area from mock #1; redo its self-checks. |
| **Day 38** | 🧪 Mock exam #2 | Timed. Grade; compare to mock #1; note remaining gaps. |
| **Day 39** | 🔁 Target 2nd weakest + flashcard blitz | Review 2nd weak area; run all flashcards. |
| **Day 40** | 🧪 Mock exam #3 + scenario walk-throughs | Timed. Then talk through all 6 scenarios out loud. |
| **Day 41** | 🔁 Final review + logistics | One-page cheat-sheet of all 5 domains. **Confirm exam logistics:** Skilljar/Pearson VUE booking, ID, quiet room, system check, closed-book rules. |
| **Day 42** | 🏛️ Exam day | Light review only. Trust the prep. Pass. ✅ |

---

## 📈 Pacing notes

- **Behind schedule?** Use the ☕ buffer days (7, 14, 21, 28, 35) to catch up — don't skip the hands-on days.
- **Ahead / already strong on a domain?** Convert that day into extra scenario reps or an additional mock.
- **Target ≥ 80% on mocks** before exam day; if you're under, push the exam date and repeat Week 6.
- **The exam is closed-book and scenario-based** — prioritize *judgment* ("which pattern/tool/config fits this situation?") over memorizing syntax.

## 🔁 The daily loop (every study day)

1. Open today's row above → do the task using the linked [domain guide](domains/).
2. Copy [daily-log/TEMPLATE.md](daily-log/TEMPLATE.md) → `daily-log/YYYY-MM-DD.md`, fill it in (what you learned, confidence 1–5, questions).
3. Tick any completed item in the [README progress tracker](README.md#-progress-tracker).
4. Commit: `git commit -m "Day N: <topic>"`.
