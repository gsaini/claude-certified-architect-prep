# Domain 4 — Tool Design & MCP Integration

> **Exam weight: 18%** · Designing tools Claude uses well, and integrating via the **Model Context Protocol (MCP)**.

## Why it matters

Agents are only as good as their tools. Scenarios test whether you can **design tools Claude won't misuse** and **integrate external systems via MCP** cleanly. You already have a head start: see the companion [MCP note](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/model-context-protocol.md).

## Key concepts

### Tool design (this is prompt engineering for tools)
- A tool = **name + description + input JSON Schema**. Claude decides whether/when to call it *from these alone* — so they must be excellent.
- **Write descriptions for the model:** state *what it does*, *when to use it* (and when not), and what each parameter means. Vague descriptions → wrong or missing calls.
- **Prefer few, well-scoped tools** over many overlapping ones. Disambiguate similar tools explicitly.
- **Inputs:** clear names, tight schemas (enums/types/required), sensible defaults. **Outputs:** return concise, structured results; on error return an actionable message, not a stack trace.
- **Idempotency & side effects** — mark/guard consequential actions; gate irreversible ones behind confirmation.

### MCP essentials (the standard way to expose tools/data)
- **Architecture:** a **Host** (Claude app) contains **Clients**, each with a **1:1 connection** to a **Server**. Messages are **JSON-RPC 2.0**.
- **Three server primitives:**
  - **Tools** — model-controlled actions (JSON-Schema input).
  - **Resources** — app-controlled readable context (by URI).
  - **Prompts** — user-controlled templates.
- **Transports:** **stdio** (local subprocess) and **Streamable HTTP** (remote).
- **Client features:** *sampling* (server borrows the host LLM), *roots* (filesystem boundaries), *elicitation* (ask the user).
- **Security:** a tool result is **untrusted input** (prompt-injection risk); require consent for consequential actions; scope OAuth tokens tightly; isolate servers.

### Direct tools vs. MCP
- **Direct (API tool use)** — simplest for a single app's own tools.
- **MCP** — when tools/data should be **reusable across apps**, maintained independently, or shared (the M×N → M+N win).

## You must be able to

- [ ] Write a **tool description + schema** that Claude calls correctly (and know why a bad one fails).
- [ ] Diagnose "Claude isn't calling my tool / calls it wrong" → fix the description/schema.
- [ ] Explain **host/client/server** and the **three MCP primitives** (and who controls each).
- [ ] Choose **stdio vs. Streamable HTTP**, and **direct tools vs. MCP**.
- [ ] Apply the **security model** (untrusted results, consent, least-privilege tokens).

## Resources

- Companion: [Model Context Protocol note](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/model-context-protocol.md).
- Anthropic — Tool use guide; [modelcontextprotocol.io](https://modelcontextprotocol.io/) spec + SDK quickstarts.
- Build one small MCP server (stdio) as hands-on practice ([Day 11](../STUDY-PLAN.md)).

## 🔎 Self-check

1. Claude keeps calling the wrong one of two similar tools. What do you fix first?
2. What are the **three MCP primitives**, and who controls each (model/app/user)?
3. Local dev tool vs. a shared remote integration — which **transport** for each?
4. When do you choose **MCP over direct API tool use**?
5. Why is an MCP **tool result** a security concern, and how do you mitigate it?
6. Write (in words) a good description + schema for a `refund_order(order_id, amount)` tool. What guards does it need?
