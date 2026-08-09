# 🔗 Study Resources

Official first, everything else second. **When a community source and Anthropic's docs disagree, the docs win.**

## 🏛️ Official (authoritative)

- **[Anthropic Certification Program (Skilljar)](https://anthropic-partners.skilljar.com/)** — the exam, blueprint, policies, and Partner Academy prep courses (Claude Partner Network).
- **[Claude Certification via Pearson VUE](https://www.pearsonvue.com/us/en/anthropic.html)** — scheduling/proctoring.
- **[Anthropic Docs](https://docs.anthropic.com/)** — the single most important resource. Especially:
  - *Building Effective Agents* & the agents cookbook → Domain 1 (Solution Design)
  - *Tool use*, *MCP*, contextual retrieval / RAG guidance → Domain 3 (Integration)
  - *Prompt engineering*, *Structured outputs*, **Prompt caching**, *Models overview*, *Batch API* → Domain 2
  - Evaluation guidance + the Console **Evaluate** tool, *Reduce latency* → Domain 4
  - **Usage Policies**, Trust Center → Domain 5 (Governance)
  - *Claude Code* docs (Memory/CLAUDE.md, Settings, Hooks, Slash commands, Subagents) → Domain 7
- **[Model Context Protocol](https://modelcontextprotocol.io/)** — spec, transports, auth, SDKs, quickstarts → Domain 3.
- **[Anthropic Prompt Engineering interactive tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial)** and the **[Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)** — hands-on.
- **[Anthropic Academy / courses](https://www.anthropic.com/learn)** — free learning content.
- **[Architect – Professional prep path (Skilljar)](https://anthropic-partners.skilljar.com/path/claude-certified-architect-professional)** — the authoritative curriculum, and the best source for Domains 5 & 6.

## 🏛️ Governance & risk frameworks (Domain 5)

- **OWASP Top 10 for LLM Applications** — the standard vocabulary: prompt injection, excessive agency, sensitive information disclosure.
- **NIST AI Risk Management Framework** — govern / map / measure / manage. Useful framing for "how would you assess this risk?"

## 📝 Blueprint references (domains & weightings)

- ⭐ **[exam-logistics.md](exam-logistics.md)** — the Day 1 verified write-up: prerequisites, cost, format, retake policy, **both** the Foundations (5-domain) and Professional (7-domain) blueprints, each with sources and a confidence marker.
- freeCodeCamp — *Claude Certified Architect – Foundations prep* (context on the program).
- DEV.to — *"The Claude Certified Architect Exam: 5 Domains, 6 Scenarios"* (the domain weightings this plan uses — note these are the **Foundations** weightings).

> Treat any third-party "practice questions" or "brain dumps" with skepticism — verify every answer against the official docs, and never use material that claims to reproduce real exam questions (against exam policy).

## 📓 Companion notes library

Your own **[awesome-software-engineering](https://github.com/gsaini/awesome-software-engineering)** notes cover a lot of the substrate:

- [Loop Engineering](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/loop-engineering.md) → Domain 1 · [Building an Agent Evaluator](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/building-agent-evaluators.md) → Domains 4 & 5
- [Model Context Protocol](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/model-context-protocol.md) → Domain 3
- [Durable Execution](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/durable-execution.md) → Domains 1 & 6

## 🧰 Hands-on practice

- **Claude Code** itself — install it and configure a real repo (Domain 2 practice).
- **Build an MCP server** — the [MCP quickstart](https://modelcontextprotocol.io/quickstart) (Domain 3 practice).
- **The Anthropic Console** — Workbench for prompt iteration, and **Evaluate** for eval sets (Domains 2 & 4 practice).

---

### Adding a resource
Keep it high-signal and official-first. Add a one-line note on which **domain** it serves.
