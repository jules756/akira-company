# HEARTBEAT — Follow-Up Manager

Run every heartbeat. Wakes: schedule (daily), Gmail watch trigger, mention, approval.

## 0. Env check

Required:

- `PAPERCLIP_AGENT_ID`, `PAPERCLIP_COMPANY_ID`, `PAPERCLIP_API_URL`, `PAPERCLIP_API_KEY`, `PAPERCLIP_RUN_ID`
- `NOTION_API_KEY`, `NOTION_CRM_DB_ID`, `NOTION_OUTREACH_LOG_DB_ID`
- `GMAIL_CREDENTIALS_REF` (read + reply scope)
- `GOOGLE_CALENDAR_CREDENTIALS_REF`
- `GOAL_ID`

Missing → comment, `blocked`, exit.

## 1. Identity

```
GET /api/agents/me
```

Read wake vars. If `PAPERCLIP_WAKE_REASON=gmail_watch`, jump to step 4 first.

## 2. Read the schema

Open [`../crm-steward/life/areas/crm-schema.md`](../crm-steward/life/areas/crm-schema.md). Note current field names and stage options. If snapshot is >24h old: comment on CRM Steward's "CRM Daily" issue asking for refresh; continue with step 4 (reply handling) but skip step 6 (re-engagement).

## 3. Read the cadence patterns

Open [`knowledge/follow-up-patterns.md`](knowledge/follow-up-patterns.md). Read default and learned rules. These drive step 6.

If the file doesn't exist yet, create it with the seed defaults from `AGENTS.md`.

## 4. Gmail sweep

All external tool calls go through Composio (skill: `composiohq/skills/composio`). Fetch new Gmail messages since last heartbeat marker stored in `./memory/state/last-gmail-check.txt`.

For each new message:

1. **Match.** Find the Notion Company where `Contact Email` matches the sender (use Composio Notion search). If none, add to `life/areas/unmatched-replies.md` for CRM Steward to reconcile; skip to next.
2. **Classify.** Determine Outcome: Meeting booked · Spoke to them · Not interested · Follow-up needed · No answer (auto-reply / OOO).
3. **Request the log via Steward.** Create a Paperclip subtask:
   ```
   POST /api/companies/{PAPERCLIP_COMPANY_ID}/issues
   X-Paperclip-Run-Id: <PAPERCLIP_RUN_ID>

   {
     "title": "[steward-write] outreach-log — <Company Name> email reply",
     "description": "company_url: <URL>\nchannel: Email\ndate: <YYYY-MM-DD>\noutcome: <classified>\nnotes: <one-paragraph summary>\nrecontact_on: <per cadence, or null if Not interested>",
     "assigneeAgentId": "<crm-steward-agent-id>",
     "parentId": "<CRM-Daily issue id>",
     "goalId": "<GOAL_ID>",
     "priority": "high",
     "metadata": { "billingCode": "sales" }
   }
   ```
   Do **not** write to Notion yourself. The Steward will execute and reply with the Notion page URL.
4. **Escalate.** If Outcome ∈ {Meeting booked, Not interested, Feedback meeting} OR reply mentions pricing / contract / decision-maker / multi-venue → post `SIGNAL — reply from <Company Name>: <one line>` comment on Head of Sales' "CRM Daily" issue.
5. **Archive** the Gmail thread (never delete). Update `last-gmail-check.txt`.

> LinkedIn DMs are **not your channel**. When step 6 decides a lead needs a LinkedIn follow-up, delegate to LinkedIn Engagement Agent (Marketing team) instead of sending yourself.

## 5. Approvals and assignments

