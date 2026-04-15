# HEARTBEAT — Lead Magnet Creator

Daily cadence (86400s) + on mention + on approval. Friday heartbeat triggers the weekly ideation sweep.

## 0. Env

Required:
- `PAPERCLIP_*` standard
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID` (Composio connected to Tally + Beehiiv + Notion)
- `NOTION_CRM_DB_ID`, `NOTION_OUTREACH_LOG_DB_ID`
- `GOAL_ID`

Missing → comment, `blocked`, exit.

## 1. Identity + wake

```
GET /api/agents/me
```

Read wake vars.

## 2. Read state

- [`life/areas/magnet-pipeline.md`](life/areas/magnet-pipeline.md) — ranked candidate ideas.
- [`life/areas/magnet-library.md`](life/areas/magnet-library.md) — live + killed magnets.
- [`../head-of-marketing/life/areas/content-playbook.md`](../head-of-marketing/life/areas/content-playbook.md) — team direction + learnings.

## 3. Approvals

If `PAPERCLIP_APPROVAL_ID` — an idea was approved/rejected OR a pre-publish magnet review. Handle first.

## 4. HR loop

Scan CEO corrective comments citing the Auditor.

## 5. Assignments

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={your-id}&status=todo,in_progress,blocked
```

Typical assignments:
- `Build <magnet slug>` — approved idea, time to ship.
- `Iterate <magnet slug>` — live magnet underperforming; change hook/intro/flow.
- `Kill <magnet slug>` — remove CTAs, archive.
- `Ideate for <theme>` — one-off brainstorm.

## 6. Weekly ideation (Fridays only)

Run in this order:

1. **Read prospect questions** via Composio Notion: pull Outreach Log entries from the last 14 days where `Notes` contain questions (heuristic: contains "?"). Cluster into 3–5 common themes.
2. **Read Growth Analyst's content perf.** What hooks are landing in LinkedIn? What's in the newsletter's top-clicked sections?
3. **Web search** using `tool-belt/skills/web-search` for Nordic hospitality trends — labor costs, regulation, tech adoption, competitor moves.
4. **Brainstorm** using `obra/superpowers/brainstorming` skill. Generate 10 candidate magnet ideas (intentionally long list), then narrow.
5. **Rank** the top 3–5 using the scoring criteria in `magnet-pipeline.md` (ICP pull, Signal quality, Buildability, Uniqueness).
6. **Update** `life/areas/magnet-pipeline.md` — prepend this week's ranked ideas with evidence for each.
7. **Post DIGEST** on Head of Marketing's standing issue: "Week of YYYY-MM-DD ideation — top 3: <ids>. Proposed to approve: <id> for build." Wait for approval reply.

## 7. Build workflow — Tally magnet

Triggered by an `in_progress` subtask titled `Build <magnet slug>`.

1. Pull the approved idea from `magnet-pipeline.md`.
2. Design the Tally form via Composio:
   - Inputs (clearly labeled).
   - Conditional logic for calculations.
   - Output page(s) with the personalized result.
   - Email capture step (required field).
   - Redirect to a Notion public page for the detailed result / extras.
3. Create the Notion public page (content + download links).
4. Configure Tally → Beehiiv via Composio:
   - On submission, add email to Beehiiv with tag `magnet:<slug>`.
   - Trigger the Beehiiv nurture sequence for that tag.
5. Request Content Writer to draft the nurture sequence (3 emails over 10 days) via subtask — include magnet context and CTA direction.
6. Test end-to-end using a dummy email address.
7. Submit for Head of Marketing pre-publish review: share Tally URL, Notion URL, nurture sequence link.
8. On approval → go live.
9. Post a launch kit to subtasks for Content Writer (LinkedIn launch post) and LinkedIn Engagement (promote + drop references in relevant comment threads).
10. Append to `magnet-library.md` with live date, URL, initial metrics empty.

## 8. Build workflow — complex (Founding Engineer assist)

Triggered when a magnet needs custom code. Workflow:

1. Write the spec (inputs, outputs, logic, target load).
2. Create subtask under CTO's delegation chain (reassign via CTO):
   ```
   Title: Build magnet tool — <slug> (custom)
   Body: <spec>
   Deployment: Cloudflare Pages, akira-agent.com/tools/<slug>
   ```
3. Founding Engineer builds + deploys + returns URL.
4. Resume Tally workflow at step 3 (embed the custom tool, add Tally-only capture step).

## 9. Weekly measurement (Fridays, after ideation)

For each live magnet:
1. Pull Tally response count + Beehiiv subscriber count with magnet tag (via Composio).
2. Compute rolling 4w views, signups, conversion.
3. Update `magnet-library.md` with new numbers.
4. Flag any magnet below thresholds (10% conv OR <3 signups/week) to Head of Marketing: "Proposed kill: <slug>. Rolling 4w: <data>."

## 10. Exit

- Comment on `in_progress` work.
- `X-Paperclip-Run-Id` on mutating calls.
- Clean exit.
