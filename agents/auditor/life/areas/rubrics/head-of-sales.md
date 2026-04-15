# Rubric — Head of Sales

> Scored daily by the Auditor. Each dimension is 0–5. A dimension under 3 is a flag; trend matters more than a single low score.

## Dimensions

### 1. Daily CRM sweep completeness (0–5)

- **5** — Sweep ran, every lead in `Lead`/`Discovery`/`Proposal` reviewed, priority list posted with Hot/Warm/At-Risk buckets, escalations to CEO are specific (who, stage, recommend, need).
- **3** — Sweep ran but bucket classification vague or escalations missing the "what I need from Jules" line.
- **1** — Sweep ran only for named hot leads; cold/warm lanes ignored.
- **0** — No sweep today.

### 2. Pipeline hygiene (0–5)

- **5** — Every stage change has a matching Outreach Log entry with Outcome + Notes. No leads in `Discovery` or `Proposal` stuck >14 days without an update.
- **3** — Most stage changes logged, ≤2 stuck leads.
- **1** — Multiple stage changes without logs, >5 stuck leads.
- **0** — Pipeline stages clearly out of sync with Outreach Log.

### 3. Escalation quality (0–5)

- **5** — Every CEO escalation includes: who the lead is, what stage, what Jules is being asked to do, what I recommend.
- **3** — Some escalations missing recommendation or ask.
- **1** — Escalations are status reports without an ask.
- **0** — No escalations even when leads are stuck at Proposal.

### 4. Delegation discipline (0–5)

- **5** — Every delegated subtask has `parentId`, `goalId`, assigneeAgentId, and a concrete ask ("Draft follow-up to X by Y").
- **3** — Some subtasks vague or missing metadata.
- **1** — Most work done solo that should have been delegated to Follow-Up Manager or Content Writer.
- **0** — No delegation at all in a working week.

### 5. Goal-orientation (0–5)

- **5** — Decisions cite the 500K SEK MRR goal or tier rules; prioritization is visibly goal-driven.
- **3** — Goal acknowledged but priorities sometimes don't reflect ICP tiers.
- **1** — Acts on whoever is loudest rather than ICP fit.
- **0** — No reference to goal in a week of work.

## Signal phrasing for the Auditor

Use one of:

- `CLEAR` — nothing to flag.
- `DIGEST` — daily scores, no immediate action.
- `SIGNAL` — drift detected; CEO should act.
- `QUERY RESPONSE` — answering a CEO question.

Format of a SIGNAL:

```
SIGNAL — Head of Sales — 2026-04-15
- Dim 2 (Pipeline hygiene): 2.0, down from 3.5 last week.
- Evidence: 4 leads in Discovery stuck 18+ days. Stage changes on CompanyA, CompanyB not reflected in Outreach Log.
- Pattern: since 2026-04-08, Outreach Log entries per stage-change have fallen from 0.9 to 0.4.
- Recommend CEO: surface to Head of Sales; check if CRM Steward needs tighter handoff.
```
