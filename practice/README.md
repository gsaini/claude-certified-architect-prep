# 🧪 Practice — Hands-On Projects, Mock Exams & Self-Quiz

The exam is closed-book and scenario-based, so **doing** beats reading. This folder is your active-practice toolkit.

## 🛠️ Hands-on mini-projects (do at least these)

Each is small (~1–2 hrs) and maps to a domain. Building them makes the concepts stick.

| Project | Domain | What you'll internalize |
| ------- | :----: | ----------------------- |
| **Build a single agent end-to-end** (tool + loop + stopping condition) | 1 | The agent loop and control |
| **Build a tiny MCP server (stdio)** exposing one tool over a real API | 4 | Tool schema + JSON-RPC + MCP wiring |
| **Configure Claude Code for a real repo** (`CLAUDE.md`, a command, an allowlist, a hook) | 2 | Team configuration & permissions |
| **Structured extraction pipeline** (tool-use schema + validation + retry) | 3 | Reliable JSON, validation, batch |
| **A multi-agent research sketch** (orchestrator + 3 subagents) | 1 | Decomposition & orchestration |

> Keep each project's code/notes in a subfolder here (e.g. `practice/mcp-server/`) so you can revisit before the exam.

## 🧪 Mock exams (Week 6)

You don't have the real question bank — so **build your own**:

1. **Write ~20 scenario questions per domain** as you study (put them in `practice/questions/`). Each: a short scenario + 4 options + the correct answer + *why*. Writing good distractors is itself great revision.
2. Assemble a **60-question mock** (weighted: ~16 D1, ~12 each D2/D3, ~11 D4, ~9 D5) and take it **timed, closed-book**.
3. **Grade by domain** — your lowest domain is your next study target.
4. Repeat with fresh questions for mocks #2 and #3. **Target ≥ 80%.**

Alternatively, use reputable practice-question sets from the [resources](../resources/) list — but treat unofficial banks skeptically and always verify against Anthropic docs.

## 🔁 Self-quiz method (daily)

- Turn each domain guide's **"You must be able to"** bullets into flashcards (Anki, or a `practice/flashcards.md`).
- Answer each domain's **Self-check** questions from memory; if you stumble, re-study and log it.
- **Spaced repetition:** re-run last week's flashcards on buffer days.

## 📏 Readiness bar (before booking / sitting)

- [ ] Completed all 5 domain guides + self-checks.
- [ ] Built at least 3 of the hands-on projects.
- [ ] Drilled all 6 scenarios.
- [ ] Scored **≥ 80%** on 2+ timed mocks.
- [ ] Confident on Domain 1 (27%) specifically.