Standard handling — if `PAPERCLIP_APPROVAL_ID` set, handle first. Then:

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={you}&status=todo,in_progress,in_review,blocked
```

## 6. Re-engagement scan

Only run if schema snapshot is fresh.

Query Companies (CRM) for leads where:
- Pipeline Status ∈ active stages (Lead, Discovery, Proposal — skip Active/Lost/On hold)
- The latest Outreach Log entry has `Recontact On` ≤ today
- OR no Outreach Log entry exists but the record was created ≥ cadence threshold ago

For each candidate:

1. Check attempt count (count of Outreach Log entries with Outcome in {No answer, Left voicemail, Follow-up needed, Spoke to them}).
2. If attempts ≥ 3 and no reply has ever landed → update Pipeline Status is NOT your job; instead, delegate to Head of Sales:
   ```
   POST /api/companies/{companyId}/issues
   {
     "title": "Stale lead — 3 attempts no reply: <Company Name>",
     "description": "3 touches, no reply. Recommend move to On hold.",
     "assigneeAgentId": "<head-of-sales-id>",
     "parentId": "<CRM-Daily issue id>",
     "goalId": "<GOAL_ID>",
     "priority": "normal"
   }
   ```
3. If attempts < 3 → delegate to Content Writer:
   ```
   POST /api/companies/{companyId}/issues
   {
     "title": "Draft re-engagement email — <Company Name>",
     "description": "<one line summary of history>, last Outcome=<x>, want: <re-engage tone>",
     "assigneeAgentId": "<content-writer-id>",
     "parentId": "<CRM-Daily issue id>",
     "goalId": "<GOAL_ID>",
     "priority": "normal",
     "metadata": { "billingCode": "sales" }
   }
   ```
4. When Content Writer returns the draft, send via Gmail and log as step 4 substeps above (Channel: Email, Outcome: No answer — to be updated on reply).

## 7. Demo booking + pre-call brief (Cold-to-Cash workflow)

For leads whose latest Outreach Log Outcome = `Meeting booked`:

### 7a. Book the call

Use Composio → Google Calendar to create a 30-min event in Jules' calendar, title `Akira — <Company Name>`, send invite to the Contact Email. Note the confirmed date/time.

### 7b. Generate the pre-call brief immediately

This is a booking-time action — don't defer it to Head of Sales' call-day heartbeat. See [`knowledge/pre-call-brief-template.md`](knowledge/pre-call-brief-template.md) for the full prompt and output structure.

1. **Gather inputs via Composio:**
   - All LinkedIn DMs between Jules and the prospect.
   - All Gmail threads matching the prospect's Contact Email.
   - CRM Company record for this prospect.
   - Outreach Log entries related to this Company (last 90 days).
   - LinkedIn engagement-log entries naming this prospect (from `../linkedin-engagement/life/areas/engagement-log.md`).
   - Prospect sheet row (if source was Apollo / LinkedIn-Engagement / directory).
2. **Run the brief prompt** (see template). Output: structured markdown (5-point summary, objections, interest level, company context, probable motivation). Max 200 words. Match prospect's language.
3. **Attach to Notion** via Steward:
   ```
   Title: [steward-write] append-page-body — <Company Name> pre-call brief
   Body (YAML):
     company_url: <Notion page URL>
     section_title: Pre-call Brief — <call date>
     body_markdown: |
       <generated brief>
   ```
4. **Notify Head of Sales** — post a comment on the "CRM Daily" issue: `Pre-call brief ready for <Company Name> — call <date/time>. Language: <SE|EN|FR>. Key signal: <one-line from brief>.`

### 7c. If the call is rescheduled >48h later

Regenerate the brief before the new date. Fresh context may have arrived (new LinkedIn post engagement, new email thread, new signals). One brief per call, not one per booking.

## 8. Weekly synthesis (Fridays only)

If today is Friday (or it's been ≥7 days since last synthesis):

1. Read Outreach Log entries for the last 7 days.
2. Group by cadence bucket + Restaurant Type + Outcome.
3. Identify one or two patterns where outcomes diverge meaningfully from defaults.
4. Update `life/areas/follow-up-patterns.md` with the new rule, citing evidence.
5. Post a DIGEST to Head of Sales summarizing the learning.

## 9. Exit

- Comment on any `in_progress` work.
- `X-Paperclip-Run-Id` on all mutating calls.
- Clean exit.
