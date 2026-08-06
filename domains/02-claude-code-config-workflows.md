# Domain 2 — Claude Code Configuration & Workflows

> **Exam weight: 20%** · How to configure and operate **Claude Code** (the agentic coding tool) for individuals and teams.

## Why it matters

Expect scenarios about **setting up Claude Code for a team**: shared context, custom commands, permissions, hooks, and CI/CD. Questions test *configuration judgment* — what goes where, and why.

## Key concepts

### CLAUDE.md — the memory hierarchy
- `CLAUDE.md` gives Claude persistent, project-specific context (conventions, architecture, commands, do's/don'ts).
- **Hierarchy & precedence** (most local wins): **enterprise/managed → project (`./CLAUDE.md`, committed) → user (`~/.claude/CLAUDE.md`, personal)**. Subdirectory `CLAUDE.md` files add local context.
- **Put in it:** build/test commands, code style, architectural constraints, "always/never" rules. **Keep it concise** — it's loaded into context.

### Custom slash commands & skills
- **Slash commands** — reusable prompts stored as Markdown in `.claude/commands/` (project) or `~/.claude/commands/` (user). Parameterizable.
- **Skills** — packaged instructions/workflows Claude invokes for a specific kind of task.
- **Subagents** — specialized agents Claude Code can delegate to (own prompt/tools/context).

### settings.json, permissions & hooks
- **`settings.json`** (project `.claude/settings.json`, local, and user) configures models, env, permissions, and hooks.
- **Permissions / allowlist** — control which tools/commands run without a prompt; tighten for autonomy, loosen carefully. Least privilege.
- **Hooks** — shell commands the harness runs on events (e.g. *PreToolUse*, *PostToolUse*, *Stop*). Use hooks for **deterministic automation** ("run the linter after every edit") — the harness executes them, not the model.

### MCP in Claude Code
- Add **MCP servers** to give Claude Code new tools/data sources (project or user scope). See [Domain 4](04-tool-design-mcp.md).

### Workflows
- **Team code generation & refactoring**, **CI/CD** (automated code review, test generation), **codebase Q&A/navigation**. Know how config choices (CLAUDE.md, commands, permissions, headless mode) enable each.

## You must be able to

- [ ] Explain the **CLAUDE.md precedence** order and what belongs at each level.
- [ ] Choose between a **slash command, a skill, a subagent, and a hook** for a given need.
- [ ] Configure **permissions** for safe autonomy (what to allowlist vs. prompt).
- [ ] Use a **hook** to enforce an automated, deterministic behavior.
- [ ] Wire an **MCP server** into Claude Code at the right scope.
- [ ] Design a **CI/CD** setup using Claude Code (headless/automation) for review or test-gen.

## Resources

- Anthropic — Claude Code docs: *Memory (CLAUDE.md)*, *Settings*, *Hooks*, *Slash commands*, *Subagents*, *MCP*.
- Anthropic — *Claude Code best practices*.
- Try the built-in `/init` to generate a starter `CLAUDE.md` for a repo.

## 🔎 Self-check

1. A rule must apply to **everyone on the team** in a repo — which `CLAUDE.md` level, and is it committed?
2. You want the test suite to run **automatically after every file edit**. Command, skill, or hook — and why?
3. What's the difference between a **slash command** and a **subagent**?
4. How do you let Claude Code run `npm test` without prompting, but still confirm `rm`?
5. Where do you configure an **MCP server** so only *this project* sees it?
6. Design a CI pipeline step where Claude Code reviews a PR. What config makes it safe and non-interactive?
