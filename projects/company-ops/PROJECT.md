---
name: "Company Ops"
slug: company-ops
status: in_progress
color: "#64748b"
---

# Company Ops

The governance layer for the company. Company Ops is where routing, approvals, escalation, and cross-functional handoffs are kept explicit so the CEO stays strategic and the Chief of Staff handles daily coordination.

## Scope

- **CEO daily rhythm** — identity check, approval queue, delegation, fact extraction.
- **Chief of Staff routing** — receive ambiguous or cross-functional work, classify it into one of the four operating flows, split it into the right next-step brief, and prevent duplicate or orphaned work.
- **Auditor daily scoring** — rubric runs across all active agents; SIGNALs to CEO when drift is material.
- **HR loop** — CEO reads Auditor findings, acts on flagged agents (corrective comment / update AGENTS.md / pause).
- **Weekly governance review** — CEO's Friday audit-log scan posted to the standing `Governance Review` issue.
- **Monday board brief** — CEO summarizes the week for Jules (prev-week outcomes + this-week bet).
- **Approval handling** — CEO routes hiring / strategy / structural-change approvals to Jules.
- **Governance-phase advancement** — CEO proposes Phase 1 → 2 → 3 transitions based on evidence from all rubrics.
- **Cross-functional escalation** — anything that spans Sales, Marketing, Engineering & Product, or Customer Success gets routed through the Chief of Staff first unless the CEO decision itself is the issue.
- **Signal routing rule** — Sales passes lead objections, stage changes, and repeatable demand signals to the Chief of Staff; Marketing passes content performance, engagement spikes, and magnet signals to the Chief of Staff. The Chief of Staff decides the next owner and writes the handoff brief.

## Primary agents

- CEO (owner)
- Chief of Staff
- Auditor

## References

- [`governance.md`](../../agents/ceo/knowledge/governance.md) — phase model + approval-gate matrix + audit cadence + budgets.
- [`founder-role.md`](../../agents/ceo/knowledge/founder-role.md) — what Jules does vs. what the org does.
- [`company-goal.md`](../../agents/ceo/knowledge/company-goal.md) — the North Star.
- All rubrics under [`agents/auditor/rubrics/`](../../agents/auditor/rubrics/).

## Success criteria (ongoing)

- CEO board brief shipped every Monday.
- Governance Review issue updated every Friday with audit-log findings.
- Every Auditor SIGNAL gets a CEO-recorded response within 48h.
- Budget alerts (80% threshold) never sit unresponded > 24h.
- Cross-functional requests get routed without bouncing between teams.
- Phase advancement happens on evidence, not wishful thinking.
