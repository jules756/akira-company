---
name: "Company Ops"
slug: company-ops
status: in_progress
color: "#64748b"
---

# Company Ops

The CEO + Auditor layer. Governance, weekly audit-log review, engineering retros that affect company-wide direction, board briefs, and cross-functional coordination issues that don't cleanly belong in any single team's project.

## Scope

- **CEO daily rhythm** — identity check, approval queue, delegation, fact extraction.
- **Auditor daily scoring** — rubric runs across all active agents; SIGNALs to CEO when drift is material.
- **HR loop** — CEO reads Auditor findings, acts on flagged agents (corrective comment / update AGENTS.md / pause).
- **Weekly governance review** — CEO's Friday audit-log scan posted to the standing `Governance Review` issue.
- **Monday board brief** — CEO summarizes the week for Jules (prev-week outcomes + this-week bet).
- **Approval handling** — CEO routes hiring / strategy / structural-change approvals to Jules.
- **Governance-phase advancement** — CEO proposes Phase 1 → 2 → 3 transitions based on evidence from all rubrics.

## Primary agents

- CEO (owner)
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
- Phase advancement happens on evidence, not wishful thinking.
