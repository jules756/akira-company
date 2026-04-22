# Rubric — Web Operations

Scored daily by Auditor. Each dimension 0–5.

### 1. Deploy reliability (0–5)
- **5** — Every Web Writer / Web Designer / SEO change shipped to Vercel within 1 heartbeat of approval. Zero broken production deploys this week. Rollback path verified.
- **3** — Most ships clean, occasional fix-forward.
- **1** — Multiple regressions.
- **0** — Production broken.

### 2. Analytics tagging completeness (0–5)
- **5** — Every shipped page has: GA4 page_view event, GTM container live, custom events for primary CTAs, scroll-depth tracking, conversion event firing on form submits / magnet signups.
- **3** — Tagging on most pages, some events missing.
- **1** — Inconsistent tagging.
- **0** — Tagging gaps prevent attribution.

### 3. Core Web Vitals + indexing (0–5)
- **5** — Weekly Search Console + GA4 review. Any page with CWV regression or indexing issue (noindex / 404 / blocked) flagged + fixed same week.
- **3** — Reviewed but slower fix cycle.
- **1** — Reactive only.
- **0** — Not monitored.

### 4. SEO implementation accuracy (0–5)
- **5** — Schema markup on every article matches SEO + AEO Specialist's spec. Sitemap auto-updates. Internal links resolve. Redirects in place where needed. No technical SEO debt.
- **3** — Mostly correct, occasional miss.
- **1** — Spec drifts from implementation.
- **0** — Implementation diverges from spec.

### 5. Founding-Engineer escalation discipline (0–5)
- **5** — Code-level changes (new platform feature, complex backend) properly escalated to Founding Engineer with spec. Day-to-day deploys handled without escalation.
- **3** — Mostly clean, occasional unnecessary escalation.
- **1** — Either over-escalates or hacks past Founding Engineer.
- **0** — Escalation broken.
