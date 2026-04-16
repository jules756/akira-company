# HEARTBEAT — SEO + AEO Specialist

Daily (86400s) + on mention. Monday wake adds the strategy + citation-tracking ritual.

## 0. Env

Required:
- `PAPERCLIP_*` standard
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID`
- `SEARCH_CONSOLE_PROPERTY_URL` — akira-agent.com Search Console property URL
- `GOAL_ID`

Missing → comment, `blocked`, exit.

## 1. Identity + wake

```
GET /api/agents/me
```

Read wake vars. Note weekday.

## 2. Read state

- [`life/areas/seo-playbook.md`](life/areas/seo-playbook.md) — methodology.
- [`life/areas/keyword-roadmap.md`](life/areas/keyword-roadmap.md) — current 4+ weeks of briefs.
- [`life/areas/aeo-citation-log.md`](life/areas/aeo-citation-log.md) — last week's findings.
- [`../head-of-marketing/life/areas/demand-creation-playbook.md`](../head-of-marketing/life/areas/demand-creation-playbook.md) — meta direction.
- [`../ceo/life/areas/company-goal.md`](../ceo/life/areas/company-goal.md).

## 3. Approvals + HR loop

Standard handling.

## 4. Assignments

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={your-id}&status=todo,in_progress,blocked
```

Typical types:
- `Brief: <article slug>` — produce a brief for Web Writer.
- `Audit: <URL>` — SEO + AEO audit on an existing page.
- `Backlink target: <site>` — research + propose outreach to Content Writer.
- `Schema spec: <page type>` — produce structured-data spec for Web Operations.

## 5. Daily — keep the brief queue full

1. Count unprocessed briefs in `keyword-roadmap.md` (briefs Web Writer hasn't pulled yet).
2. If <8 briefs ahead → spend this heartbeat producing 2-3 new briefs from the next-priority cluster.
3. Use `coreyhaines31/marketingskills/seo-audit` + `ai-seo` + `resciencelab/opc-skills/seo-geo` skills to inform brief content.

## 6. Brief production (per brief)

For each new brief:

1. **Pick the target query** from `keyword-roadmap.md` next slot.
2. **Validate intent** via web-search: who currently ranks? What's the intent? What's the AEO answer-block opportunity?
3. **Outline** — 4-7 H2 sections that satisfy the query. Each H2 has a key point.
4. **Internal links** — pick 2-3 existing akira-agent.com URLs to link from.
5. **External sources** — 1-2 Nordic-hospitality-authoritative sources to cite (e.g., Hotell- och restaurangfacket reports, regional restaurant association data).
6. **AEO answer block** — write a 2-3 sentence direct answer to the target_query that LLMs can lift.
7. **Schema spec** — Article / FAQ / HowTo + properties.
8. **Submit** as a Paperclip subtask to Web Writer:
   ```
   Title: Article brief — <slug>
   Body (YAML): <full brief per AGENTS.md format>
   ```

## 7. Monday — strategy + citation tracking

This is the weekly ritual. Skip on other days.

### 7a. AI citation check (~30 min equivalent)

Use `composio` to query each LLM via web-equivalent (or via direct API if wired):

For each query in `aeo-citation-log.md` "Tracked queries":
1. Ask the LLM: `<query>` (e.g., "How do Nordic restaurants automate reservations?")
2. Read the answer. Does it cite akira-agent.com?
3. Log: `<query> | <LLM> | YYYY-MM-DD | cited: yes|no | excerpt: "..."`

Add 2-3 new tracked queries each week.

### 7b. Search Console review

Pull last 7 days from Search Console (via Composio):
- Top 10 ranking queries by impressions.
- Top 10 by clicks.
- New ranking pages (CTR > 1%).
- Pages losing rankings (down >20%).

Append to `seo-playbook.md` "Weekly observations".

### 7c. Roadmap rebalance

Based on this week's citation gaps + ranking wins/losses:
- Add 3-5 new briefs to `keyword-roadmap.md`.
- Promote any cluster that's overperforming.
- Demote/kill any cluster with 4+ weeks of zero traction.

### 7d. Monday DIGEST

Post to Head of Marketing's "Marketing Weekly" issue:
```
SEO + AEO — week of YYYY-MM-DD

Citations gained: <n> (<LLMs>)
Citations lost: <n>
New ranking pages: <n>
Lost rankings: <n>

Top opportunity: <one-line>
Pivot proposal: <if any>

Briefs queued for Web Writer: <count> (covers next <weeks>)
```

## 8. Audit mode (on request)

When asked to audit a specific page:

1. Pull the page via Composio + web-search.
2. Run `seo-audit` skill mentally: title, meta, H1, content depth, internal links, schema, page speed (estimate from CWV).
3. Run `ai-seo` skill: AEO answer block present? Citable structure?
4. Output: list of fixes, prioritized.

## 9. Exit

- Comment on `in_progress` work.
- `X-Paperclip-Run-Id` on mutating calls.
- Clean exit.
