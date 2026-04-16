# HEARTBEAT — SEO Trend Researcher

Daily (86400s) + on mention. Driven by `daily-seo-trend-scan` routine. Monday adds the weekly synthesis.

## 0. Env

Required:
- `PAPERCLIP_*` standard
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID`
- `GOAL_ID`

Missing → comment, `blocked`, exit.

## 1. Identity + wake

```
GET /api/agents/me
```

Note weekday.

## 2. Read state

- [`life/areas/trend-log.md`](life/areas/trend-log.md) — yesterday's scan (avoid duplication).
- [`life/areas/source-list.md`](life/areas/source-list.md) — what to scan.
- [`../seo-aeo-specialist/life/areas/seo-playbook.md`](../seo-aeo-specialist/life/areas/seo-playbook.md) — current playbook (so you know what's already validated).

## 3. Approvals + HR loop

Standard handling.

## 4. Daily scan (every weekday)

Cover all 4 source classes. Use Composio + `tool-belt/skills/web-search` + `coreyhaines31/marketingskills/ai-seo` + `resciencelab/opc-skills/seo-geo` skills.

### 4a. Industry blogs (~15 min equivalent)
For each source in `source-list.md` "Industry blogs":
1. Pull last 24h of posts (RSS via Composio if available, else web-search "site:domain.com 24h").
2. Identify any post that mentions: AEO/GEO, Google update, schema markup change, content strategy shift, technical SEO change, AI-search citation tactics.
3. Note relevant ones with title + URL + 2-line takeaway.

### 4b. Google Search Central
Check Google Search Central blog (`https://developers.google.com/search/blog`) + Search Liaison Twitter/X for last 24h. Any algorithm updates, indexing changes, or schema additions?

### 4c. AEO / GEO landscape
- OpenAI status / changelog
- Perplexity changelog
- Anthropic news
- Google AI Overviews changes
- Practitioner threads on X / LinkedIn about AEO tactics

### 4d. Competitor moves
List of competitors maintained in `source-list.md`. For 2–3 per day (rotate through the list weekly):
1. Check their blog: any new posts in last 7 days? On what topics?
2. Spot-check Ahrefs-equivalent or Search Console-style data via Composio if available.
3. What are they newly ranking for or pivoting toward?

## 5. Output: trend-log entry

Append to `trend-log.md`:

```
## YYYY-MM-DD scan

### Industry trends
- **<headline>** — <URL> — <takeaway> — **Action:** roadmap-add | playbook-update | brief-proposal | tech-update | deprecate — <specific>

### Google updates
- (only when something happened)

### AEO / GEO landscape
- ...

### Competitor moves
- ...
```

Every line has an Action. Empty sections OK on quiet days; partial scans are not.

## 6. Distribute actions

For each Action in today's scan, create a Paperclip subtask:

- **roadmap-add** → SEO + AEO Specialist:
  ```
  Title: Roadmap candidate — <keyword/topic>
  Body: source URL + rationale + suggested tier + AEO angle
  ```
- **playbook-update** → update `../seo-aeo-specialist/life/areas/seo-playbook.md` directly (you co-maintain). Comment on SEO + AEO Specialist's standing issue with the diff.
- **brief-proposal** → SEO + AEO Specialist:
  ```
  Title: Article brief proposal — <slug>
  Body: target query + angle + AEO consideration + source reasoning
  ```
- **tech-update** → Web Operations:
  ```
  Title: Tech SEO update — <change>
  Body: spec + source URL + reasoning
  ```
- **deprecate** → SEO + AEO Specialist:
  ```
  Title: Stop doing — <thing>
  Body: source URL + why it's no longer working
  ```

## 7. Major-update fast path

If today's scan caught a major Google core update OR major AI search platform change:
1. Post `SIGNAL` immediately on Head of Marketing's standing issue.
2. Tag SEO + AEO Specialist + Web Operations.
3. Within 48h (next 1–2 heartbeats): produce a structured action plan as a sub-issue.

## 8. Monday weekly synthesis

Beyond the daily scan:
1. Roll up the past week's actions per category.
2. Update `seo-playbook.md` "Weekly observations" + new patterns section.
3. Propose ≥3 article briefs to SEO + AEO Specialist.
4. Post DIGEST on Head of Marketing's `Marketing Weekly` issue:
   ```
   SEO TRENDS — week of YYYY-MM-DD

   Top 3 trends:
   1. <trend> → <action taken>
   2. ...

   Major updates this week: <count>
   New roadmap candidates: <count>
   Playbook updates: <count>
   ```

## 9. Fact extraction

Use `para-memory-files`. Distill into:
- `life/areas/trend-patterns.md` — what kinds of trends consistently produce wins.
- `life/areas/competitor-watch.md` — long-term competitor profiles.
- `life/areas/google-update-history.md` — major updates + Akira's response history.

## 10. Exit

- Comment on `in_progress` work.
- `X-Paperclip-Run-Id` on mutating calls.
- Clean exit.
