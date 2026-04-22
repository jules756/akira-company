# Akira Skill Library — Inventory

> The canonical inventory of every skill Akira has built, in flight, or considered. Maintained by Founding Engineer. Read by Product Manager (for new-client fit audits) and Customer Success Manager (for upsell sweeps).
>
> **Last updated:** 2026-04-16 (seed)

## How to read this file

Each skill has five states:
- `production` — deployed to at least one client, stable, ready for broad deploy.
- `beta` — built and deployed to 1 pilot client; watch before promoting to `production`.
- `building` — Founding Engineer is actively building.
- `planned` — on the roadmap; not started.
- `retired` — no longer maintained; existing deployments need migration.

When a skill moves to `production`, Founding Engineer notifies Customer Success Manager the same heartbeat so a library-sweep for upsells can fire.

## The library

### Voice
- **State:** `production` (single-client pilot; formally promote after client #2).
- **Stack:** Vapi + Twilio.
- **Deploy pattern:** per-client Vapi prompt + Twilio number + Cal.com booking webhook.
- **Monthly price (list):** 3,700 SEK / month / location.
- **Install cost:** 20,000–57,000 SEK depending on complexity (integrations, scripts, language mix).
- **Languages supported:** Swedish, English. French by request. Never Norwegian (Jules' constraint).
- **Deploy docs:** `../implementation-engineer/life/areas/clients/<slug>/voice-log.md` template.
- **Known limitations:** Accent handling on heavy regional dialects; escalates to human after 2 misunderstood turns.

### Email
- **State:** `beta` (n8n-based; in-flight migration to custom email-skill).
- **Stack (today):** n8n flows + Gmail API.
- **Stack (target):** Custom email-skill built by Founding Engineer. Priority project.
- **Monthly price:** 3,700 SEK / month.
- **Install cost:** 20,000 SEK standalone; discounted bundled with Voice.
- **Deploy pattern:** per-client triage rules + reply templates + escalation inbox.
- **Flagship ROI story:** replaces 5–10 h/week of staff email handling → 6–12K SEK → 3.7K SEK → 3–5 month payback.

### Booking
- **State:** `production`.
- **Stack:** Cal.com.
- **Monthly price:** bundled with Voice or Email.
- **Deploy pattern:** per-client event types + booking rules.

### Social media
- **State:** `planned`.
- **Stack (proposed):** LinkedIn + Instagram + possibly Facebook via Composio. Content generation + scheduling + basic engagement.
- **Priority:** pulled forward when first client explicitly asks or when 3+ existing clients say they'd buy.
- **Upsell potential:** high (most restaurants have inconsistent social presence).

### Reviews
- **State:** `planned`.
- **Stack (proposed):** Google Reviews API + TripAdvisor (manual today, pending API access) + response drafting.
- **Priority:** activate when a live client asks OR when Customer Success surfaces it in 3+ feedback sessions.
- **Deploy pattern:** daily review scan + drafted responses for client approval + autopost (or full-draft based on trust tier).

### Dashboard
- **State:** `planned`.
- **Stack (proposed):** Vercel (Next.js) + Supabase backend. Client-facing UI showing all skills live, call logs, email stats, review health.
- **Priority:** activate when a client has 2+ skills live and wants unified visibility.
- **Upsell potential:** becomes standard once Voice + Email are both adopted per client.

## How the library becomes revenue (the compounding rule)

1. Skilled Builder ships capability X to `production`.
2. Founding Engineer updates this file + comments on CEO's Engineering Retro issue tagging Customer Success Manager.
3. CS Manager runs a library sweep same heartbeat: for every Production client, does X fit their ops?
4. Eligible clients → upsell subtasks to Head of Sales. Skill that took one build goes to N clients.

This compounds. Client 20 with 12 capabilities is very different economics from client 20 with 3 capabilities.

## Skill requests (incoming)

When PM or CS surfaces a client-specific need not covered by the library, add a row here:

| Date | Requester | Need | Current library coverage | Recommended action | Status |
|---|---|---|---|---|---|
| | | | | | |

Format:
- **Current library coverage:** `full` (existing skill handles it), `partial` (needs extension), `none` (net-new skill).
- **Recommended action:** `deploy existing`, `extend existing skill`, `build new skill`, `escalate to CTO`.
- **Status:** `under triage`, `queued`, `building`, `done`, `parked`.

CTO reviews queued skill-build requests weekly. Prioritizes based on client value + deploy-N potential.

## Maintenance + optimization log

When Implementation Engineer or Customer Success flags an issue with an existing capability (accent handling, integration breaking, performance drop), log here with the date, issue, fix shipped, and date shipped. Keeps capability evolution auditable.

*(Empty seed.)*
