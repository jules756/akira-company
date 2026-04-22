# Retention Playbook — Customer Success Manager

> Owned by Customer Success Manager. Referenced on every heartbeat. SLAs + templates + escalation rules for the CS function.
>
> **Last updated:** 2026-04-16 (seed)

## SLAs

| Event | Max response time |
|---|---|
| Churn signal detected | Flag same heartbeat (i.e., within 24h). |
| Library skill shipped → upsell sweep | Same heartbeat the ship is announced. |
| 90-day referral ask | Within 7 days of the 90-day mark. |
| Production client goes 30 days silent | Flag same heartbeat. |
| Monthly upsell sweep | First Monday of the month, completed in one heartbeat. |

## Target metrics (North Star: 1M SEK/mo total revenue)

- **NRR ≥ 120%** (rolling 3-month). Existing clients expand faster than any attrition.
- **Referrals ≥ 0.33 per quarter per Production client.** 30 clients × 0.33 = 10 referred leads per quarter.
- **Churn ≤ 10% annual.** Y1 aspirational; tune as real data arrives.

## The three loops — concrete rules

### 1. Retention

- Daily health scan is not optional.
- Churn signal → SIGNAL on CEO's Governance Review + subtask to Head of Sales same heartbeat.
- Never let Implementation Engineer's incidents file sit unread for >24h.

### 2. Library-driven upsell

- Every skill ship = immediate sweep.
- Subtask format in AGENTS.md is the contract. Don't send to Head of Sales if rationale isn't tied to a specific client signal.
- Respect the no: 60-day cooldown on rejected upsells before re-proposing.

### 3. Referral

- 90-day mark is a trigger, not a suggestion.
- Referral ask is specific: name a real win, in the client's language, framed as a favor to a peer.
- Track referred companies back to the source client — attribute pipeline credit.

## Churn signal playbook

| Signal | Severity | Action |
|---|---|---|
| ≥3 unresolved incidents / 7d | High | SIGNAL + Jules calls client within 48h. |
| 30 days client silence | Medium | SIGNAL + Head of Sales nudges via email. |
| Explicit "pause/reduce" request | High | SIGNAL + Jules + Head of Sales joint response. |
| Single negative feedback session | Low | Log, watch. If repeats → escalate. |
| Late / disputed payment | High | SIGNAL + Head of Sales + Jules. |

## Upsell signal library

Recognized patterns where a client becomes eligible for a new capability:

- **Email skill installed; mention of TripAdvisor / reviews in feedback** → Reviews skill upsell.
- **Voice skill installed; mention of social media management by staff** → Social skill upsell.
- **Multi-skill client; no unified visibility** → Dashboard skill upsell.
- **Reaches 6-month anniversary** → check whether client has room for any additional capability they don't yet have.

Add to this list as new upsell patterns emerge.

## Referral ask templates

Three variants — Content Writer picks based on what fits the client's relationship.

### Variant A — direct ask (peer referral)
> We've been running your <voice/email/booking> for <months> — <specific win>. If anyone in your network is facing similar <pain>, we'd love an intro. No pressure.

### Variant B — case study exchange
> We're putting together a short case study on <specific win>. Would you be open to (1) a 30-min recorded chat we'd publish, and (2) any intros it sparks for peers of yours?

### Variant C — "who comes to mind"
> Quick one: if another <restaurant-group / hotel operator> in Nordic hospitality reminded you of your own ops a year ago, who comes to mind? No commitment — just curious.

Language matches the client's primary (Swedish / English / French — never Norwegian).

## What to never do

- Never bypass Head of Sales on an upsell close. You surface, they close.
- Never send client-facing messages directly. Jules sends.
- Never assume a "no" is permanent. 60-day cooldown, then re-evaluate.
- Never propose a skill the client already has.
- Never miss a 90-day referral ask.
