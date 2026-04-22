# Governance — Akira Agent

> Owned by CEO. Read by CEO on every heartbeat. Referenced by every other agent whenever they're deciding whether to act or request approval.
>
> **Framework source:** Headcount Zero (Anthony David Adams), Ch 9 "Kill Switch."
>
> **Last updated:** 2026-04-16

## Where we are today

**Phase 1 — Lock it down.** Just started. No customers yet, no live bots, team is being imported from disk into Paperclip. Every outbound communication is gated. Every agent budget is conservative. Jules reviews daily.

| Phase | Target entry trigger | Description |
|---|---|---|
| **1. Lock it down** | Day 1 (now) | Gate all external comms + publishing. Jules reviews all output. Conservative budgets (50% of expected). Daily audit log scans. |
| **2. Selective trust** | 2+ weeks of clean Phase 1 with no flagged incidents | Gate external comms + publishing for new topics/clients. Established workflows run ungated. Weekly audit reviews. Budgets tuned to observed usage. |
| **3. Exception-only** | 60+ days of clean Phase 2; real customers live | Gate only high-stakes (new hires, strategy, structural changes, large pricing). Routine ops autonomous. Monthly audit deep-dives. |

**Advance based on evidence, not enthusiasm.** Advance only when observed agent behavior justifies loosening. Regress immediately if an incident proves a gate was needed.

## Approval gates

### Always gated (all phases, never loosen)

| Action | Who requests | Who approves |
|---|---|---|
| Hiring a new agent | CEO (via `paperclip-create-agent` skill) | Jules (board approval) |
| Changing the org structure (reporting lines, renames, removals) | CEO | Jules |
| Changing pricing model or install-fee structure | Head of Sales | Jules |
| Contract / legal commitment on behalf of Akira | Any | Jules |
| Spending outside approved per-agent monthly budget | The agent | Jules (budget raise via UI) |
| Paperclip-server or Composio configuration changes | CTO | Jules |

### Gated in Phase 1 (loosen in Phase 2+)

| Action | Who approves | Loosening criteria |
|---|---|---|
| Publishing a LinkedIn post (Jules' account) | Head of Marketing reviews; Jules approves first 10 posts | 10 consecutive approved posts without voice drift → Head of Marketing-only approval in Phase 2 |
| Sending outbound LinkedIn DMs | Head of Marketing reviews; Jules approves first 10 DMs | Same |
| Sending a cold email campaign | Head of Sales + Jules | 10 successful campaigns → Head of Sales-only in Phase 2 |
| Publishing a Beehiiv newsletter issue | Head of Marketing + Jules | 4 weeks of clean issues → Head of Marketing-only |
| Publishing / going-live on a lead magnet | Head of Marketing + Jules | 3 magnets shipped clean → Head of Marketing-only |
| Moving a client Bot to Production | PM + Implementation Engineer verify, Jules approves | First 3 clients → PM + Implementation Engineer, no Jules in Phase 2+ |
| Committing + merging a branch | CTO reviews; Jules approves first 5 merges | 5 clean merges → CTO-only in Phase 2 |

### Never gated (always autonomous)

| Action | Owner |
|---|---|
| Reading Notion, Gmail (inbound only), LinkedIn feed, Apollo | All agents with the tool access |
| Writing to the CRM via Steward (after the Steward validates the request) | CRM Steward |
| Writing to Agents (The Bots) (PM only) | PM |
| Creating Paperclip subtasks / comments | Any agent |
| Updating agent's own `life/areas/` + `memory/` files | The agent that owns it |
| Logging an Outreach Log entry on a reply received | Follow-Up Manager (via Steward request) |
| Daily schema snapshot by CRM Steward | CRM Steward |
| Running `autoplan` / `review` / `retro` skills | Relevant agent |

## Monthly budgets (per-agent, seed)

Set in Paperclip UI → agent → budget. Values in USD. These are opening floors; raise after seeing real usage.

| Agent | Monthly cap | Reasoning |
|---|---|---|
| CEO | $120 | Strategic, 5-min heartbeat + on-event. Reasoning-heavy but short. |
| Auditor | $80 | Daily rubric scoring across ~15 agents. Lots of reads, cheap outputs. |
| CTO | $150 | Plans + reviews + retros. Deep thinking sessions. |
| Founding Engineer | $250 | Building code + skills. Long runs during features. |
| Implementation Engineer | $180 | Per-client config work; grows with customer count (+$50 per live Bot). |
| UX Designer | $80 | On-event only. Design specs are short bursts. |
| Product Manager | $120 | Daily scans + subtask orchestration. Low per-run, many runs. |
| Head of Sales | $120 | Daily CRM sweep + escalations. |
| CRM Steward | $100 | Event-driven; hourly background. Many small runs. |
| Follow-Up Manager | $100 | Daily Gmail sweep + cadence work. |
| Growth Analyst | $60 | Weekly brief + ad-hoc queries. |
| Lead Researcher | $120 | Daily Apollo runs + enrichment + secondary-source sweeps. |
| Head of Marketing | $100 | Daily + Monday ritual. |
| LinkedIn Engagement Agent | $120 | 5×/day wakes. Medium-cost. |
| Content Writer | $120 | On-event; long drafts occasionally. |
| Lead Magnet Creator | $100 | Daily + Friday ideation + build sprints. |

**Company-wide monthly cap: $2,000** (sum of per-agent + ~20% buffer).

**80% alerts go to CEO + Jules's Telegram once Telegram is wired** (deferred per earlier decision).

## Audit-log review ritual

Paperclip provides full activity logs (every mutation, before/after). Review cadence:

- **Daily (Jules, 5 min):** Scan the company dashboard for blocked tasks, budget alerts, stale work. Not the log line-by-line.
- **Weekly (CEO agent, Friday):** Read last 7 days of activity log (`GET /api/companies/{id}/activity`). Look for: unexpected actions, efficiency outliers, approval-gate triggers. Post findings to CEO's standing "Governance Review" issue, tagged to Jules.
- **Monthly (Jules, 30 min):** Pick one agent. Trace its entire month — issues, comments, spend, outputs. Surface improvements. Update agent's `AGENTS.md` or budget based on findings.

## Pause / Override / Terminate

Operational, not emergency. Normal tools.

- **Pause** an agent when investigating behavior or retuning — in Paperclip UI.
- **Override** a task — edit the issue description, change the assignee, cancel with a comment.
- **Terminate** an agent — irreversible; prefer `pause` unless certain.

Any of these taken by Jules without prior CEO-agent involvement should be noted in CEO's memory so the CEO stays informed.

## Data isolation

Akira is one company. No multi-company concerns today. If Jules later spins up another company on the same Paperclip instance (e.g., an experimental side-project), Paperclip isolates data/budgets/logs automatically — no agent cross-access.

## What this buys

Governance doesn't slow the company. It lets Jules trust the system. Hard budget caps + audit logs + approval gates = Jules can sleep. Without them, every autonomous action becomes a thing to worry about.
