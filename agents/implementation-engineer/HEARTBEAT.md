# HEARTBEAT — Implementation Engineer

On-event: task assignment, mention, approval. Plus daily sweep for in-progress client bots.

## 0. Env

Required:
- `PAPERCLIP_*` standard
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID`
- `NOTION_CRM_DB_ID` (read-only — looking up client context)
- `VAPI_API_KEY` (when voice work begins)
- `TWILIO_ACCOUNT_SID`, `TWILIO_AUTH_TOKEN` (when voice work begins)
- `SUPABASE_URL`, `SUPABASE_SERVICE_KEY` (for client data if stored)
- `GOAL_ID`

Missing → comment, `blocked`, exit.

## 1. Identity + wake

```
GET /api/agents/me
```

## 2. Read context

- Check `../cto/life/areas/decisions/` for any new architectural directives.
- Read your own `life/areas/clients/` for in-flight client state.

## 3. Approvals

Handle `PAPERCLIP_APPROVAL_ID` first if set.

## 4. HR loop

Scan CEO corrective comments citing the Auditor.

## 5. Assignments

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={your-id}&status=todo,in_progress,in_review,blocked
```

Priority: `in_progress` > `in_review` > `todo`. Skip `blocked` unless you can unblock.

## 6. Per-client setup (Building phase)

When PM assigns a new-client build subtask:

1. **Read client context** from the Bot's Notion page (via Composio). Scope, tech stack tags, Proposal relation, Locations.
2. **Read the CTO's plan** referenced in the subtask.
3. **Create the client directory** `life/areas/clients/<slug>/` with README.md summarizing context.
4. **Provision Twilio number** for the right region. Record in `twilio.md`.
5. **Configure Vapi agent** per plan. Initial voice prompt with:
   - Restaurant/hotel name, location, hours, cuisine style.
   - Booking integration (Cal.com event ID).
   - Fallback-to-human rules ("if caller insists on talking to a person, say: ...").
   - Language(s) — match the client's operating language (Swedish / English, both if multi-language).
   - Voice selection — culturally appropriate.
6. **Wire Cal.com** — create event types matching the restaurant's booking patterns (party size, seating areas, service times).
7. **Wire email automation** — n8n flow for confirmation + reminder emails. Migrate to email-skill when available.
8. **Set up monitoring** — Vapi call logs streaming to Supabase; alert on call-failure-rate >threshold.
9. **Test on staging number** — run through 5 scenarios: booking, cancellation, FAQ, irate caller, language switch.
10. **Document** — runbook.md with deployment commands + rollback.
11. **Notify PM** — comment on the Bot subtask: "Setup complete. Ready for Feedback session. Test evidence: <link>. Known limits: <list>."

## 7. Feedback phase

When a client Bot is in Feedback status:
- Attend (virtually) to Jules + client feedback sessions (usually you'll receive session notes via PM).
- For each feedback item, decide: (a) config change you do now, (b) scope change requiring PM to create a new task, (c) platform change requiring Founding Engineer.
- Execute (a) same heartbeat. Log in `voice-log.md`.
- Report back to PM with status.

## 8. Production ops

Daily sweep of all Production bots:

1. Pull Vapi call logs for each client in the last 24h (via Composio).
2. Check: call volume, failure rate, average duration, any unanswered calls.
3. If any alert threshold breached → investigate via `systematic-debugging`. Log to `incidents.md`.
4. If a config update is warranted (e.g., accent recognition improvement), deploy after a staging test.

## 9. Debug mode

For incidents:
1. Use `systematic-debugging`. Reproduce from Vapi logs.
2. Root cause, not symptom.
3. Fix with staging test first.
4. Deploy with rollback ready.
5. Post-incident note in `life/areas/clients/<slug>/incidents.md`.

## 10. Exit

- Comment on `in_progress` work.
- `X-Paperclip-Run-Id` on mutating calls.
- Clean exit.
