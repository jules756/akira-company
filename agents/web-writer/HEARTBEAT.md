# HEARTBEAT — Web Writer

Daily (86400s) + on event (brief assignment, case-study trigger). Run once per day to clear the day's brief queue.

## 0. Env

Required:
- `PAPERCLIP_*` standard
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID`
- `NOTION_AGENTS_BOTS_DB_ID` (read for case-study source data)
- `NOTION_OUTREACH_LOG_DB_ID` (read for case-study highlights)
- `GOAL_ID`

Missing → comment, `blocked`, exit.

## 1. Identity + wake

```
GET /api/agents/me
```

## 2. Read state

- [`life/areas/editorial-calendar.md`](life/areas/editorial-calendar.md) — what's shipped + queued.
- [`../seo-aeo-specialist/life/areas/seo-playbook.md`](../seo-aeo-specialist/life/areas/seo-playbook.md) — article shape + brand rules.
- [`../head-of-marketing/life/areas/content-playbook.md`](../head-of-marketing/life/areas/content-playbook.md) — brand voice direction.

## 3. Approvals + HR loop

Standard handling.

## 4. Pull today's queue

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={your-id}&status=todo,in_progress,blocked
```

Filter by:
- `[Article brief]` titled subtasks from SEO + AEO Specialist.
- `[Case study draft]` titled subtasks from Customer Success Manager.

Order: in_progress first, then todo (oldest first). Target: clear 1 article per business day (driven by the `daily-blog-publish` routine).

## 5. Brief validation (every pulled brief)

Before drafting, verify the brief has:
- `target_query`
- `aeo_answer_block` text
- `outline` with at least 3 H2s
- `internal_links` list
- `schema` recommendation
- `word_count_target`

If any missing → comment on the subtask: `Brief incomplete: missing <fields>. Cannot proceed.` Reassign to SEO + AEO Specialist with `status=blocked`. Move to next brief.

## 6. Draft mode

For each valid brief:

1. **Open the brief.** Read all fields.
2. **Research one specific data point** (via `tool-belt/skills/web-search` if needed) to anchor the article — Nordic hospitality stat, recent news, regional benchmark.
3. **Draft the article in this exact shape**:
   - Hook (50-75 words) — specific number or story.
   - AEO answer block (50-100 words, verbatim from brief or close).
   - Problem (200-300 words).
   - Solution (400-600 words) — name Akira's capabilities explicitly.
   - How it works (200-300 words) — Vapi / Twilio / Cal.com / email-skill mentioned where relevant.
   - ROI section (if commercial intent).
   - CTA matching brief's `cta` field.
4. **Insert internal links** per brief's `internal_links` list, with descriptive anchors.
5. **Brand-voice check** — scan for: em-dashes, "delve," "robust," "seamless," "we're thrilled," compound sentences. Rewrite hits.
6. **Word count check** — within brief's `word_count_target` ± 10%.
7. **Format** — markdown. H1 = article title. H2s = section headers. Code blocks for any technical examples (rare in marketing content).
8. **Submit** — paste full markdown as a comment on the brief subtask. Mark `status=in_review`. Reassign to SEO + AEO Specialist.

## 7. Case-study mode

For each `Case study draft` subtask:

1. **Read source data** via Composio:
   - Notion `Agents (The Bots)` page for the client (Bot documentation).
   - Notion `Companies (CRM)` row.
   - Outreach Log entries with Outcome `Spoke to them` / `Feedback meeting`.
   - Implementation Engineer's `clients/<slug>/voice-log.md` + `incidents.md` (read via the agent's life/areas).
2. **Request a quote** — if CS hasn't already attached one:
   ```
   Comment on subtask:
   "Quote needed for case study. Suggested ask for Jules to send to client:
   'Could I get one or two sentences from you on what's changed since Akira went live?
   Quote will appear in our case study (anonymized if you prefer).'"
   ```
3. **Wait 24h for quote** OR proceed with paraphrased outcome if client declined.
4. **Draft case study** in the structure from AGENTS.md:
   - Client overview (anonymized if requested).
   - Problem before Akira.
   - What we built (which capabilities).
   - Result with numbers.
   - Lesson for similar operators.
5. **Submit** to SEO + AEO Specialist for review, who forwards to Web Operations for publish at `/case-studies/<slug>`.

## 8. Editorial calendar update

After every shipped article (when Web Operations confirms publish):
- Append to `editorial-calendar.md` — date, slug, target query, internal links, current status.

## 9. Weekly synthesis (Fridays)

- Count articles shipped this week vs. target (5-10).
- Review SEO + AEO Specialist's Monday observations from `seo-playbook.md` — any patterns to apply.
- Note any briefs that produced disproportionately strong drafts (capture pattern in `editorial-calendar.md`).

## 10. Exit

- Comment on `in_progress` work.
- `X-Paperclip-Run-Id` on mutating calls.
- Clean exit.
