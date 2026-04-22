# HEARTBEAT — Web Operations

Daily (86400s) + on event (deploy request, monitoring alert). Friday wake adds the weekly performance review.

## 0. Env

Required:
- `PAPERCLIP_*` standard
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID`
- `VERCEL_TOKEN`, `VERCEL_TEAM_ID`, `VERCEL_PROJECT_ID_WEBSITE`
- `WEBSITE_REPO_URL` — `https://github.com/jules756/akira-agent-website`
- `WEBSITE_URL` — `https://akira-agent.com`
- `GA4_MEASUREMENT_ID`, `GTM_CONTAINER_ID`
- `SEARCH_CONSOLE_PROPERTY_URL`
- `GITHUB_TOKEN`
- `GOAL_ID`

Missing → comment, `blocked`, exit.

## 1. Identity + wake

```
GET /api/agents/me
```

Note weekday.

## 2. Read state

- [`knowledge/deployment-playbook.md`](knowledge/deployment-playbook.md) — your SOPs.
- [`knowledge/analytics-tracking-spec.md`](knowledge/analytics-tracking-spec.md) — current event taxonomy.
- Last Vercel deploy log (via Composio).

## 3. Approvals + HR loop

Standard handling.

## 4. Pull queue

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={your-id}&status=todo,in_progress,blocked
```

Sort by `createdTime` ascending — oldest publish requests first. Web Writer publishes don't sit.

## 5. Per-request workflow

### 5a. Article publish

Triggered by SEO + AEO Specialist after they review Web Writer's draft.

1. **Pull** the markdown + brief metadata from the subtask.
2. **Add** to website repo's `content/blog/` directory (Next.js MDX or Sanity push).
3. **Implement schema** (JSON-LD `<script>` block) per the brief.
4. **Add OG metadata** from Web Designer's batch.
5. **Insert internal links** per the brief's `internal_links`.
6. **Build locally / preview** — confirm no Next.js errors.
7. **Push** to `main` — Vercel auto-deploys.
8. **Wait** for deploy success (poll Vercel API via Composio).
9. **Smoke test** the live URL via `vercel-labs/agent-browser`:
   - HTTP 200.
   - Schema parses correctly.
   - OG metadata present.
   - Internal links resolve.
   - GA4 page_view fires.
10. **Submit** to Search Console for indexing (Composio Google integration).
11. **Confirm** in subtask comment with live URL + verification summary. Reassign back to SEO + AEO Specialist for `done`.

### 5b. Landing page

Triggered by Web Designer or Lead Magnet Creator.

1. Pull v0 component code + copy + assets + form spec from the handoff.
2. Implement Next.js page at the target URL.
3. Embed Tally form (or build React form for custom).
4. Add GTM events: `lp_view`, `lp_form_start`, `lp_form_submit_success`, `lp_outbound_click`.
5. Build, preview, deploy.
6. Smoke test: page loads, form submits land in Tally, success event fires in GA4 real-time.
7. Confirm with live URL + tracking proof to requester.

### 5c. Schema / tagging change

Triggered by SEO + AEO Specialist (schema) or anyone needing event tagging.

1. Implement change.
2. Validate (Rich Results Test for schema; GTM Tag Assistant for events).
3. Deploy.
4. Document in `analytics-tracking-spec.md` (for events) or in the affected page's metadata.
5. Confirm to requester.

## 6. Monitoring scan (Mon / Wed / Fri — every 2–3 days)

Driven by the `web-ops-monitoring-mwf` routine. On non-routine days, skip this step (focus on deploy queue).

When the routine fires:
- **Vercel deploy log** — any failed builds in last 24h?
- **GA4 real-time** — traffic anomaly (drop or spike >50% vs. baseline)?
- **Search Console errors** — new coverage issues?
- **Magnet form submissions** — Tally → Beehiiv pipeline healthy?

If any anomaly → investigate via `systematic-debugging` skill, file as `SIGNAL` to Head of Marketing if material.

## 7. Friday — weekly performance review

Skip on other days.

1. **Search Console deep scan**:
   - Top 10 ranking queries by impressions / clicks (last 7 days).
   - New ranking pages.
   - Pages losing rankings.
   - Coverage / indexing issues.
2. **Core Web Vitals report** (Vercel Analytics):
   - LCP / CLS / INP per page.
   - Pages with regressions.
3. **GA4 funnel review**:
   - Magnet signup conversion rates per magnet.
   - Blog article → service page click-through.
   - Contact form / Cal.com booking submission rate.
4. **Tag-coverage audit** — sample 5 pages, verify GA4 page_view + custom events fire correctly.
5. **Post DIGEST** to Head of Marketing's `Web Performance Daily` standing issue:

```
WEB OPS — week of YYYY-MM-DD

Deploys this week: <n> (<failed>)
Articles published: <n>
New pages live: <n>

Top organic page (last 7d): <URL> — <clicks>
New ranking pages: <n>
Lost rankings: <n>

CWV regressions: <list>
Magnet conversion rates: <per magnet>

Action items:
- <flagged issue + owner>
```

## 8. Founding Engineer escalation

When you hit a code-level blocker (custom React component beyond v0, backend integration, complex Next.js feature), escalate same heartbeat:

```
POST /api/companies/{companyId}/issues
{
  "title": "Platform request — <specific>",
  "description": "<spec: expected behavior, current behavior, what was tried>",
  "assigneeAgentId": "<founding-engineer-agent-id>",
  "parentId": "<current task>",
  "goalId": "<GOAL_ID>",
  "priority": "...",
  "metadata": { "billingCode": "eng" }
}
```

## 9. Exit

- Comment on `in_progress` work.
- `X-Paperclip-Run-Id` on mutating calls.
- Clean exit.
