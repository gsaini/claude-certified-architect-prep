# Domain 7 — Developer Productivity & Operational Enablement

> **Exam weight: 7% — the smallest domain.** · *"Enabling teams to build and operate effectively."*
> Don't over-invest here. It's ~4 of 63 questions, and if you use Claude Code daily you already know most of it. Budget two study days, then spend the time on [Integration](03-integration.md) and [Evaluation](04-evaluation-testing-optimization.md).

## Why it matters

The architect's angle isn't "how do I use Claude Code" — it's **how do you roll it out to thirty engineers safely**: shared context, sane permissions, deterministic enforcement, and a non-interactive path for CI. Expect configuration-judgment questions: *what goes where, and why*.

## Task statements

- Configure **Claude tools and environments for teams**
- Support developer workflows (code generation, review, testing, codebase navigation)
- Enable safe, non-interactive automation

## Key concepts

### `CLAUDE.md` — the memory hierarchy

Persistent project context: build/test commands, conventions, architectural constraints, always/never rules.

**Precedence — more specific wins:** enterprise/managed → **project** (`./CLAUDE.md`, committed) → **user** (`~/.claude/CLAUDE.md`, personal). Subdirectory `CLAUDE.md` files add local context for that directory.

| Belongs in **project** (committed) | Belongs in **user** (personal) |
| ---------------------------------- | ------------------------------ |
| "Use `pnpm test`, never `npm test`" | "I prefer terse answers" |
| Architecture constraints, module ownership | Personal tooling preferences |
| Repo-specific gotchas everyone hits | Anything you'd be embarrassed to commit |

> **The tested trap: bloat.** `CLAUDE.md` is loaded into *every* request — it costs tokens continuously and dilutes attention. Short and high-signal beats comprehensive. `/init` generates a starter.

### Command vs. skill vs. subagent vs. hook

The discriminator is **who triggers it, and in which context**:

| Mechanism | Triggered by | Context | Right when |
| --------- | ------------ | ------- | ---------- |
| **Slash command** | The **user**, explicitly (`/deploy-check`) | Current | A human wants a repeatable prompt on demand |
| **Skill** | The **model**, when the task matches its description | Current, loaded on demand | Domain knowledge that should appear automatically but not sit in context always |
| **Subagent** | The **model**, delegating | **Separate** — returns a result, not a transcript | Work that would flood the main context, or needs isolation/parallelism |
| **Hook** | The **harness**, on an event (`PreToolUse`, `PostToolUse`, `Stop`) | N/A — shell command | **Deterministic** automation: "always run the linter after an edit" |

> **The exam question here is almost always the hook.** Anything phrased *"always / every time / automatically after X"* is a hook — because `CLAUDE.md` and prompts are **advisory** and the model may skip them, while a hook is executed by the harness. Same principle as [Governance](05-governance-safety-risk.md): prompt = request, code = guarantee.

### `settings.json` and permissions

Configuration lives in `.claude/settings.json` (project, **committed**), `.claude/settings.local.json` (personal, **git-ignored**), and user-level settings. It carries model choice, env vars, permissions, and hooks.

**Permission design is least-privilege:**

- Allowlist **narrowly scoped** commands (`Bash(gh pr view:*)`), never blanket `Bash(*)`.
- Read-only and idempotent operations are good allowlist candidates; anything destructive stays prompted.
- Secrets go in `settings.local.json` or an env var — **never** in a committed file or in `CLAUDE.md`.
- Permission modes trade autonomy for safety; the more autonomous the run, the tighter the allowlist must be.

### MCP in Claude Code

Wire MCP servers in at the right **scope** — project (shared with the team, committed) vs. user (just you) vs. local. Mechanics and design in [Integration](03-integration.md).

### Team workflows

- **Code generation & refactoring** — shared `CLAUDE.md` + custom commands make output consistent across the team.
- **Code review** — a command or subagent with a review checklist; see [Evaluation](04-evaluation-testing-optimization.md) for the report-everything-then-filter pattern.
- **Test generation** — pair with a hook that runs the suite so generated tests are verified, not assumed.
- **Codebase navigation / Q&A** — often better served by MCP servers exposing search over your systems.
- **CI/CD** — the scenario to be ready for. Non-interactive/headless invocation, an explicit narrow allowlist (no interactive prompts to answer), hooks enforcing checks, scoped credentials, and **bounded blast radius**: what can it change, and who reviews it? An automated agent with write access and no human gate is the wrong answer.

## You must be able to

- [ ] State the **`CLAUDE.md` precedence** order and what belongs at each level.
- [ ] Choose between **command / skill / subagent / hook** — and reach for the hook on "always do X."
- [ ] Design a permission allowlist for safe autonomy.
- [ ] Say what must never be committed, and where it goes instead.
- [ ] Wire an MCP server at the right scope.
- [ ] Design a **CI/CD** setup that's safe non-interactively.
- [ ] Explain why `CLAUDE.md` bloat is a real cost.

## Resources

- Anthropic — Claude Code docs: *Memory (CLAUDE.md)*, *Settings*, *Hooks*, *Slash commands*, *Subagents*, *Skills*, *MCP*.
- Anthropic — *Claude Code best practices*.
- Run `/init` on a real repo — the fastest way to internalize this domain.

## 🔎 Self-check

1. A rule must apply to everyone on the team in a repo. Which level, and is it committed?
2. The linter must run after every file edit, automatically. Command, skill, or hook — and *why*?
3. Slash command vs. subagent: what's the actual difference?
4. Allow `pnpm test` without prompting but still confirm `rm`. How?
5. Where does a personal API token for a local MCP server go?
6. Design a CI step where Claude Code reviews a PR. What makes it safe and non-interactive?
7. Why is a 400-line `CLAUDE.md` a problem even if everything in it is true?
