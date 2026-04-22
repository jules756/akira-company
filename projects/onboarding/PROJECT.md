---
name: "Onboarding"
slug: onboarding
status: in_progress
color: "#6366f1"
---

# Onboarding

The bootstrap phase. Closes when the company is in Governance **Phase 2 (Selective Trust)** per [`governance.md`](../../agents/ceo/knowledge/governance.md).

## Scope

Every Day-0 and activation-plan task lives here until the company is self-running.

- Create the Paperclip Goal in UI → paste UUID into `GOAL_ID` secret.
- Wire all secrets in Paperclip's Secrets UI (see [`SECRETS-CHECKLIST.md`](../../../SECRETS-CHECKLIST.md)).
- Connect Composio integrations (Notion / Gmail / Calendar / Drive / LinkedIn / Apollo / Beehiiv / Tally + GitHub).
- Set per-agent monthly budgets per the checklist.
- Wire env-var map into each agent's adapter config.
- Unpause Week 1 five agents (CEO / Auditor / CRM Steward / Head of Sales / Follow-Up Manager) per [`ACTIVATION-PLAN.md`](../../../ACTIVATION-PLAN.md).
- Progress through Week 2–5 of the activation plan.
- First client Bot closing → Engineering team activation trigger.

## Primary agents

All of them, in ramp order — but the CEO owns this project's success.

## Success criteria (close this project when)

- Governance Phase 2 entered (≥2 weeks of clean Phase 1 runs, no unresolved Auditor SIGNALs).
- All 17 agents unpaused and running on their intended heartbeat cadences.
- All 8 secrets + 10 Composio connections live and healthy.
- First Production Bot live.

When all four are true, mark this project `done` and archive. The ongoing projects (demand-creation / sales-pipeline / skill-library-delivery / customer-success / company-ops) absorb the steady-state work from then on.
