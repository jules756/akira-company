# HEARTBEAT — Implementation Engineer

Event-driven + daily scan of active-client feedback queue. Wakes on: task assignment, mention, comment, approval. Deployment Engineer handles infrastructure; your work starts after a client is `active` and lasts through the client lifecycle.

## 0. Env

Required:
- `PAPERCLIP_*` standard
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID`
- `NOTION_CRM_DB_ID` (read-only — client context lookups)
- `NOTION_AGENTS_BOTS_DB_ID` (read-only — Bot status)
- `GOAL_ID`

No VM or Vapi / Twilio secrets anymore — you don't touch infrastructure.

Missing → comment, `blocked`, exit.

## 1. Identity + wake

```
GET /api/agents/me
```

## 2. Read context

- Read your own [`life/areas/clients/`](life/areas/clients/) for in-flight client state.
- Read Deployment Engineer's [`../deployment-engineer/life/areas/fleet.md`](../deployment-engineer/life/areas/fleet.md) to see which clients are active.

## 3. Approvals

Handle `PAPERCLIP_APPROVAL_ID` if set.

## 4. HR loop

Scan CEO corrective comments citing [`../auditor/rubrics/implementation-engineer.md`](../auditor/rubrics/implementation-engineer.md).

## 5. Assignments

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={your-id}&status=todo,in_progress,in_review,blocked
```

Priority:
1. Client-reported bug / incident (from Jules-forwarded portal feedback).
2. Stage-4 Testing sessions needing feedback capture.
3. Stage-2 knowledge-index validation support.
4. Pattern-surfacing (weekly, Fridays).
5. Other.

## 6. Client validation support (PRD Stage 2)

When Deployment Engineer hands off "crawler complete":

1. Read the knowledge-index files via the client's Supabase (`tone.md`, `questions.md`, `booking_patterns.md`, `edge_cases.md`, `INDEX.md`).
2. Write a summary comment on the onboarding issue: what the crawler found, what the client should review, direct portal links to each file.
3. Reassign to Jules for client-facing comms (Jules sends the portal invite + review request to the client).
4. Track progress via the portal. When client confirms validation:
   - Update the client's [`life/areas/clients/<slug>/README.md`](life/areas/clients/) with a validation-complete timestamp.
   - Comment on the onboarding issue → reassign to Deployment Engineer to unblock PRD Step 6.

## 7. Stage-4 Testing monitoring

When Deployment Engineer comments a new client is `active` (Step 6 complete):

1. Create `life/areas/clients/<slug>/` dossier: `README.md`, `feedback-log.md`, `support-log.md`, `observations.md`.
2. Monitor daily for 7 days via the client's Supabase `email_activity` log (read-only):
   - Emails received × classifier category.
   - Approval requests fired.
   - Edge cases triggered.
   - Response latency.
3. Hold a feedback session (Jules facilitates) at end of week:
   - Client validates what's working + what isn't.
   - You capture the session as a structured entry in `feedback-log.md`.
   - Route actions:
     - Config → client (via Jules, "update X in your portal").
     - Skill bug → open an issue for Founding Engineer with reproduction from Supabase logs.
     - Deploy issue → Deployment Engineer.
4. When client confirms Testing complete and PM flips the Bot to `Production`:
   - Write [`life/areas/clients/<slug>/handoff-to-csm.md`](life/areas/clients/) — dossier summary for Customer Success Manager.
   - Comment on the Bot's Notion page tagging CSM for retention handoff.

## 8. First-line support (live clients)

When a client issue arrives (via Jules forwarding from portal Support & Feedback):

1. Reproduce from Supabase logs where possible.
2. Triage:
   - Skill bug → Founding Engineer issue with: affected client, timestamp, log excerpt, reproduction steps.
   - Config → portal-guide comment to Jules for client.
   - Deploy → Deployment Engineer issue with: client slug, VM ID, symptom.
   - Integration (OAuth expired, API key changed) → portal-reauth nudge to client.
3. Log to [`life/areas/clients/<slug>/support-log.md`](life/areas/clients/) with: date, symptom, triage category, owner, resolution date (filled later).
4. Follow up: confirm the resolving agent landed the fix; confirm with the client via Jules.

## 9. Weekly pattern surfacing (Fridays)

1. Read every active client's `support-log.md` for the past 7 days.
2. Group by symptom. Patterns that hit 3+ clients are library-fix candidates.
3. Write a pattern report on CTO's standing "Fleet Patterns" issue:
   ```
   Pattern: <symptom>
   Clients affected: <list>
   Hypothesis: <one line>
   Recommended fix owner: Founding Engineer | Web Designer | other
   Severity: low | medium | high
   ```
4. Tag Founding Engineer on high-severity items.

## 10. Exit

- Comment `in_progress` work.
- `X-Paperclip-Run-Id` on mutating calls.
- Clean exit.
