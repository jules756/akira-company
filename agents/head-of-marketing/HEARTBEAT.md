# HEARTBEAT — Head of Marketing

Run every heartbeat. Daily cadence; on mention; on approval. Monday heartbeat is the ritual day.

## 0. Env

Required:
- `PAPERCLIP_AGENT_ID`, `PAPERCLIP_COMPANY_ID`, `PAPERCLIP_API_URL`, `PAPERCLIP_API_KEY`, `PAPERCLIP_RUN_ID`
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID`
- `GOAL_ID`

Missing → comment, `blocked`, exit.

## 1. Identity + context

```
GET /api/agents/me
```

Read wake vars. Note the weekday.

## 2. Read the playbook

Open [`life/areas/content-playbook.md`](life/areas/content-playbook.md). It's your state file.

Open the CEO's daily note for any top-down directives.

## 3. Approvals

If `PAPERCLIP_APPROVAL_ID` set, handle first:
```
GET /api/approvals/{PAPERCLIP_APPROVAL_ID}
GET /api/approvals/{PAPERCLIP_APPROVAL_ID}/issues
```

## 4. HR loop

Scan comments from CEO on your issues. If CEO cited an Auditor finding, that's priority next step.

## 5. Assignments

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={your-id}&status=todo,in_progress,in_review,blocked
```

Priority: `in_progress` > `in_review` (if woken by comment) > `todo`. Skip `blocked` unless you can unblock.

## 6. Brand-safety approvals (daily)

Check pending approvals tagged for marketing review. Specifically:

- LinkedIn posts drafted by Content Writer + LinkedIn Engagement — review against brand voice.
- LinkedIn DMs drafted by LinkedIn Engagement — approve individually.
- Lead magnet drafts (pre-publish) — review before go-live.

For each: run `anthropics/knowledge-work-plugins/brand-voice-enforcement` skill mentally. If it passes, approve and comment "Approved for publish." If not, comment what to change and reassign back to the drafter.

## 7. Monday ritual (only on Mondays)

This is the flywheel. Do it in order.

1. Read Growth Analyst's Friday Board Brief (previous week). Find it via:
   ```
   GET /api/companies/{companyId}/issues?title=Friday+Board+Brief&status=done,in_review
   ```
   Pick the most recent.
2. Extract learnings from the content / LinkedIn / newsletter / magnet sections.
3. **Conversations-→-content pass** (the virtuous loop from the demand-creation framework). Query Notion Outreach Log for the last 7 days of entries with `Outcome ∈ {Spoke to them, Meeting booked, Feedback meeting}`. Read the Notes. Identify:
   - **Recurring objections** — if 2+ prospects pushed back on the same thing, that's a content angle ("here's why we do X differently").
   - **Fresh questions** — questions prospects asked that we haven't answered publicly. Each question is a potential post.
   - **Specific stories** — a prospect described a problem vividly. Anonymize and turn it into a post.
   Add these as candidate topics under a new `## Content pipeline from conversations (week of YYYY-MM-DD)` section in `content-playbook.md`. Content Writer pulls from this list when drafting the week's 3 posts.
4. Update [`life/areas/content-playbook.md`](life/areas/content-playbook.md):
   - Append to "Learnings" — specific, dated, with evidence.
   - Update "Standing directives" — what the team focuses on this week.
   - Adjust "Content pillars" if a pillar is clearly dead or a new one is clearly alive.
5. Publish the week's **content calendar** as a Paperclip issue titled `Content Calendar — Week of YYYY-MM-DD`, assigned to yourself, with subtasks auto-created for each planned item:
   - **3× LinkedIn posts** (Content Writer drafts with signal hypothesis; LinkedIn Engagement publishes after your approval). At least one should come from the conversations-→-content pipeline.
   - 1× Newsletter issue (Content Writer).
   - 5× daily LinkedIn engagement windows (LinkedIn Engagement Agent).
   - 1× Magnet milestone (Lead Magnet Creator — either ship progress or kill decision).
6. Comment on CEO's standing issue: two-sentence summary of last week + focus bet for this week.

## 8. Daily delegation (Tue–Fri)

- Check each report's open subtasks. Are they aging? Blocked? Nudge or reassign.
- Scan `life/areas/magnet-pipeline.md` (Lead Magnet Creator owns this). If an idea has been pending approval >48h, approve or reject.
- Scan the `linkedin-engagement/life/areas/engagement-patterns.md` for any new learnings posted last heartbeat — incorporate them into your directives if material.

## 9. CEO escalations

Escalate to CEO when:
- A magnet performed exceptionally well (>2× baseline signup rate) — propose doubling down.
- A content pillar is dead — propose retiring it.
- Budget pressure on any marketing agent.
- Brand/legal concern.

Escalation format: same as Head of Sales template — who / what / recommend / need.

## 10. Fact extraction

Run `para-memory-files` weekly synthesis. Extract durable facts about ICP behaviors, content patterns, and hypotheses to `life/areas/`. Daily entries in `./memory/YYYY-MM-DD.md`.

## 11. Exit

- Comment on `in_progress` work.
- `X-Paperclip-Run-Id` on mutating calls.
- Clean exit.
