# ICP Playbook — Lead Researcher

> Canonical definition of who goes into the prospect Google Sheet and at what tier. Updated monthly by Head of Sales in collaboration with Growth Analyst; Lead Researcher reads on every heartbeat.
>
> **Operates under** [`../../head-of-marketing/life/areas/demand-creation-playbook.md`](../../head-of-marketing/life/areas/demand-creation-playbook.md) — the signal hierarchy is defined there. This file is the concrete scoring criteria.
>
> **Last updated:** 2026-04-16

## Target geography

- Sweden and Norway, full country coverage.
- Primary cities: Stockholm, Gothenburg, Malmö, Oslo, Bergen, Trondheim.
- Secondary: any Nordic city with population ≥ 50K and a concentrated hospitality sector.

## Target business types

- Restaurants: Cafe / Fast casual / Fine dining / Bar / Bakery / Food truck / Pizzaria (matching Notion's `Restaurant Type` options).
- Hotels: independent hotels and small hotel groups (up to ~20 properties).
- Restaurant groups operating multiple brands under one ownership.

## Tier definitions

### Tier 1 — drop-everything prospects

All of:
- Multi-location operator (≥ 3 locations for restaurants; ≥ 2 properties for hotels).
- Decision-maker identified by name + role + contactable.
- Visible admin function (central office / HQ page on website, LinkedIn employees titled "office manager", "reservation manager", "admin", etc.).

### Tier 2 — solid prospects

At least two of Tier 1's three criteria.

### Tier 3 — long-tail

Single-location, decision-maker not identified, or no admin signal. Kept in sheet but deprioritized.

## ICP scoring as hypothesis (not checklist)

Scoring isn't a static checklist — it's a three-question hypothesis the team tests with content and refines with data:

> **Who buys?** Multi-location Nordic hospitality operators with a central admin function (office managers, reservation coordinators, back-office staff).
>
> **Why now?** Rising Nordic labor costs + staff shortages + openness to AI automation driven by peer examples.
>
> **What signal?** Engagement on Jules's LinkedIn content that names their specific pain (no-shows, reservation phone volume, admin time cost). Not "interested in AI" — interested in a specific Nordic-hospitality problem.

Every quarter, Head of Sales + Growth Analyst + Head of Marketing review whether the three answers still hold. If Tier 1 conversion rates are dropping, the hypothesis probably needs updating — new segment, new pain, new signal.

## Apollo search templates (seed)

### Template A — Multi-location Nordic restaurant operators (Tier 1 candidates)

- Industries: Restaurants, Food & Beverages, Hospitality.
- Locations: Sweden OR Norway.
- Company headcount: 50–1000.
- Keywords in job titles: "owner", "founder", "operations director", "general manager", "CEO", "restaurant group director".
- Exclude: franchise corporate HQs unless they control the individual restaurants.

### Template B — Nordic hotel groups (Tier 1/2 candidates)

- Industries: Hospitality, Hotels & Resorts.
- Locations: Sweden OR Norway.
- Company headcount: 20–500.
- Keywords in job titles: "general manager", "revenue manager", "operations director", "owner".

### Template C — Signal scrape (indirect)

Browse LinkedIn posts by Nordic hospitality operators who complained about reservations, phone volume, admin cost, or no-shows in the last 60 days. Commenters + authors are warm-Tier-2.

## Disqualifiers (never enter sheet)

- Single-person operations.
- Fine-dining without any online booking (too analog, not the right fit).
- Companies that already appear in the Notion CRM (check first — the Steward's snapshot has all contacted records).
- Food retail (grocery / deli unless hosting a kitchen).
- Businesses outside hospitality primary use case.

## Volume target (seed)

- 50 prospects/week into the Sheet.
- Split: ~20 Tier 1, ~20 Tier 2, ~10 Tier 3.

Adjust after 4 weeks of first-touch-success data from Growth Analyst.

## Sourcing priority order (per demand-creation framework)

Run sources in this order each heartbeat — don't start Apollo searches until engagement sourcing is caught up.

1. **LinkedIn engagements on Jules's own posts (primary).** Check LinkedIn Engagement Agent's `engagement-log.md` for engagers flagged Tier 1 in the last 24h not yet in the sheet. Process all of them first — these are buying signals.
2. **LinkedIn engagements graded Tier 2 (secondary).** Worth including but lower priority — less hot but still warmer than cold Apollo.
3. **Apollo ICP searches (tertiary).** Fill remaining weekly quota from cold Apollo sourcing. These are classical firmographic fits, not buying-signal leads.
4. **Directory + signal scrape (supplementary).** Wednesday rotation — LinkedIn public search, Nordic hospitality directories.

When adding an engagement-sourced prospect to the sheet, set `Source: LinkedIn-Engagement` and include the post URL + engagement type in the notes. These rows get higher priority in the Follow-Up Manager's reactivation flow.

## Column schema for the Google Sheet (must stay in sync with `PROSPECT_SHEET_ID`)

See `SECRETS-CHECKLIST.md` for the canonical column list. If the sheet's structure changes, Lead Researcher flags to Head of Sales.
