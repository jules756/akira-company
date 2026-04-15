# Demand Creation Playbook — Akira Agent

> Owned by Head of Marketing. Read by every Marketing + Sales agent on every heartbeat. This is the meta-framework underneath `content-playbook.md`, `engagement-patterns.md`, and `icp-playbook.md`.
>
> **Framework source:** Cold to Cash — *"Les signaux d'achat sont morts. Voici ce qui les remplace."* Adapted for Akira's Nordic-hospitality ICP.
>
> **Last updated:** 2026-04-16

## The inversion

**Classical buying signals are dead.** Funding raised, hiring spikes, "growing" tags — everyone pulls the same data from the same tools (Apollo, Clay, Pharow). The prospect gets 50 identical messages the day the signal fires. Conversion collapses.

**Replace signal-chasing with demand creation.** We don't scrape signals others built. We *create* signals by putting ICP-calibrated content into Jules's LinkedIn feed, and we treat every engagement (like, comment, share) on that content as a real buying signal — because the prospect chose to react.

## The 4 layers (the system every agent plugs into)

```
┌─ Content layer                                 (Head of Marketing + Content Writer + LinkedIn Engagement)
│  3 ICP-calibrated LinkedIn posts per week from Jules's account.
│  Every post is a test: what signal is this post trying to draw out?
│
├─ Capture + enrichment                          (LinkedIn Engagement + Lead Researcher)
│  Every liker / commenter / sharer on Jules's posts is captured.
│  Enriched (LinkedIn profile → company → ICP match).
│  Scored Tier 1 / 2 / 3 against the ICP playbook.
│
├─ Reactivation (24h window)                     (Follow-Up Manager + Content Writer + LinkedIn Engagement)
│  Within 24h of a Tier 1 engagement: a personalized DM (or email if we have the address)
│  anchored in the specific post + the specific engagement.
│  Never a generic template.
│
└─ Conversion                                    (Head of Sales + Follow-Up Manager)
   Cal.com booking. Auto pre-call brief generated AT BOOKING TIME by Follow-Up Manager
   (Cold-to-Cash workflow: Cal.com → gather LinkedIn + email + CRM + engagement history
   → LLM-structured brief → attached to Notion Company page via Steward).
   See `../../follow-up-manager/life/areas/pre-call-brief-template.md`.
   Head of Sales reads brief, doesn't regenerate. Discovery call. Post-call notes
   appended to same Notion page after the call ends.
```

## Signal hierarchy (re-ranking of priorities)

| Signal class | Source | Action cadence | Conversion expectation |
|---|---|---|---|
| **Primary (active buying signal)** | Engaged with Jules's LinkedIn post in last 30 days. | Reactivate within 24h. Priority above everything else. | High — they chose to react. Cold-to-Cash benchmark: 25%+ reply rate on engagement-anchored reactivation vs. 5–10% on cold outbound. |
| **Secondary (warm fit)** | Sourced by Lead Researcher via Apollo / directories, matches Tier 1 ICP. | Route through content targeting (draw them to engage first) OR cold outbound only if patient. | Medium — classical outbound pace. |
| **Tertiary (classical "buying signals")** | "Just raised" / "hiring" / "growing" tags pulled from public data. | **Never the trigger.** Use only as scoring/prioritization on already-engaged prospects. | Low as trigger. Useful as ranker. |

### Classical signals are scoring, not sourcing

Funding raises, hiring spikes, "growing" tags — these still exist, they're just not *why* we contact someone. Once a prospect has engaged with Jules's content, Apollo / Clay-style data (headcount, segment, stack, hiring) tells us whether to treat them as Tier 1 or Tier 3. Useful as a ranker, never as the reason to reach out.

## The third model — outbound-led inbound

This isn't pure inbound (waiting for signups) or pure outbound (cold spraying classical signals). It's a hybrid:

- **Content generates exclusive signals.** Every post creates engagements *only we see*.
- **Outreach reactivates those signals.** We DM people who already raised their hand.
- **Conversations fuel new content.** Every call surfaces fresh objections, questions, and topics — those become next week's posts.

The loop compounds: better content → more engagements → more reactivations → more conversations → better content. Classical outbound is linear (spray → pray → burn list). Demand creation is a flywheel.

## Exclusion filters — who we never reactivate

Before LinkedIn Engagement flags a Tier 1 engager for reactivation, AND before Lead Researcher adds an engager to the sheet, filter:

| Rule | Why |
|---|---|
| Already in Notion CRM | They've been contacted before — route through Head of Sales's pipeline, not a fresh reactivation. |
| Already in prospect sheet with `First-Touch Date` set | In-flight somewhere — don't double-touch. |
| Marked as competitor | Separate workflow if any — never reactivate for sales. |
| Below minimum ICP size (solo operator / single-location café with no multi-location parent) | Time-box: Tier 3 doesn't convert at Akira's price point. |
| Latest Outreach Log `Outcome = Not interested` within last 180 days | Respect the no. Re-evaluate after 6 months. |
| In an industry block-list (competitors, unrelated verticals) | Configurable via `life/areas/exclusion-list.md` under Head of Marketing. |

