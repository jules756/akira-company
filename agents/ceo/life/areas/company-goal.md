# Akira Agent — Company Goal

> Owned by CEO. Read by every agent on every heartbeat. This is the canonical framing. When anything else drifts, anchor back here.
>
> **Last updated:** 2026-04-16

## What Akira is

The **AI workforce layer for Scandinavian hospitality**. Every restaurant, hotel, and venue that can't afford a full ops team gets one — through us.

## Core insight

The market sells static software. We sell a **living system**. An n8n workflow breaks the moment the restaurant's operation shifts. An agent with the right skill adapts. That's defensible because building it well requires deep hospitality domain knowledge — and we're accumulating it client by client.

We're not selling automation. We're selling an **AI employee that knows the business**.

## The product — a modular AI employee

Starts with one skill. Learns the client's operation. Grows over time. Every new skill we add to the library deploys to existing clients as an upsell.

### The skill stack

- **Voice** — reservations, cancellations, upsells (Vapi + Twilio).
- **Email** — inquiry triage, group bookings, supplier comms (n8n today; replacing with a custom email-skill).
- **Social media** — content, scheduling, engagement.
- **Reviews** — Google / TripAdvisor monitoring + response drafting.
- **Dashboard** — visibility layer across all skills deployed to a client.

The engineering team doesn't specialize *per skill type*. **Founding Engineer builds any skill the library needs. Implementation Engineer deploys any skill from the library to any client. Product Manager audits the library when a new client proposal arrives — reuse first, build only where the library doesn't cover.** Build once; deploy N.

## The moat

**Not the tech — the hospitality-specific skill library we're accumulating.** By client 20 we have 10–15 battle-tested skills no generalist agency can replicate. We already know the industry, the tools, the decision-makers.

## The pitch

> *"You're paying 6,000–12,000 SEK/month for someone to handle emails. We do it for 3,700 SEK and it never calls in sick."*

### The ROI case

- Restaurants spend 5–10 h/week on emails × 280 SEK/h = **6,000–12,000 SEK/month in staff cost**.
- Our email skill: **3,700 SEK/month**.
- Setup pays back in **3–5 months**.
- Conversation is over in 60 seconds.

## Market position

- **Below:** n8n freelancers doing one-off automations for ~5,000 SEK. They sell tools.
- **Above:** enterprise consultancies charging 500,000 SEK for strategy decks. They sell reports.
- **Akira:** 20,000–57,000 SEK setup + 3,700 SEK/month per skill. Deployed in two weeks. ROI visible in month one. We sell employees.

## Geography

- **Year 1 (2026):** Sweden + Norway. High labor costs, early-adopter mentality, restaurants saving 8,000 SEK/mo for 3,700 SEK is an obvious decision.
- **Year 2 (2027):** Add Denmark. Skill library mature — deployment is near-fully templated.

## Targets

### Year 1 (end of 2026) — Beachhead

- **30 clients** signed and Production-live.
- **~110,000 SEK / month MRR** at full Y1 run rate (30 × 3,700 SEK avg, single-skill per client dominant).
- **~970,000 SEK** in cumulative setup revenue.
- **~2.1M SEK total revenue** (MRR ramp + setups).
- **Year 1 is beachhead.** Validate the skill library, the demand-creation loop, the delivery pipeline. No rush on revenue — rush on proof.

### North Star (Y2 exit — end of 2027) — Scaling

- **1,000,000 SEK / month revenue run rate** (~12M SEK / year).
- Likely composition: ~150–200 clients × 2–3 skills per client on average. Multi-skill per client is where the library compounds.
- Denmark operational.
- Customer Success function detecting upsells from new library skills automatically.
- Sales + Customer Success are the only human functions. Every other function is agent-supported.

Gap between Y1 and North Star is ~10×. Closed through: more clients × more skills per client × more geographies. Not through a hand-rolled pivot.

## How Akira runs (target state within 3 months)

Paperclip absorbs monitoring, onboarding flows, client reporting, follow-ups. From then on Akira has exactly **two human functions**:

1. **Sales** — pipeline, outreach, owner calls, closes. Supported by Head of Sales + team.
2. **Customer Success** — retain, upsell (library-driven), refer. Supported by Customer Success Manager agent.

Technical side is invisible to the client. They see results.

## Year-2 strategic expansion

- **Denmark** operational.
- **Skill library matures** — deployment is near-fully templated.
- **Customer Success-led upsell flywheel** — every new library skill auto-triggers a sweep for eligible existing clients.
- **Referral flywheel** — 90-day post-Production referral asks are standard.

We're the dominant AI workforce provider for Scandinavian hospitality. Not a SaaS company. Not a consultancy. A lean, highly automated agency where sales and customer success are the only human functions.

## The principle every agent operates under

Every action connects to one of three compounding loops:

1. **Demand creation** (Marketing team) — content creates exclusive buying signals → reactivation → discovery call.
2. **Library compounding** (Engineering team + PM + CS) — every skill built deploys to every eligible client. Build once, deploy many. PM audits fit when a proposal arrives; Founding Engineer builds the gap; Implementation Engineer deploys; CS sweeps existing clients on every new library skill.
3. **Retention compounding** (Customer Success) — every retained client is a multi-skill expansion candidate + referral source.

When in doubt, ask: does this work feed one of the three loops? If not, stop.

## Reference

The goal must be created in Paperclip UI → Goals and its UUID pasted into secret `GOAL_ID`. Every agent-created issue carries this UUID.
