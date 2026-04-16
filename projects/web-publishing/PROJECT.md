---
name: "Web Publishing"
slug: web-publishing
status: in_progress
color: "#0ea5e9"
---

# Web Publishing

The daily execution engine for akira-agent.com. 2 articles per day, 7 days/week. Case studies for every Production client. Page designs and templates. Vercel deploys + GA4 / GTM / Microsoft Clarity / Search Console daily monitoring instrumentation.

The strategic ammunition (keyword roadmap, AEO targets, trend insights) lives in the sister project [`seo-aeo-growth`](../seo-aeo-growth/PROJECT.md). This project is where briefs become live, instrumented pages.

## Scope

- **Daily blog publishing** — Web Writer ships 2 articles/day, 7 days/week from SEO + AEO Specialist's briefs.
- **Case studies** — Web Writer covers every Production client within 30 days of go-live.
- **Page templates + designs** — Web Designer using v0.dev + Vercel-native components for landing pages, /services, /case-studies, hero sections, OG images.
- **Vercel deploys** — Web Operations is the single deployer for akira-agent.com + magnet pages + custom tools (single-stack rule).
- **Analytics instrumentation** — GA4 + GTM + Microsoft Clarity tags wired on every page; conversion events mapped to Tally / Beehiiv / Cal.com.
- **Daily Search Console analysis** — Web Operations reviews GSC every morning, surfaces opportunities to SEO + AEO Specialist (their work then lives in `seo-aeo-growth`).
- **Web ops monitoring** — Vercel deploy log + GA4 anomalies + magnet form pipeline health (Mon/Wed/Fri).
- **Friday performance review** — weekly DIGEST to Head of Marketing.

## Primary agents

- **Web Writer** — drafts 14 articles/week + case studies.
- **Web Designer** — page templates + landing pages + OG images via v0.dev.
- **Web Operations** — Vercel deployer + analytics + Search Console monitor + technical SEO implementation.

Coordinates with SEO + AEO Specialist + SEO Trend Researcher (briefs, schema specs, trend-driven changes from `seo-aeo-growth`), Lead Magnet Creator (magnet pages on the same Vercel project), Founding Engineer (rare backend escalations).

## References

- [`editorial-calendar.md`](../../agents/web-writer/life/areas/editorial-calendar.md)
- [`web-design-system.md`](../../agents/web-designer/life/areas/web-design-system.md)
- [`deployment-playbook.md`](../../agents/web-operations/life/areas/deployment-playbook.md)
- [`analytics-tracking-spec.md`](../../agents/web-operations/life/areas/analytics-tracking-spec.md)

## Routines feeding this project

- `daily-blog-publish` (Web Writer) — 2x/day, 7 days/week (9am + 3pm Stockholm)
- `daily-search-console-analysis` (Web Operations) — every morning 8am
- `web-ops-monitoring-mwf` (Web Operations) — Mon/Wed/Fri 11am
- `friday-web-performance-review` (Web Operations) — Fridays 3pm

## Success criteria (ongoing)

- 14 articles/week shipped without backlog.
- ≥1 case study per new Production client within 30 days.
- Every shipped page meets Core Web Vitals green at first deploy.
- Every shipped page has correct GA4 + GTM event coverage.
- Vercel deploys: zero broken production deploys per week.
- Daily Search Console anomalies surfaced same heartbeat to SEO + AEO Specialist.