Any engager hitting one of these filters is logged in `engagement-log.md` with `Tier: excluded — <reason>` and not flagged for reactivation.

## Target metrics (what "working" looks like)

Calibrated to Akira's scale — these are Phase-1 targets; tighten based on Growth Analyst's data.

| Metric | Target |
|---|---|
| LinkedIn posts per week (from Jules's account) | 3 |
| Engagement → Tier 1 prospect conversion | ≥30% of engagers match ICP |
| Tier 1 engagement → 24h reactivation | 100% |
| Reactivation → Discovery call booked | ≥15% |
| Discovery call → deal closed | ≥20% (Cold-to-Cash reported 33%, but assume lower for conservative) |
| LinkedIn monthly impressions | ≥50K within 90 days, ≥200K within 180 days |

## What this changes for each agent

### Head of Marketing
- Weekly content calendar locks **3 posts** (not a suggestion — the minimum).
- Every post has a **signal hypothesis**: "this post is designed to draw engagement from [ICP segment]." Posts without a signal hypothesis get rewritten before approval.
- Monday review scores each post: what kind of engagement did it draw? Did the engagers match ICP? Did any convert via reactivation?

### Content Writer
- Every LinkedIn post brief must include the **signal hypothesis**. Drafts without it get sent back.
- Post angles optimize for *engagement*, not reach. A post that gets 3 ICP comments beats a post that gets 50 random likes.
- Reactivation DMs are drafted referencing the specific post + the specific engagement (quote the comment, or name the post's topic). Generic reactivations = rejected.

### LinkedIn Engagement Agent
- **New responsibility:** scan engagements on Jules's own posts after each publish. Log every liker, commenter, and sharer to `life/areas/engagement-log.md` with: post URL, engager profile URL, engagement type (like/comment/share), engagement content (if comment).
- Flag Tier 1 engagers to Lead Researcher (for CRM enrichment) + Follow-Up Manager (for 24h reactivation).

### Lead Researcher
- **Primary sourcing shifts.** Start each day by pulling the last 24h of LinkedIn engagements. For every Tier 1 engager, ensure they're in the prospect sheet (with `Source: LinkedIn-Engagement`, `Tier: 1`).
- **Apollo demoted.** Apollo sourcing continues but is secondary. Apollo fills the top of funnel when LinkedIn engagement doesn't — not the other way around.

### Follow-Up Manager
- **New workflow:** LinkedIn engagement reactivation. When LinkedIn Engagement flags a Tier 1 engager, within one heartbeat (so definitively within 24h given daily cadence, and faster if we need mention-triggered):
  1. Request Content Writer to draft a personalized DM referencing the specific post + engagement.
  2. Once drafted, request LinkedIn Engagement to send (full-draft mode Phase 1; auto-send later phases).
  3. Log the touch in Outreach Log via Steward with `Channel=DM`, `Notes: Reactivation from LinkedIn engagement on <post URL>`.

### Head of Sales
- **Priority reordering** in the daily CRM sweep:
  - **Priority 1:** Active-stage leads who also engaged with Jules's LinkedIn content in the last 30 days.
  - **Priority 2:** In-pipeline leads (Discovery / Proposal) without recent engagement.
  - **Priority 3:** Lead-stage cold prospects (classical Apollo-sourced).
- Escalations to Jules lead with engagement context when present: *"This lead commented on your reservation no-shows post last Tuesday."*

### Lead Magnet Creator
- Every magnet ships with a LinkedIn launch post whose purpose is **engagement generation**, not vanity metrics. The magnet itself drives email signups; the launch post drives LinkedIn engagements we can reactivate.
- Magnet conversion metric includes: engagements on launch post → Tier 1 → reactivation → booked call.

## What we do *not* do

- We do not buy Clay / Pharow / Apollo enrichment for the *purpose* of signal-chasing. We use Apollo for ICP sourcing of new prospects to targetin content, not as a conversion trigger.
- We do not spray cold DMs to classical-signal prospects without a prior engagement or content exposure. Either the prospect engaged with us, or we invested in engaging with them publicly first.
- We do not let engagement reactivations sit past 24h. Cold engagement is dead engagement.
- We do not send templated reactivations. Every DM names the post and the engagement.

## Cross-references

- [`content-playbook.md`](content-playbook.md) — weekly cadence, pillars, language rules.
- [`../../linkedin-engagement/life/areas/engagement-patterns.md`](../../linkedin-engagement/life/areas/engagement-patterns.md) — engagement actions on *others'* posts + engagement-log on *our* posts.
- [`../../lead-researcher/life/areas/icp-playbook.md`](../../lead-researcher/life/areas/icp-playbook.md) — ICP tiers + sourcing templates.
- [`../../follow-up-manager/life/areas/follow-up-patterns.md`](../../follow-up-manager/life/areas/follow-up-patterns.md) — cadence seed + reactivation rules.
