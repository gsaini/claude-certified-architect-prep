# Domain 5 — Context Management & Reliability

> **Exam weight: 15%** · Managing information across long interactions, and making Claude systems **reliable** in production.

## Why it matters

The smallest domain, but it ties everything together: **how you keep the right context in the window, control cost/latency, and make the system fail safely.** Scenarios test judgment about long conversations, escalation, and optimization.

## Key concepts

### Context management
- **Context window** is finite — treat it as a **budget**. What earns a place: the task, the rules, the *relevant* history — not everything.
- **Long conversations:** summarize/compact older turns, keep a running state summary, and re-inject only what's needed. Persist durable state **outside** the window (a store) and pull it back in — the "write to disk, not to context" instinct.
- **Retrieval (RAG):** fetch only the relevant chunks into context instead of stuffing everything; relevance > volume.
- **Memory across sessions:** external memory (files/DB) the app reloads; know what belongs in-context vs. persisted.

### Reliability & escalation
- **Know when to escalate/defer to a human** — low confidence, high-stakes/irreversible actions, out-of-scope requests, repeated failure. Designing the *escalation boundary* is a core skill (see the Customer Support scenario).
- **Graceful degradation & retries** — handle tool failures, timeouts, and malformed output with retries/fallbacks, not crashes.
- **Guardrails & validation** on inputs and outputs; treat model output as untrusted until checked.
- **Observability** — log decisions, tool calls, and escalations so you can debug and improve (trace the trajectory).

### Cost, latency & optimization
- **Model selection:** match model to task — Haiku (fast/cheap) → Sonnet (balanced) → Opus (hardest reasoning). Use the cheapest that meets quality.
- **Prompt caching** — cache large, stable prompt prefixes (system prompt, context docs) to cut cost and latency on repeated calls. Know what's cacheable and the TTL trade-off.
- **Token optimization** — trim context, stream for perceived latency, batch where possible.

## You must be able to

- [ ] Manage a **long conversation** without blowing the context budget (summarize/persist/retrieve).
- [ ] Decide **what to keep in-context vs. persist externally**.
- [ ] Design an **escalation boundary** — when the agent hands off to a human.
- [ ] Add **retries, fallbacks, and validation** for graceful degradation.
- [ ] Choose a **model** and apply **prompt caching** for cost/latency.

## Resources

- Anthropic — docs on long context, prompt caching, model selection/overview, and reducing latency.
- Companion: [Building an Agent Evaluator](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/building-agent-evaluators.md) (reliability/eval), [Caching Strategies](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/caching-strategies.md) (TTL/staleness intuition behind prompt caching).

## 🔎 Self-check

1. A support chat has run 200 turns and is nearing the context limit. What do you do?
2. What belongs **in the context window** vs. **in external memory**?
3. Give three concrete triggers for **escalating to a human**.
4. Which model would you pick for (a) bulk classification, (b) hard multi-step reasoning — and why?
5. What is **prompt caching** best used for, and what's the trade-off?
6. A tool call times out mid-task. How should a reliable agent respond?
