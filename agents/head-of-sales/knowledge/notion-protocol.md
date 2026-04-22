# Notion Protocol — Rules Every Sales Agent Obeys

> **All Notion writes go through the CRM Steward.** No agent writes to Notion directly. Submit structured write-request subtasks with typed titles (see below); the Steward validates against this protocol and executes. The Steward is event-driven — it wakes on every subtask assignment — so latency is low. Rejected requests come back as comments with the reason; fix and resubmit.
>
> This file is owned by Head of Sales. Every sales agent reads it on every heartbeat. The CRM Steward is the canonical source of the current schema — do not hardcode fields; read them from [`../../../crm-steward/life/areas/crm-schema.md`](../../../crm-steward/life/areas/crm-schema.md) which the Steward refreshes hourly.

## The two live databases

| Name | ID | Purpose |
|---|---|---|
| 🏢 Companies (CRM) | `4150aee3-1f35-4306-bde8-001e710a31a1` | Contacted leads only. One row per company. |
| 📞 Outreach Log | `423b73be-7031-4691-bce3-cb78a49d0424` | One row per outbound touch (call, email, DM, in-person). |

These IDs are referenced via Paperclip secrets `NOTION_CRM_DB_ID` and `NOTION_OUTREACH_LOG_DB_ID`. Never hardcode in agent files. If you need them directly, read them from the CRM Steward's schema snapshot.

## The funnel model

```
Apollo / Research ──► Prospect list (Google Sheet in Drive)
                                │
                                │  FIRST TOUCH (call / email / DM / in person)
                                ▼
                         Notion CRM ── Pipeline stages ── Active / Lost / On hold
                                ▲
                                │
                    Outreach Log (every touch, forever)
```

**Notion = contacted leads only.** Prospects that have not been touched live in the Google Sheet. They do not get written into the CRM by anyone.

## The promotion rule

An entry enters the Companies (CRM) at the moment first-touch happens — not before. The agent performing the first touch (Content Writer for email, LinkedIn Engagement Agent for DM, Head of Sales for call) is responsible for:

1. Creating the Company row with stage `Lead`.
2. Filling: Company Name, Contact name, Contact Email, Phone Number, Website, Address, Restaurant Type.
3. Creating the initial Outreach Log row and relating it to the new Company.

## Pipeline stages (current, as of 2026-04-15)

Always verify via the CRM Steward's schema snapshot. Current values:

- **Lead** — first touched, no meaningful response yet.
- **Discovery** — responded, qualification or discovery call scheduled/held.
- **Proposal** — proposal sent, awaiting decision.
- **Active** — customer signed; delivery in progress or live.
- **Lost** — did not close.
- **On hold** — paused; recontact later.

Never move a lead backward without a comment explaining why (goes in Outreach Log `Notes`).

## Touch logging (Outreach Log)

Every outbound contact creates one row:

| Field | Rule |
|---|---|
| Company (relation) | Required. Links to the CRM row. |
| Channel | Phone / Email / DM / In person / Other. Pick exactly one. **DM is the generic direct-message channel (LinkedIn DM, Instagram DM, etc.) — specify which platform in the `Notes` field** (e.g., "Via LinkedIn DM"). If Jules adds dedicated platform options later, CRM Steward will surface them and agents switch automatically. |
| Date | When the touch happened. |
| Outcome | No answer / Left voicemail / Spoke to them / Meeting booked / Not interested / Follow-up needed / Feedback meeting. |
| Notes | Plain text. What was said, what was agreed, what's next. |
| Recontact On | Date for the next touch. Drives Follow-Up Manager's cadence. |

## Who **requests** what

All rows below = who submits the request. Executor is always CRM Steward.

| Request type | Title prefix | Requesters allowed |
|---|---|---|
| New Outreach Log row | `[steward-write] outreach-log` | Head of Sales, Follow-Up Manager, Content Writer, LinkedIn Engagement |
| First-touch promotion (create Company record) | `[steward-write] company-first-touch` | Head of Sales, Content Writer, LinkedIn Engagement |
| Pipeline stage change | `[steward-write] pipeline-status` | **Head of Sales only** (requires `evidence_outreach_log_url`) |
| Update `Recontact On` | `[steward-write] recontact-on` | Head of Sales, Follow-Up Manager |
| Enrichment (fill missing field) | `[steward-write] enrich-field` | Head of Sales; CRM Steward self-hygiene |
| Append a timestamped section to a Company page body | `[steward-write] append-page-body` | Head of Sales (call prep), Follow-Up Manager (demo prep), Content Writer (proposal link) |
| Dedupe / merge | (no request — Steward-initiated hygiene) | — |
| Delete a record | ❌ never | — |

## Request body format

Every `[steward-write] …` subtask's `description` is a YAML block. The Steward parses it mechanically.

**Example — Outreach Log row:**
```
company_url: https://www.notion.so/akira/...
channel: Email
date: 2026-04-15
outcome: Spoke to them
notes: Short summary of the reply.
recontact_on: 2026-04-18
```

**Example — Pipeline status change:**
```
company_url: https://www.notion.so/akira/...
from_stage: Discovery
to_stage: Proposal
justification: Proposal emailed, awaiting decision.
evidence_outreach_log_url: https://www.notion.so/akira/outreach-log/...
```

See each request type's required fields in the CRM Steward's AGENTS.md.

## Never-do

- Never create a Company CRM record for an untouched prospect. Those belong in the Google Sheet.
- Never delete a record. Use `Lost` or `On hold`.
- Never move a lead backward in pipeline without a comment.
- Never edit a field outside your role's allow-list above.
- Never skip Outreach Log logging because "it was a quick touch." No touch, no log = it didn't happen.

## If the schema changes

Only Jules changes the Notion schema manually. When he does:

1. CRM Steward detects the change on the next heartbeat (compares to last snapshot).
2. CRM Steward updates `life/areas/crm-schema.md`.
3. CRM Steward posts a `SCHEMA CHANGE` comment on a standing Paperclip issue owned by Head of Sales.
4. Head of Sales reviews; if the change affects agent behavior, files an HR issue with the Auditor or updates this file via a subtask.

No agent should break or flail on schema drift. Flag it; don't guess.
