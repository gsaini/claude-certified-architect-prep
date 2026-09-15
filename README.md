<div align="center">

# 🏛️ Claude Certified Architect – Professional · Prep

**A structured, 6-week, daily study plan for Anthropic's Claude Certified Architect – Professional exam — with domain guides, scenario drills, hands-on projects, mock exams, and a daily log.**

![Certification](https://img.shields.io/badge/Anthropic-Claude%20Certified%20Architect-D97757?style=for-the-badge&logo=anthropic&logoColor=white)
![Level](https://img.shields.io/badge/Level-Professional-4F46E5?style=for-the-badge&logo=readthedocs&logoColor=white)
![Plan](https://img.shields.io/badge/Plan-6%20weeks%20·%20daily-16A34A?style=for-the-badge&logo=googlecalendar&logoColor=white)
![Progress](https://img.shields.io/badge/Progress-Day%206%20of%2042-0EA5E9?style=for-the-badge&logo=checkmarx&logoColor=white)
![Exam date](https://img.shields.io/badge/Target%20exam-2026--10--20-DC2626?style=for-the-badge&logo=googlecalendar&logoColor=white)

</div>

---

## 🎯 The exam at a glance

| | |
| --- | --- |
| **Certification** | Claude Certified Architect – Professional |
| **Provider** | Anthropic (via [Skilljar](https://anthropic-partners.skilljar.com/) / [Pearson VUE](https://www.pearsonvue.com/us/en/anthropic.html)) |
| **Format** | Proctored · **closed-book · no AI assistance** |
| **Questions** | ~63, multiple-choice and multiple-response, scenario-framed |
| **Duration** | 120 minutes · pass mark **720/1000** · credential valid 12 months |
| **Cost** | $175 USD |
| **Audience** | Solution architects, senior AI/ML engineers, technical leads designing production Claude systems |

> ✅ **Verified 2026-08-06, re-verified 2026-09-13 (no changes):** **Foundations is *not* a prerequisite** — there are no mandatory certification prerequisites; the credential is awarded on exam performance alone. **$175**, proctored via Pearson VUE, ~63 items, 120 min, 720/1000 to pass, valid 12 months. Full sourced write-up: **[resources/exam-logistics.md](resources/exam-logistics.md)**.
>
> 📐 **Blueprint:** this repo targets the **Professional (CCAR-P) 7-domain** blueprint (Exam Guide v1.0, July 2026). It was rebuilt from the Foundations 5-domain structure on 2026-08-07 — ⚠️ the 7-domain split is third-party-sourced and still needs confirming against Anthropic's own PDF.

## 📊 The 7 exam domains (by weight)

| # | Domain | Weight | Study guide | Days |
| - | ------ | :----: | ----------- | ---- |
| 3 | **Integration** | **19%** | [domains/03](domains/03-integration.md) | 8–13 |
| 1 | **Solution Design & Architecture** | **17%** | [domains/01](domains/01-solution-design-architecture.md) | 2–6 |
| 4 | **Evaluation, Testing & Optimisation** | **16%** | [domains/04](domains/04-evaluation-testing-optimization.md) | 19–23 |
| 5 | **Governance, Safety & Risk Management** | **14%** | [domains/05](domains/05-governance-safety-risk.md) | 24–27 |
| 6 | **Stakeholder Communication & Lifecycle** | **14%** | [domains/06](domains/06-stakeholder-communication-lifecycle.md) | 29–32 |
| 2 | **Claude Models, Prompting & Context Engineering** | **13%** | [domains/02](domains/02-models-prompting-context-engineering.md) | 15–18 |
| 7 | **Developer Productivity & Operational Enablement** | **7%** | [domains/07](domains/07-developer-productivity-operational-enablement.md) | 33–34 |

Sorted by weight; numbering follows the official exam guide (v1.0, July 2026). The plan **allocates study time by weight** — Integration, Solution Design, and Evaluation are **52% of the exam** between them.

## 🗂️ Repository layout

```
claude-certified-architect-prep/
├── STUDY-PLAN.md      # ⭐ the full 6-week, day-by-day plan — start here after this README
├── domains/           # one study guide per exam domain (concepts, must-knows, self-checks)
├── scenarios/         # 7 design drills + how to practice each
├── practice/          # hands-on projects, mock-exam approach, self-quiz method
│   └── self-assessment.md   # ⭐ the Day 1 (and Day 39) 10-question diagnostic
├── daily-log/         # your daily prep journal (+ a copy-paste template)
└── resources/         # official + community study resources
    └── exam-logistics.md    # ⭐ verified cost, format, prerequisites, blueprint (+ sources)
```

## 🚀 How to use this repo

0. **Start with [resources/exam-logistics.md](resources/exam-logistics.md)** — which tier you're sitting, what it costs, and what's actually on it. Then take the [self-assessment](practice/self-assessment.md) to find your weak spots.
1. **Read [STUDY-PLAN.md](STUDY-PLAN.md)** — the 6-week schedule with a concrete task for each day (dates: **Day 1 = 2026-08-06**, resumed as Day 5 on 2026-09-13 **→ exam 2026-10-20**).
2. **Each day:** do that day's task using the linked [domain guide](domains/), then **write a short entry** in [daily-log/](daily-log/) (copy [TEMPLATE.md](daily-log/TEMPLATE.md)).
3. **Weekends / buffer days:** review, do a [hands-on project](practice/), or catch up.
4. **Weeks 5–6:** run the [design drills](scenarios/) and take timed [mock exams](practice/).
5. **Tick the checklist below** as you finish each domain and milestone.

## ✅ Progress tracker

- [x] Day 1 — Orientation & logistics ([log](daily-log/2026-08-06.md) · [findings](resources/exam-logistics.md) · [self-assessment](practice/self-assessment.md)) — ⬜ *still to do: book the exam (target Tue 2026-10-20)*
- [ ] Domain 1 — Solution Design & Architecture (17%) · Days 2–6
- [ ] Domain 3 — Integration (19%) · Days 8–13
- [ ] Domain 2 — Models, Prompting & Context (13%) · Days 15–18
- [ ] Domain 4 — Evaluation, Testing & Optimisation (16%) · Days 19–23
- [ ] Domain 5 — Governance, Safety & Risk (14%) · Days 24–27
- [ ] Domain 6 — Stakeholder Communication & Lifecycle (14%) · Days 29–32
- [ ] Domain 7 — Developer Productivity & Ops Enablement (7%) · Days 33–34
- [ ] All 7 design drills practiced
- [ ] Mock exam #1 · #2 · #3 (≥ 80% target)
- [ ] Final review + logistics check
- [ ] 🏛️ **Exam passed**

## 🔗 You're not starting cold

A lot of the underlying material is already written up in the companion **[awesome-software-engineering](https://github.com/gsaini/awesome-software-engineering)** notes library — the plan links to these where relevant:

- [Loop Engineering](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/loop-engineering.md) → Domain 1 (Solution Design)
- [Model Context Protocol (MCP)](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/model-context-protocol.md) → Domain 3 (Integration)
- [Building an Agent Evaluator](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/building-agent-evaluators.md) → Domains 4 & 5
- [Durable Execution](https://github.com/gsaini/awesome-software-engineering/blob/main/notes/durable-execution.md) → Domains 1 & 6 (operating long-lived systems)

## ⚖️ Disclaimer

This is an **unofficial, independent study plan** for personal exam preparation. It is built from Anthropic's **publicly available** exam blueprint (domains & weightings) plus Anthropic's published best-practice guidance — **not** from confidential exam content. The authoritative curriculum is the **[Anthropic Partner Academy](https://anthropic-partners.skilljar.com/)** courses (Claude Partner Network); use this plan to *organize* your prep, and supplement it with the official material. "Anthropic" and "Claude" are trademarks of Anthropic; this repo is not affiliated with or endorsed by Anthropic.

---

<div align="center">
<sub>Prep smart, build hands-on, pass once. ⭐ Star it to track along.</sub>
</div>
