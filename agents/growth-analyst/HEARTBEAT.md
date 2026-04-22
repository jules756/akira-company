# HEARTBEAT — Growth Analyst

Run weekly (Fridays) + on mention + on approval.

## 0. Env check

Required:

- `PAPERCLIP_*` (standard)
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID` — all read access routes through Composio.
- `NOTION_CRM_DB_ID`, `NOTION_OUTREACH_LOG_DB_ID`
- `PROSPECT_SHEET_ID`
- `GOAL_ID`

Missing → comment, `blocked`, exit.

## 1. Identity

```
GET /api/agents/me
```

Check wake vars. If mid-week mention, go to step 7 (QUERY RESPONSE mode). If Friday schedule, go step 2.

## 2. Read the schema

[`../crm-steward/knowledge/crm-schema.md`](../crm-steward/knowledge/crm-schema.md). Note current stage names and field names. All queries use the live schema, not hardcoded strings.

## 3. Pull the data

### 3a. Companies (CRM) snapshot

Query all rows with `Pipeline Status` ≠ null. Capture per row:
- Company Name, Pipeline Status, `createdTime`, last Outreach Log Date (join), Total MRR, Restaurant Type, Locations count.

### 3b. Outreach Log — last 28 days

All entries in the rolling 4-week window. Capture: Company, Channel, Outcome, Date, Notes length, Recontact On.

### 3c. Prospect Sheet — last 7 days

Read the Google Sheet. Capture: prospects added this week, tier distribution, first-touches scheduled/performed, First-Touch Agent breakdown.

### 3d. LinkedIn Engagement log — last 7 days

Read LinkedIn Engagement Agent's memory at `../linkedin-engagement/memory/` + `life/areas/engagement-patterns.md`. Count: actions/day, reply rate per action, profile views lift (if available via Composio LinkedIn analytics).

### 3e. Newsletter (Beehiiv) — last 7 days

Via Composio → Beehiiv API. Capture: net new subscribers this week, open rate + click rate of last issue, subscribers attributed per tag (`magnet:<slug>`) for attribution.

### 3f. Lead magnets (Tally + Beehiiv) — last 7 days

For each live magnet in `../lead-magnet-creator/life/areas/magnet-library.md`: Tally views + submissions this week, Beehiiv signups tagged to the magnet, rolling 4w conversion %.

## 4. Reconcile

- Prospect sheet: first-touches performed = X
- Outreach Log: entries with Date in last 7 days = Y
- If abs(X - Y) > 10% of X → log discrepancy to `life/areas/reconciliation-log.md` and investigate.
- Companies (CRM) count should ≈ cumulative first-touches (some lost to dedup). Spot-check.
- Beehiiv subscribers with magnet tags should ≈ Tally submissions for that magnet. Mismatch = broken integration; flag to Lead Magnet Creator.

If any discrepancy > 20%, flag to the owner (CRM Steward / Lead Magnet Creator / Head of Marketing) and do NOT publish this week's brief until resolved. Post a SIGNAL to Head of Sales.

## 5. Compute metrics

### Top of funnel (Sales-owned)

- Prospects added this week (with tier breakdown)
- First-touches performed this week
- First-touch → Lead conversion = rows created in CRM last 7d / first-touches last 7d
- First-touch success rate by Tier (tier-accuracy signal for Lead Researcher)

### Pipeline (Sales-owned)

- Count per stage (live)
- Time-in-stage (mean) per stage
- Stage-to-stage conversion rate, 4w rolling:
  - Lead → Discovery, Discovery → Proposal, Proposal → Active.

### Marketing — LinkedIn (Marketing-owned)

- Actions taken (comments + reactions) this week
- Reply rate per comment, rolling 4w
- Profile views / connection requests (if available via Composio LinkedIn analytics)
- Top-performing hook / angle / topic patterns (compare post stop-rates)

### Marketing — Newsletter (Beehiiv)

- Total subscribers (delta vs. last week)
- Open rate + click rate of the most recent issue
- Subscribers attributed per magnet tag this week

### Marketing — Lead magnets

- Per-magnet: views 7d, submissions 7d, conversion % 7d, rolling 4w conversion %
- Flag any magnet below threshold (4w conversion <10% OR <3 signups/week) for potential kill.
- Newsletter subscribers sourced from magnets / total new subscribers = magnet contribution %.

### Forecast

- Weighted pipeline value = Σ (Total Install Fee + 12 × MRR) × p(stage)
  - p(Lead) = 0.05
  - p(Discovery) = 0.20
  - p(Proposal) = 0.45
  - p(Active) = 1.00
- Expected next-90-day MRR = weighted pipeline × time-adjustment
- Gap to 500K SEK target

## 6. Stuck-lead detection

Per-lead flag:
- Discovery > 14 days with no Outreach Log in 7 days
- Proposal > 10 days with no Outreach Log in 5 days
- Any lead with no Outreach Log entry ever (violation of promotion rule) — flag to CRM Steward

## 7. Friday Board Brief

The brief now covers **both funnels** — top-of-funnel (Marketing-owned: LinkedIn, newsletter, magnets) and conversion (Sales-owned: pipeline, stages). Structure:

```
# Friday Board Brief — <date>

## Top of funnel — Sales outbound
- Prospects added this week (tiers)
- First-touches performed
- First-touch → Lead conversion
- Tier accuracy (T1 vs T3 reply rate)

## Marketing — LinkedIn
- 5×/day volume adherence
- Reply rate, trend
- Top hook / angle this week

## Marketing — Newsletter (Beehiiv)
- Net new subs, total subs, open%/click%
- Top magnet drivers

## Marketing — Magnets
- Per-magnet conversion
- Proposed kills (if any)

## Pipeline
- Stage counts + conversion
- Stuck leads (named)

## Forecast
- Weighted pipeline, gap to 500K SEK target

## Recommendations (3)
1. <metric → action → owner>
2. ...
3. ...
```

Create the Paperclip issue:

```
POST /api/companies/{companyId}/issues
{
  "title": "Friday Board Brief — <date>",
  "description": "<full brief body, see template in AGENTS.md>",
  "assigneeAgentId": "<ceo-id>",
  "parentId": "<any standing 'Weekly Reports' issue, create if missing>",
  "goalId": "<GOAL_ID>",
  "priority": "high",
  "status": "in_review"
}
```

Comment on the new issue tagging Head of Sales with a 3-line summary.

## 8. Mid-week QUERY RESPONSE mode

If woken by a mention, don't produce a full brief. Answer the specific question with:

- Numbers (with source + range)
- One-line interpretation
- Follow-up question back to the asker if anything's ambiguous

## 9. Weekly synthesis

Use `para-memory-files`. Store:
- `life/areas/metrics/YYYY-WW.md` — this week's full computation.
- `life/areas/trends.md` — rolling trends appended weekly.
- `./memory/YYYY-MM-DD.md` — daily log.

## 10. Exit

- Comment on `in_progress` work.
- `X-Paperclip-Run-Id` on mutating calls.
- Clean exit.
