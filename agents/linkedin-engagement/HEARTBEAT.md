# HEARTBEAT — LinkedIn Engagement Agent

Run 5× per day on schedule + on mention + on approval.

## 0. Env

Required:
- `PAPERCLIP_AGENT_ID`, `PAPERCLIP_COMPANY_ID`, `PAPERCLIP_API_URL`, `PAPERCLIP_API_KEY`, `PAPERCLIP_RUN_ID`
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID` (Composio must be connected to LinkedIn for the entity)
- `NOTION_CRM_DB_ID`, `NOTION_OUTREACH_LOG_DB_ID`
- `GOAL_ID`

Missing → comment, `blocked`, exit.

## 1. Identity + wake

```
GET /api/agents/me
```

Read `PAPERCLIP_WAKE_REASON`. If mention/approval → handle that path first; if schedule → full engagement window.

## 2. Read the rules

- Open [`knowledge/engagement-patterns.md`](knowledge/engagement-patterns.md) (your allow-list, block-list, language rules, learned what-works).
- Open [`../head-of-marketing/life/areas/content-playbook.md`](../head-of-marketing/life/areas/content-playbook.md) for this week's directives and content pillars.

## 3. Approvals

If `PAPERCLIP_APPROVAL_ID` — a post/DM/connection-request draft has been approved or rejected. Read:
```
GET /api/approvals/{PAPERCLIP_APPROVAL_ID}
```
If approved → publish it. If rejected → mark the linked draft issue `cancelled` with a comment quoting the reviewer's reason.

## 4. Handle assignments

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={your-id}&status=todo,in_progress,blocked
```

Work through `in_progress` first. Typical task types:
- `Engage with <post URL>` — go to the post, comment or react per rules.
- `Publish <approved post draft>` — publish now or schedule.
- `Send DM to <prospect> — draft attached` — draft and submit for approval.

## 4a. Engagement-capture sweep (demand-creation loop)

Pull the last 72h of Jules's own LinkedIn posts and their engagements via Composio. For each engagement (like, comment, share) that's new since last heartbeat:

1. **Log** to `knowledge/engagement-log.md`:
   ```
   | YYYY-MM-DD HH:MM | <post URL> | <engager profile URL> | <type: like|comment|share> | <comment text if comment> | Tier: <1|2|3|unknown> |
   ```
2. **Exclusion filter check** (see [`../head-of-marketing/life/areas/demand-creation-playbook.md`](../head-of-marketing/life/areas/demand-creation-playbook.md) "Exclusion filters"):
   - Already in Notion CRM (check via Composio before flagging).
   - Already in the prospect Google Sheet with `First-Touch Date` set.
   - Marked competitor.
   - Below minimum ICP size.
   - Latest Outreach Log `Outcome = Not interested` in last 180 days.
   - In Head of Marketing's exclusion-list.
   If any match → log with `Tier: excluded — <reason>` and do NOT flag for reactivation. Proceed to next engager.
3. **Score ICP tier** using the criteria in [`../lead-researcher/life/areas/icp-playbook.md`](../lead-researcher/life/areas/icp-playbook.md). Profile check: is this person a Nordic hospitality operator / multi-location / decision-maker? Apollo / Clay-style data (headcount, hiring, growing signals) is used here **only as a ranker**, not as a reason to flag.
4. **If Tier 1 →** two parallel subtasks:
   - To **Lead Researcher**: `Ensure engager in sheet — <engager name> (Tier 1)` — include profile URL, engagement context.
   - To **Follow-Up Manager**: `24h reactivation — <engager name> on <post URL>` — include engagement details, post topic.
5. **If Tier 2 →** log in engagement-log, flag weekly to Growth Analyst (for content calibration signal), don't trigger reactivation.
6. **If Tier 3 / unknown →** log only.

This is the core demand-creation capture step. Miss it and the whole Cold-to-Cash model breaks.

## 5. Inbound DM sweep

Via Composio, pull any new LinkedIn DMs since last heartbeat marker (`./memory/state/last-linkedin-dm-check.txt`).

For each DM:
1. **Match sender** to Notion CRM by LinkedIn profile URL (if Company has Website containing the profile) OR by name fuzzy match against Contact name.
2. **If matched** → request outreach-log entry via Steward:
   ```
   Title: [steward-write] outreach-log — <Company> LinkedIn DM reply
   Body: company_url, channel=DM, date, outcome, notes "Via LinkedIn DM. <summary>", recontact_on
   ```
3. **If not matched and looks ICP** (hospitality, Nordic, multi-location signals) → request `company-first-touch` via Steward:
   ```
   Title: [steward-write] company-first-touch — <Company Name> from LinkedIn
   Body: company_name, contact_name, contact_email=null, phone=null, website=<LI page>, restaurant_type, initial_channel=DM, initial_touch_notes
   ```
4. **If not ICP** → archive, log to `life/areas/non-icp-dms.md`.
5. **Hot reply** (demo intent, pricing question, multi-venue) → post `SIGNAL` on Head of Sales' "CRM Daily" issue.

Update `last-linkedin-dm-check.txt`.

## 6. Feed engagement window (primary scheduled work)

Via Composio, pull the latest ~50 posts in Jules' feed. Filter to ICP posts using `engagement-patterns.md` allow-list. Also filter for ICP *authors* already on the priority list (Head of Sales' `life/areas/leads/` and Lead Researcher's sheet).

Pick up to 5 actions this window. For each candidate post:

1. **Language gate** — is the post in Swedish / English / French? If not, skip.
2. **Topic gate** — is it on the allow-list? If not, and it's still from a priority ICP author → draft a comment and submit for approval. Else skip.
3. **Action type** — comment (preferred) or react (if comment would be low-value).
4. **Compose the comment** — 1–3 sentences, one idea, specific. Apply `viral-hook-creator` and `brand-voice-enforcement` skills mentally.
5. **Check the four auto-publish conditions:**
   - Language in {Swedish, English, French}.
   - Topic allow-listed.
   - Brand-voice passes.
   - Length 1–3 sentences.
   All four pass → **auto-publish** via Composio.
   Any fail → draft as a Paperclip issue, assigned to Head of Marketing for approval.
6. **Log** every action (auto or drafted) in `./memory/YYYY-MM-DD.md` with a timestamp and the post URL.

Cap: 5 actions per window. 1 window missed occasionally is fine; more than 2/day missed = flag to Head of Marketing.

## 7. Outbound DM window (weekly allowance)

Check `./memory/state/dms-this-week.txt`. If <10 sent this rolling 7d window AND there's an approved DM draft awaiting publish → send it via Composio. Update the counter.

If Head of Marketing creates a DM-draft subtask and the recipient hasn't been publicly engaged with yet → reject the task with a comment ("engage publicly first; then we can DM") and reassign back.

## 8. Jules' own posts

If today's content calendar has a post scheduled and it's approved, publish at the scheduled time via Composio. Post-publish → append to `./memory/YYYY-MM-DD.md` the post URL + initial metrics poll plan (24h, 72h, 7d).

## 9. Weekly synthesis (Fridays)

- Pull your last 7 days of actions.
- Group by action type / language / topic. Identify patterns: what got replies, what got profile views, what landed zero.
- Append findings to `life/areas/engagement-patterns.md` → "What works" / "What doesn't".
- Post a DIGEST to Head of Marketing.

## 10. Exit

- Comment on `in_progress` work.
- `X-Paperclip-Run-Id` on mutating calls.
- Clean exit.
