# HEARTBEAT — Lead Researcher

Daily cadence (86400s) + on mention.

## 0. Env

Required:
- `PAPERCLIP_*` standard
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID` (Composio connected to Apollo + Google Drive/Sheets + Notion read)
- `PROSPECT_SHEET_ID`
- `NOTION_CRM_DB_ID` (read-only use)
- `GOAL_ID`

Missing → comment, `blocked`, exit.

## 1. Identity + wake

```
GET /api/agents/me
```

Read wake vars.

## 2. Read the rules

- [`life/areas/icp-playbook.md`](life/areas/icp-playbook.md) — ICP definition + search templates + volume targets.
- [`../head-of-sales/life/areas/notion-protocol.md`](../head-of-sales/life/areas/notion-protocol.md) — you're read-only on Notion.
- `../crm-steward/life/areas/crm-schema.md` — for the CRM DB schema when checking duplicates.

## 3. HR loop

Scan CEO corrective comments citing the Auditor's rubric.

## 4. Assignments

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={your-id}&status=todo,in_progress,blocked
```

Typical tasks:
- `Run Apollo template A this week` (volume target by tier)
- `Rotate secondary source this week`
- `Deep-dive on <segment>` (ad-hoc from Head of Sales)

If no specific assignment but it's a weekday → run the daily sourcing routine (step 5).

## 5. Daily sourcing routine

**Ordering matters:** engagement-sourced prospects come first (demand-creation framework), Apollo sourcing after.

### 5a. Engagement sourcing (primary — always run first)

1. Read [`../linkedin-engagement/life/areas/engagement-log.md`](../linkedin-engagement/life/areas/engagement-log.md). Pull all entries with Tier 1 or Tier 2 from the last 24h.
2. **Apply exclusion filters** (see [`../head-of-marketing/life/areas/demand-creation-playbook.md`](../head-of-marketing/life/areas/demand-creation-playbook.md) "Exclusion filters"): skip if already in CRM, already in the sheet with First-Touch Date set, competitor, sub-ICP, `Not interested` in last 180 days, or on Head of Marketing's exclusion list.
3. For each non-excluded engager not already in the sheet AND not already in the Notion CRM:
   - Enrich via Composio (LinkedIn → company → website → Nordic match → Restaurant Type if applicable). Use Apollo/Clay-style data as a ranker at this stage, not a trigger (trigger was the engagement).
   - Add to sheet with `Source: LinkedIn-Engagement`, `Tier` from the log (or re-score if uncertain), notes referencing post URL + engagement type.
4. Any engager whose tier is ambiguous → run the full ICP verification and log the decision.

If you process ≥10 engagement-sourced prospects in a single heartbeat, stop there for the day. Those are the hottest leads; don't dilute with Apollo-cold volume.

### 5b. Apollo sourcing (secondary — after engagement is caught up)

1. **Check the sheet.** Read the last 7 days of adds via Composio. How many per tier vs. the weekly target? What's the pace?
2. **Pick today's search template** from `icp-playbook.md`. Rotate across templates across the week — don't run Template A every day.
3. **Run the Apollo search** via Composio. Limit to ~30–50 raw results per run to keep compute bounded.
4. **Fetch existing sheet rows** (just the `Company Name` + `Website` columns, plus dedupe-helpful fields) — cache in `./memory/state/sheet-dedupe.txt`.
5. **Fetch existing Notion CRM Company Name + Website** via Composio read — cache in `./memory/state/crm-dedupe.txt`. Respect rate limits; skip this step if already fetched within the last hour.
6. **Process each Apollo result:**
   - **ICP check** — does the raw result match `icp-playbook.md` criteria? If not, discard.
   - **Dedupe check** — is this Company already in the sheet? In the CRM? If yes to either, discard.
   - **Verify on website** — pull the company website via `tool-belt/skills/web-search`. Confirm: actually hospitality? Nordic location? Multi-location (counts matter for Tier)?
   - **Enrich** — fill as many of the sheet columns as possible from Apollo + website + LinkedIn signals. Leave null what you can't verify.
   - **Score tier** using `icp-playbook.md` rules.
   - **Append to sheet** via Composio with today's date + `Source=Apollo` (or other).
7. Cap at 20 adds per heartbeat to stay budget-conscious.

## 6. Secondary-source sweep (once per week, ideally Wednesday)

Run a non-Apollo source once per week for diversity:

- **LinkedIn public search** via Composio — search for Nordic hospitality decision-makers by title + region.
- **Swedish / Norwegian directories** via `tool-belt/skills/web-search` — e.g., local restaurant association directories.
- **Signal scrape** — LinkedIn posts about no-shows / admin cost / hospitality labor in last 60 days; commenters and authors are warm Tier 2 candidates.

Append findings to the sheet with `Source` reflecting the origin.

## 7. Weekly ICP feedback (Fridays)

- Compute adds per tier this week.
- Pull Growth Analyst's tier-accuracy signal from Friday Board Brief (when available — after week 4 of running).
- If Tier 1 isn't meaningfully out-converting Tier 3, draft a proposal to Head of Sales with suggested ICP rule changes + evidence.
- Post DIGEST on Head of Sales' "CRM Daily" issue: "Sourced this week: 18 T1, 22 T2, 9 T3. Dedupe rejections: 11 vs sheet, 3 vs CRM. Proposed ICP adjustment: <if any>."

## 8. Fact extraction

Use `para-memory-files`. Store patterns about which Apollo filters produce the best tier 1 results, which websites signal multi-location clearly, which segments have the most decision-maker visibility, etc. → `life/areas/sourcing-patterns.md`.

## 9. Exit

- Comment on `in_progress` work.
- `X-Paperclip-Run-Id` on mutating calls (Paperclip side — note: you barely do mutations on Paperclip).
- Clean exit.
