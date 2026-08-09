# 🧪 Practice — Hands-On Projects, Mock Exams & Self-Quiz

The exam is closed-book and scenario-based, so **doing** beats reading. This folder is your active-practice toolkit.

## 🩺 Start here — [self-assessment.md](self-assessment.md)

10 short-answer diagnostic questions across all 5 domains, with model answers and a weighted scorecard. **Take it on Day 1** to find your weak spots, and **retake it on Day 39** to prove you closed them.

## 🛠️ Hands-on mini-projects (do at least these)

Each is small (~1–2 hrs) and maps to a domain. Building them makes the concepts stick.

| Project | Domain | Day | What you'll internalize |
| ------- | :----: | :-: | ----------------------- |
| **Decompose a business problem end-to-end** (on paper: patterns, controls, feedback loop) | 1 | 6 | Turning an outcome into an architecture |
| **Build a tiny MCP server (stdio)** exposing one tool over a real API | 3 | 14 | Tool schema + JSON-RPC + MCP wiring |
| **Structured extraction pipeline** (schema + validation + retry + caching) | 2 | 18 | Reliable output, validation, cache hit rate |
| **Build an eval harness** (20 real examples + a grader you can re-run) | 4 | 23 | The skill most engineers skip |
| **Configure Claude Code for a real repo** (`CLAUDE.md`, a command, an allowlist, a hook) | 7 | 34 | Team configuration & permissions |

> Keep each project's code/notes in a subfolder here (e.g. `practice/mcp-server/`) so you can revisit before the exam.

## 🧪 Mock exams (Week 6)

You don't have the real question bank — so **build your own**:

1. **Write ~15 scenario questions per domain** as you study (put them in `practice/questions/`). Each: a short scenario + 4 options + the correct answer + *why*. Writing good distractors is itself great revision.
2. Assemble a **63-question mock**, stratified to the real weights: **~12 Integration · ~11 Solution Design · ~10 Evaluation · ~9 Governance · ~9 Stakeholder/Lifecycle · ~8 Models/Prompting · ~4 Dev Productivity.** Take it **timed (120 min), closed-book**.
3. **Grade by domain** — your lowest domain is your next study target.
4. Repeat with fresh questions for mocks #2 and #3. **Target ≥ 80%.**

Alternatively, use reputable practice-question sets from the [resources](../resources/) list — but treat unofficial banks skeptically and always verify against Anthropic docs.

## 🔁 Self-quiz method (daily)

- Turn each domain guide's **"You must be able to"** bullets into flashcards (Anki, or a `practice/flashcards.md`).
- Answer each domain's **Self-check** questions from memory; if you stumble, re-study and log it.
- **Spaced repetition:** re-run last week's flashcards on buffer days.

## 📏 Readiness bar (before booking / sitting)

- [ ] Completed all 7 domain guides + self-checks.
- [ ] Built at least 3 of the hands-on projects.
- [ ] Run all 7 [design drills](../scenarios/).
- [ ] Scored **≥ 80%** on 2+ timed mocks.
- [ ] Confident on **Integration (19%), Solution Design (17%), and Evaluation (16%)** specifically — 52% of the exam.
