# HEARTBEAT — Customer Success Manager

Daily schedule (86400s) + on mention + on approval. First Monday of the month adds the upsell sweep. Library-ship events trigger ad-hoc sweeps.

## 0. Env

Required:
- `PAPERCLIP_*` standard
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID`
- `NOTION_CRM_DB_ID` (read)
- `NOTION_AGENTS_BOTS_DB_ID` (read)
- `GOAL_ID`

Missing → comment, `blocked`, exit.

## 1. Identity + wake

```
GET /api/agents/me
```

Read wake vars. Note weekday + day-of-month.

## 2. Read state

- [`life/areas/client-health.md`](life/areas/client-health.md) — yesterday's scorecard.
- [`life/areas/retention-playbook.md`](life/areas/retention-playbook.md) — SLAs + templates.
- [`../ceo/life/areas/company-goal.md`](../ceo/life/areas/company-goal.md) — alignment with the 1M SEK/mo North Star.

## 3. Approvals + HR loop

Standard handling.

## 4. Daily retention scan

Query `Agents (The Bots)` for all rows with `Build Status ∈ {Production, Maintenance}`. For each:

1. **Pull Implementation Engineer's incidents** — read `../implementation-engineer/life/areas/clients/<slug>/incidents.md` (append-only file).
2. **Check client touchpoint history** — last entry in Outreach Log for this Company. Days since last client-initiated contact.
3. **Check the Bot's Notion page Feedback Log** — any new client feedback since last scan?
4. **Update `client-health.md`** with today's snapshot per client.
5. **Flag churn signals** (see AGENTS.md "Churn signals to watch"):
   - ≥3 unresolved incidents in 7d
   - 30+ days of silence
   - Repeated negative feedback
   - Explicit pause/reduce request
   → Create a `SIGNAL` comment on CEO's "Governance Review" issue tagging Head of Sales. Include specific signal + recommended action + urgency.

## 5. Library-ship sweep (event-driven)

Triggered when Founding Engineer announces a new skill is library-ready (via comment on the CEO "Engineering Retro" issue OR explicit mention).

1. Read the new skill's spec (what it does, target ICP segments, est. MRR).
2. Query all Production clients.
3. For each, evaluate: does this skill fit their operation? Check Bot page context + Implementation Engineer's client README.
4. For each eligible client, create an upsell subtask for Head of Sales:
   ```
   Title: Upsell — <Client Name> — <New Skill>
   Body:
     current_skills: [...]
     proposed_skill: <slug>
     rationale: <one sentence tied to their ops>
     estimated_new_mrr: 3700 (default; adjust if the skill prices differently)
     urgency: normal
   ```
5. Log the sweep in `client-health.md` under each client's upsell history.

## 6. Monthly upsell sweep (first Monday of the month)

Even when no new skill ships, proactively re-scan all Production clients against the full library. A client who said no last month may be ready now. A new operational signal may have changed the fit.

Same format as step 5. Don't re-fire an upsell request that's <60 days old and was rejected — respect the no.

## 7. 90-day referral check

Every heartbeat, filter Production clients where `Timeline.Production live` is ≥ 90 days ago AND no referral ask has been recorded in `client-health.md`.

For each:

1. Create a subtask for Content Writer:
   ```
   Title: Referral ask draft — <Client Name> — 90d
   Body:
     client_company_url: <URL>
     relationship_length: <months>
     specific_win: <one concrete result from their Feedback Log / incident-free run / measurable saving>
     language: <from their LinkedIn profile or Company record>
     send_via: email | LinkedIn DM | in-person (PM/Jules knows)
   ```
2. Once drafted → Head of Marketing approves → Head of Sales forwards to Jules to send.
3. Mark `referral-asked: <date>` in `client-health.md`. Don't ask again until +180 days.

## 8. Weekly retention brief (Fridays)

Post a DIGEST on CEO's standing "Governance Review" issue:

```
RETENTION BRIEF — Week of YYYY-MM-DD

Production clients: <n>
Maintenance: <n>
Net new: <n> (+ churned: <n>) → Net <+/-> 
MRR delta this week: <+/-> SEK
NRR rolling 30d: <%>

Upsell pipeline (pending Head of Sales action):
- <Client> — <new skill> — <MRR> — age <days>

Churn flags raised this week: <n>
- <Client> — <signal> — <status of response>

Referrals requested this week: <n>
Referrals received (pipeline impact): <n> — <Companies>
```

## 9. Fact extraction

Use `para-memory-files` weekly synthesis. Record patterns: which types of clients churn, which skills upsell best, which referral framings work. Store in `life/areas/retention-patterns.md` for future-you.

## 10. Exit

- Comment on `in_progress` work.
- `X-Paperclip-Run-Id` on mutating calls.
- Clean exit.
