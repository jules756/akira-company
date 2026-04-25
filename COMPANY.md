---
name: Akira Agent
description: The AI workforce layer for Scandinavian hospitality. Modular AI employees — voice, email, social, reviews, dashboard — deployed as skills from a compounding library. 3,700 SEK/month per skill vs. 6,000–12,000 SEK/month in staff cost.
slug: akira-agent
schema: agentcompanies/v1
version: 1.0.0
license: UNLICENSED
authors:
  - name: Jules
    email: jules@akira-agent.com
goals:
  - Reach 1,000,000 SEK/month total revenue run rate by end of 2027 via modular AI skills deployed to Scandinavian hospitality (restaurants, hotels, venues)
  - Year-1 beachhead (end 2026) — 30 clients live in Production, ~110K SEK/month MRR run rate, ~2.1M SEK total Year-1 revenue
  - Compound a hospitality-specific capability library — build once, deploy N — that by client 20 holds 10–15 battle-tested capabilities no generalist agency can replicate
  - Operate with sales + customer success as the only two human functions; every other role is agent-supported
  - Own Sweden and Norway in Year 1; extend to Denmark in Year 2
---

# Akira Agent

**The AI workforce layer for Scandinavian hospitality.** Every restaurant, hotel, and venue that can't afford a full ops team gets one — through us.

## The core insight

The market sells static software. We sell a **living system**. An n8n workflow breaks the moment the restaurant's operation shifts. An agent with the right skill adapts. That's defensible because building it well requires deep hospitality domain knowledge — which we accumulate client by client.

We're not selling automation. We're selling an **AI employee that knows the business**.

## The product — a modular AI employee

Starts with one skill. Learns the client's operation. Grows over time. Every new skill added to the library deploys to existing clients as an upsell.

**Skill stack:**

- **Voice** — reservations, cancellations, upsells (Vapi + Twilio).
- **Email** — inquiry triage, group bookings, supplier comms.
- **Social media** — content, scheduling, engagement.
- **Reviews** — Google / TripAdvisor monitoring + response drafting.
- **Dashboard** — visibility layer across all skills deployed to a client.

## The moat

Not the tech. The **hospitality-specific capability library** we're accumulating. By client 20 we have 10–15 battle-tested capabilities no generalist agency can replicate.

## The pitch

> *"You're paying 6,000–12,000 SEK/month for someone to handle emails. We do it for 3,700 SEK and it never calls in sick."*

## How Akira runs

Paperclip absorbs monitoring, onboarding flows, client reporting, follow-ups. Akira has exactly **two human functions**: Sales (close) and Customer Success (retain, upsell, refer). Every other role is agent-supported.

## Organisation

**18 agents across 3 teams + 1 Web publishing subteam + Chief of Staff + 2 CEO-reporting ICs:**

- **Chief of Staff** — routing, handoffs, duplicate checks, queue triage, ownership assignment
- **Sales team** — Head of Sales · CRM Steward · Follow-Up Manager · Growth Analyst · Lead Researcher
- **Marketing team** — Head of Marketing · LinkedIn Engagement · Content Writer · Lead Magnet Creator
  - **Web publishing subteam** — SEO + AEO Specialist · SEO Trend Researcher · Web Writer · Web Designer · Web Operations
- **Engineering & Product team** — CTO · Founding Engineer · Implementation Engineer · UX Designer · Product Manager
- **CEO-reporting** — CEO · Auditor · Customer Success Manager

Four operating flows organize the work: lead and reputation · new client deployment · skill library growth · maintenance.

Every agent runs on `opencode_local` with `azure-grok/grok-4-1-fast-non-reasoning` and reads its own `AGENTS.md` + `SOUL.md` + `HEARTBEAT.md` on every wake. Budgets, approval gates, and audit rituals are codified in the CEO's `governance.md`.

See [`docs/paperclip-routines.md`](docs/paperclip-routines.md) for the external routine creation list. Agent docs describe behavior; this file is the manual Paperclip routine catalog.

---

Built on [Paperclip](https://paperclip.ing) — the control plane for autonomous AI companies.
