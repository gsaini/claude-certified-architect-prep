# 🏛️ Exam Logistics — Verified Facts

> **Verified:** 2026-08-06 (Day 1) · **Re-verified:** 2026-09-13 — no changes found · **Re-verify again:** Day 41 (Mon 2026-10-19), and any time the plan changes.
> Everything below is sourced. Where a fact could only be found on **third-party** sites, it is marked
> ⚠️ *unconfirmed* — the authoritative source is Anthropic's own **Exam Guide PDF** on the
> [certifications page](https://anthropic-partners.skilljar.com/), which requires a Partner Academy login.

---

## ❓ Day 1 question: is *Foundations* a prerequisite for *Professional*?

**No.** There is no mandatory certification prerequisite for the Professional tier.

- The official Skilljar **[Architect – Professional prep course](https://anthropic-partners.skilljar.com/path/claude-certified-architect-professional)** lists recommended pre-work as: *Claude 101, Claude Code in Action, AI Fluency: Framework & Foundations, Building with the Claude API, Introduction to Model Context Protocol, AI Capabilities and Limitations*. **Claude Certified Architect – Foundations is not among them.**
- Third-party exam guides state it directly: *"There are no mandatory prerequisites… the credential is awarded on exam performance alone."*
- What *is* recommended (not required): ~3+ years systems-architecture / platform-engineering experience, and ~6 months hands-on production work with Claude.

**➡️ Action:** you can register for Professional directly. No Foundations exam needed first.

---

## 🚨 The bigger Day 1 finding: two different blueprints

The 5 domains this repo *originally* studied — Agentic Architecture 27% · Claude Code 20% · Prompt Engineering 20% · Tool Design & MCP 18% · Context & Reliability 15% — are the **Claude Certified Architect – *Foundations* (CCAR-F)** blueprint, not Professional.

The **Professional (CCAR-P)** exam publishes a **different, 7-domain blueprint** (Exam Guide v1.0, effective July 2026) — this is what the repo now targets:

| # | CCAR-P domain | Weight | Guide |
| - | ------------- | :----: | ----- |
| 1 | Solution Design & Architecture | **17%** | [domains/01](../domains/01-solution-design-architecture.md) |
| 2 | Claude Models, Prompting & Context Engineering | **13%** | [domains/02](../domains/02-models-prompting-context-engineering.md) |
| 3 | Integration (MCP vs. API/CLI vs. agent-to-agent, RAG, auth/authz, observability) | **19%** | [domains/03](../domains/03-integration.md) |
| 4 | Evaluation, Testing & Optimisation | **16%** | [domains/04](../domains/04-evaluation-testing-optimization.md) |
| 5 | Governance, Safety & Risk Management | **14%** | [domains/05](../domains/05-governance-safety-risk.md) |
| 6 | Stakeholder Communication & Lifecycle Management | **14%** | [domains/06](../domains/06-stakeholder-communication-lifecycle.md) |
| 7 | Developer Productivity & Operational Enablement | **7%** | [domains/07](../domains/07-developer-productivity-operational-enablement.md) |

⚠️ *The 7-domain split is consistently reported by multiple independent third-party guides citing "Exam Guide v1.0, July 2026", but I could not open Anthropic's own PDF (login-gated). Confirm it against the official guide.*

**✅ Resolved 2026-08-07:** targeting **Professional**. The repo was rebuilt around the 7-domain blueprint — five Foundations guides retired, seven CCAR-P guides written, and the 42-day plan reallocated by the new weights.

---

## 🔁 Re-verification — 2026-09-13

Checked again before booking, after a five-week pause:

- **Price:** still **$175** on the [official Skilljar certification page](https://anthropic-partners.skilljar.com/claude-certified-architect-professional-certification). No retirement or change notice.
- **Blueprint:** still **Exam Guide v1.0, effective July 2026** — same 7 domains and weights, 63 items, 120 min, 720/1000, 12-month validity, same 14/30/90-day retake waits. ⚠️ Still third-party-sourced; the official PDF remains login-gated.
- **Target date:** moved from Wed 2026-09-16 to **Tue 2026-10-20**. Still **not booked**.

---

## 📋 Format, cost & policy

| | Foundations (CCAR-F) | Professional (CCAR-P) |
| --- | --- | --- |
| **Cost** | **$125** ([official](https://anthropic-partners.skilljar.com/claude-certified-architect-foundations-certification)) | **$175** ([official](https://anthropic-partners.skilljar.com/claude-certified-architect-professional-certification)) |
| **Items** | 60 ⚠️ | 63, multiple-choice + multiple-response ⚠️ |
| **Duration** | 120 min ⚠️ | 120 min ⚠️ |
| **Passing score** | 720 scaled (100–1,000) ⚠️ | 720 scaled (100–1,000) ⚠️ |
| **Structure** | 4 scenarios drawn from a bank of 6 ⚠️ | Standalone items — ⚠️ *the "6 scenarios" model may be Foundations-only* |
| **Domains** | 5 | 7 |
| **Delivery** | Pearson VUE — online-proctored or test centre | same |
| **Rules** | Proctored, **closed-book, no AI assistance** | same |
| **Validity** | 12 months ⚠️ | 12 months ⚠️ |
| **Retakes** | ⚠️ 14 / 30 / 90-day waits, up to 4 attempts per rolling year | same ⚠️ |

**Registration:** purchase on **[Anthropic Partner Academy (Skilljar)](https://anthropic-partners.skilljar.com/)**, then schedule the proctored sitting via **[Pearson VUE](https://www.pearsonvue.com/us/en/anthropic.html)**. The program went public in **July 2026** (previously partner-only). Foundations launched **12 March 2026**.

**Note the 12-month validity** — don't sit the exam far ahead of when you need the credential.

---

## ✅ Day 1 checklist

- [x] Confirm Foundations is *not* a prerequisite → **confirmed, it is not**
- [x] Confirm cost & delivery → **$125 / $175, Pearson VUE, proctored, closed-book**
- [x] Pull the blueprint → **found a mismatch between this repo and CCAR-P; see above**
- [x] **Decide the target tier** → **Professional (CCAR-P)**; repo rebuilt 2026-08-07
- [ ] **Purchase + schedule** the exam — target **Tue 2026-10-20** ([STUDY-PLAN.md](../STUDY-PLAN.md))
- [ ] Open the official **Exam Guide PDF** after login and replace every ⚠️ above with a confirmed fact

---

## 🔗 Sources

**Official**
- [Claude Certified Architect – Professional certification (Skilljar)](https://anthropic-partners.skilljar.com/claude-certified-architect-professional-certification) — $175
- [Claude Certified Architect – Professional prep course path (Skilljar)](https://anthropic-partners.skilljar.com/path/claude-certified-architect-professional) — recommended pre-work, no Foundations prerequisite
- [Claude Certified Architect – Foundations certification (Skilljar)](https://anthropic-partners.skilljar.com/claude-certified-architect-foundations-certification) — $125
- [Claude certification at Pearson VUE](https://www.pearsonvue.com/us/en/anthropic.html) — scheduling/proctoring

**Third-party (⚠️ verify against the official guide)**
- [Claude Certification Guide — Architect Professional blueprint](https://claudecertificationguide.com/blog/claude-certified-architect-professional-exam-guide) — 7 domains, 63 items, 720 cut
- [Claude Certification Guide — Architect Professional hub](https://claudecertificationguide.com/architect-professional) — retake policy, 12-month validity
- [Claude Certification Guide — Architect Foundations blueprint](https://claudecertificationguide.com/blog/claude-certified-architect-foundations-exam-guide) — the 5 domains this repo uses
- [Claude Certified Architect Prep — CCAR-P certification](https://claudearchitectcertification.com/certifications/ccar-p) — independent confirmation of the 7 domains
- [DEV — The Claude Certified Architect Exam: 5 Domains, 6 Scenarios](https://dev.to/aws-builders/the-claude-certified-architect-exam-5-domains-6-scenarios-and-everything-you-need-to-know-4le3) — the source of this repo's original structure
