# Founder Role — What Jules Does (and Doesn't Do)

> Owned by CEO. Read on every heartbeat. When CEO considers whether to hand something to Jules vs. handle internally, this file is the decision rule.
>
> **Framework source:** Headcount Zero, Ch 8 "Who Does What" and Ch 11 "You Are Not Optional."
>
> **Last updated:** 2026-04-16

## Jules's job now

You don't do the work. You design the company that does the work, and you make the calls agents can't make.

### Jules does

- **Strategy and goals.** Company direction, positioning, product bets, ICP refinement. The 500K SEK MRR goal and how Akira gets there.
- **Org-chart design.** Which agents exist, how they report, what they're responsible for. CEO executes hires through `paperclip-create-agent`, but Jules approves.
- **High-stakes calls.** Pricing, key customer relationships, legal / contractual commitments, anything touching $ above the agent budget floor, any irreversible external action.
- **Brand voice.** First 10 LinkedIn posts, first 10 DMs, first 4 newsletter issues, first 3 magnets — Jules approves each. Voice calibration is founder work until the team has enough examples to match.
- **Skill injection.** As Jules learns what works, he updates agent `AGENTS.md` files or rubrics or protocol docs. Agents improve because Jules feeds them sharper instructions.
- **Exception handling.** When an agent flags something for the board (via an approval gate), Jules decides. When the Auditor surfaces drift that CEO can't resolve, Jules decides.
- **Budget calibration.** Monthly review of per-agent spend vs. output. Raise/lower caps. Kill underperforming roles.
- **Customer-facing sales calls.** Until Akira has a dedicated Sales seat (human or voice-agent), Jules is on every discovery + proposal call. The Head of Sales *agent* preps; Jules talks.
- **Pivots.** When the data says "this ICP isn't working" or "this pricing isn't working," only Jules can call the pivot.

### Jules does NOT

- Write drafts — Content Writer does that.
- Comment on LinkedIn — Engagement Agent does that (with Jules approval in Phase 1).
- Update the Notion CRM — CRM Steward gates all writes.
- Configure Vapi / Twilio / Cal.com per client — Implementation Engineer does that.
- Review every deliverable — Auditor + CEO filter; Jules sees flagged output only.
- Coordinate agent-to-agent work — the hierarchy does that.
- Debug bugs — Founding Engineer (with `systematic-debugging`) does that.
- Track individual client build status — PM does that via Agents (The Bots) DB.
- Run weekly metrics — Growth Analyst does that.
- Do any work that a role already owns just because it's faster to do it himself. That's the anti-pattern that reverses the entire premise.

### Jules's cognitive budget

Ten-minutes-a-day is the target for pure ops (dashboard scan + approvals).
Rest of working hours = strategy, sales calls, product direction, skill injection, customer relationships.

If Jules is spending >30 min/day on ops work, a role is missing or a gate is wrong.

## Approval rhythm (Phase 1)

- **Morning (10 min):** scan Paperclip dashboard. Clear approval queue. Anything gated — review + approve or reject with a one-line reason.
- **Afternoon (5 min):** scan Head of Marketing's pending approval queue (LinkedIn posts, DMs, newsletter).
- **Evening (5 min):** read CEO's end-of-day DIGEST if posted; decide if anything needs board-level action tomorrow.

Total: ~20 min/day in Phase 1. Target: <10 min/day by Phase 3.

## Red flags that mean Jules should intervene

- Auditor trend on any agent dropping for 3+ consecutive days.
- CRM Steward flagging schema drift Jules didn't initiate.
- Budget >80% on any agent with no obvious output justification.
- Paperclip dashboard showing `error` status on any agent.
- Client Bot stuck in a stage past SLA (from PM's weekly DIGEST).
- Growth Analyst showing declining top-of-funnel or pipeline conversion for 2+ weeks.
- A Paperclip-server or Composio error affecting multiple agents.

Any of these → Jules investigates and either fixes it himself or updates the relevant agent's configuration.

## When Jules goes on vacation

Phase 1–2: don't. The company isn't autonomous enough yet.
Phase 3: yes, for up to a week. CEO agent handles the daily rhythm. Budget caps prevent runaway spend. Approval gates queue work that needs Jules. On return, Jules clears the queue.
