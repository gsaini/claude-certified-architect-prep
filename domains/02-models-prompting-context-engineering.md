# Domain 2 — Claude Models, Prompting & Context Engineering

> **Exam weight: 13%** · *"Choosing and steering models; engineering context at scale."*
> The smallest of the "core technical" domains, but it underpins the others — a cost or latency answer in [Integration](03-integration.md) usually turns on a model-selection or caching decision made here.

## Task statements

- Select the right Claude model on **cost / latency / capability** trade-offs
- Design **system prompts, templates, and guardrails**
- Apply prompt techniques (clarity, examples, structure, reasoning)
- Produce reliable **structured output**
- **Token optimization** and context engineering at scale

## Key concepts

### Model selection

> **Numbers verified 2026-08-06** against Anthropic's current model docs. Rates are per million tokens (MTok), input / output. Re-check before the exam — the exam tests the *reasoning*, but stale numbers make you doubt a correct answer.

| Model | ID | Context | $/MTok in / out | Reach for it when |
| ----- | -- | :-----: | :-------------: | ----------------- |
| **Opus 5** | `claude-opus-5` | 1M | $5 / $25 | Hardest reasoning, long-horizon agentic work, complex coding |
| **Sonnet 5** | `claude-sonnet-5` | 1M | $3 / $15 | The default for most production work — near-Opus quality, better cost/latency |
| **Haiku 4.5** | `claude-haiku-4-5` | 200K | $1 / $5 | Classification, routing, extraction, anything high-volume and simple |

**The architect's move is routing, not picking one model.** Classify or triage with Haiku, handle the body of work with Sonnet, escalate only the genuinely hard path to Opus. State the escalation trigger (confidence, complexity, stakes) — that's the part scenarios grade.

**Other levers on the same model:** `effort` (`low` → `max`) trades tokens and latency for depth; **adaptive thinking** lets the model decide how much to reason per request. Cheaper than upgrading a model tier, and often enough.

### Prompt techniques — and their costs

| Technique | What it buys | What it costs / when it's wrong |
| --------- | ------------ | ------------------------------- |
| **Be clear and direct** | The single highest-value change | Over-constraining a task that needs reasoning room |
| **Multishot examples** | Format and edge-case conformance | Tokens on every call; the model **overfits** unrepresentative examples |
| **Chain-of-thought / extended thinking** | Accuracy on multi-step reasoning | Latency and tokens — wasted on simple classification |
| **XML tags / structure** | Unambiguous section boundaries in long prompts | Clutter on a short prompt |
| **System prompt / role** | Task framing, persona, guardrails | Becomes a personality instead of a spec |
| **Prefill** | Forces output shape | ⚠️ **Removed on current models** — use structured outputs instead |
| **Prompt chaining** | Each step independently testable | Latency, and a new failure surface per hop |

> ⚠️ **Know this trap:** assistant-turn **prefill returns a 400** on the current model generation. If a scenario offers "prefill the assistant turn with `{`" as the way to force JSON, it's the wrong answer — the right one is a **schema**.

### Structured output — the architecture, not the prompt tweak

Asking for JSON in prose is the anti-pattern. In order of preference:

1. **Structured outputs** — declare a JSON Schema (`output_config.format`); the API constrains the response.
2. **Tool use with a schema** — define the target shape as a tool; `strict: true` guarantees the input validates.
3. **Validate in code** (Pydantic / zod) on every response — never trust the shape.
4. **On failure, retry once feeding back the validator's error.** A repair turn fixes most malformed output.
5. **Check `stop_reason`.** `max_tokens` means truncation — no prompt change fixes that; raise the limit or shrink the output.

Treat the residual failure rate as an **eval signal**, not noise.

### Context engineering

Context is a **budget you manage**, not a wall you hit. The levers:

| Lever | What it does | Cost |
| ----- | ------------ | ---- |
| **Prompt caching** | Reuse a stable prefix across requests | Cache reads ≈ **0.1×** input price; writes **1.25×** (5-min TTL) or **2×** (1-hr TTL) |
| **Compaction** | Summarize older turns server-side as the window fills | Lossy — early detail can't be recovered |
| **Context editing** | *Clear* stale tool results / thinking blocks (not summarize) | Cleared content is gone |
| **External memory** | Persist facts outside the window, re-inject what's relevant | You must decide relevance; stale memory misleads |
| **Retrieval** | Fetch on demand instead of stuffing | Retrieval quality becomes the ceiling; adds a hop |
| **Subagent offload** | Do the noisy work in a separate context, return only the conclusion | Extra call; the subagent lacks your context |

**Prompt caching is the highest-leverage cost lever, and it has one rule:**

> **It is a prefix match. Any byte change anywhere in the prefix invalidates everything after it.**

Render order is `tools` → `system` → `messages`. So: keep the stable content **first**, put volatile content (timestamps, per-request IDs, the user's actual question) **last**. Classic silent invalidators — memorize these, they're exam-shaped:

- `datetime.now()` or a UUID interpolated into the system prompt
- Non-deterministic serialization (`json.dumps` without `sort_keys`, iterating a set)
- **Changing the tool list mid-conversation** — tools render at position 0, so this invalidates *everything*
- **Switching models mid-conversation** — caches are model-scoped

Break-even: with the 5-minute TTL, **two** requests pay it back (1.25× write + 0.1× read vs. 2× uncached); with the 1-hour TTL you need **three**. Verify with `usage.cache_read_input_tokens` — a persistent zero means an invalidator is at work.

### Cost & latency toolkit

- **Batch API — ~50% off** for anything not latency-sensitive. Up to 100K requests per batch; most finish within an hour (24-hour ceiling). If a scenario says "overnight", "nightly", or "not user-facing", batch is almost certainly in the answer.
- **Stream** so *perceived* latency drops even when total latency doesn't.
- **Cap output** — output tokens cost 5× input; the biggest wins are usually in shortening responses, not prompts.
- **Route by difficulty** before you optimize anything else.

## You must be able to

- [ ] Choose a model for a scenario and **justify it on cost, latency, and capability** — then design the routing rather than picking one.
- [ ] Pick prompt techniques and name what each one **costs**.
- [ ] Design reliable structured output: schema → validation → repair-retry → `stop_reason` check.
- [ ] Explain the **prefix-match rule** for prompt caching and spot a cache-invalidating design.
- [ ] Give four distinct context-management strategies with their trade-offs.
- [ ] Say when **Batch API** is the right answer, and when it isn't.
- [ ] Write a system prompt with real guardrails, not just a persona.

## Resources

- Anthropic docs — *Prompt engineering* guide, *Structured outputs*, *Tool use*, **Prompt caching**, *Context editing*, *Compaction*, *Batch processing*, *Models overview* & *Pricing*.
- [Anthropic prompt-engineering interactive tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial) · [Cookbook](https://github.com/anthropics/anthropic-cookbook).

## 🔎 Self-check

1. A pipeline gets ~4% unparseable JSON. Give the architectural fix, not a prompt tweak.
2. Your prompt cache hit rate is zero across identical-looking requests. Name four things to check.
3. Nightly, 50,000 documents, no user waiting. Which model, which API, and why?
4. Name five prompt techniques and, for each, a case where it's the *wrong* tool.
5. A 100-turn support chat overruns the window. Give four strategies with trade-offs.
6. When does the 1-hour cache TTL beat the 5-minute one — and when does it lose?
7. A scenario offers "prefill the assistant turn to force JSON." Why is that wrong now, and what replaces it?
8. Where would you put a per-request timestamp so it doesn't destroy your cache?
