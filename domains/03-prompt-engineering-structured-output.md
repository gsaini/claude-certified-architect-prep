# Domain 3 — Prompt Engineering & Structured Output

> **Exam weight: 20%** · Designing prompts for **reliable, production** behavior — especially structured (JSON) output and batch processing.

## Why it matters

Scenarios test whether you can get Claude to produce **dependable, parseable, correct** output at scale — not clever one-offs. Emphasis on structure, validation, and controllability.

## Key concepts

### Prompting fundamentals (roughly in order of leverage)
1. **Be clear and direct** — explicit instructions, the *why*, and the desired output format.
2. **Multishot (examples)** — 2–5 well-chosen examples dramatically improve reliability and format adherence.
3. **Chain-of-thought** — let Claude reason before answering for complex tasks (e.g. in `<thinking>` tags); improves accuracy, costs tokens/latency.
4. **XML tags** — delimit inputs/sections (`<document>`, `<instructions>`) so Claude parses structure unambiguously.
5. **System prompt / role** — set persona and task framing in the system prompt.
6. **Prefilling** — start the assistant turn to force a format (e.g. prefill `{` or `[` to force JSON; prefill to skip preamble).

### Structured / JSON output
- **Most reliable path: tool use ("function calling")** — define a tool with a JSON Schema; Claude returns arguments matching the schema. Prefer this over "please output JSON" for anything critical.
- **Prefill + stop sequences** to bound raw JSON output.
- **Always validate** the output against your schema and **handle failure** (retry, repair, or escalate). Output that *parses* isn't necessarily *correct* (echoes "coverage ≠ assertions").

### Output control
- **Stop sequences**, **temperature** (lower = more deterministic), **max tokens**, **streaming** for latency/UX.
- **Batch API** — process large volumes of independent prompts asynchronously at lower cost; know when batch vs. real-time.

### Evaluating prompts
- Build an **eval set** (inputs + expected outputs/criteria) and test prompt changes against it — test-driven prompting. Cross-ref: [Building an Agent Evaluator](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/building-agent-evaluators.md).

## You must be able to

- [ ] Improve an unreliable prompt using clarity, **examples**, XML tags, CoT, and prefill.
- [ ] Choose **tool use vs. prompt-for-JSON** for structured output, and justify it.
- [ ] Guarantee parseable output and **validate + handle malformed** results.
- [ ] Use **prefill and stop sequences** to control format.
- [ ] Decide **real-time vs. Batch API** for a workload.
- [ ] Design a small **eval** to compare two prompt versions.

## Resources

- Anthropic — Prompt Engineering docs (be clear, multishot, CoT, XML tags, system prompts, prefilling, chain complex prompts).
- Anthropic — Tool use / structured outputs guide; the Batch API guide.
- The Anthropic prompt-engineering interactive tutorial + prompt library.

## 🔎 Self-check

1. You need **guaranteed** schema-valid JSON for a downstream system. What's the most reliable technique, and why not just ask for JSON?
2. How does **prefilling** force JSON and remove preamble?
3. When is **chain-of-thought** worth the extra tokens — and when is it a waste?
4. A prompt works on 8/10 inputs. What's your process to make it reliable?
5. 500k documents to classify overnight, cost-sensitive — real-time or Batch API?
6. Why does "the JSON parsed" not mean "the extraction is correct," and what do you add?
