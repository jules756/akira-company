# Delivery Playbook — Product Manager

> Canonical SLAs, templates, and escalation rules for client Bot delivery. Updated as we learn from real deliveries.
>
> **Last updated:** 2026-04-15 (seed)

## SLAs (seed — adjust as we collect data)

| Stage | Target duration | Escalation if exceeded |
|---|---|---|
| Proposal → Building | 3 days after signed Proposal | Escalate to CTO — plan missing |
| Building → Feedback | 14 days | Escalate to CTO — Implementation stuck |
| Feedback → Production | 10 days from feedback session | Escalate to CTO + Head of Sales |
| Production → Maintenance | 60 days (routine) | — |

## Aging thresholds on subtasks

- 4 days silent → comment on the subtask asking for status.
- 7 days silent → escalate to CTO (for Engineering subtasks) or Head of Marketing (for UX subtasks).
- 14 days silent → flag to CEO in the weekly portfolio DIGEST.

## Bot page documentation template

```
## Scope
(extracted from Proposal body — what we committed to deliver)

## Tech Stack
(Vapi + Twilio for voice, Cal.com for booking, etc. Match Tech Stack Tags field.)

## Build Plan
Link to CTO plan: <Paperclip issue URL>
Summary: <one paragraph>

## Implementation Checklist
- [ ] Twilio number provisioned (SE / NO?)
- [ ] Vapi agent configured (system prompt, voice, fallback)
- [ ] Cal.com event types wired
- [ ] Email automation (n8n OR email-skill)
- [ ] Monitoring alerts live
- [ ] Staging test passed

## Timeline
- Proposal signed: YYYY-MM-DD
- Build started: YYYY-MM-DD
- Feedback session: YYYY-MM-DD
- Production live: YYYY-MM-DD

## Feedback Log
(Append-only. Format: `YYYY-MM-DD — <who raised> — <item> — <disposition>`.)

## Incidents
(Link to Implementation Engineer's incidents.md. Summarize key ones here.)

## Client Summary (latest)
(Three paragraphs Jules can forward as-is.)
```

## Client-facing summary template (what you write for Jules to forward)

Paragraph 1 — **What we built.** Plain-language description tied to the problem the Proposal named. No tech jargon unless the client is technical.

Paragraph 2 — **How to review.** Concrete action: "Call +46-X between these hours to hear the agent; here's what you'll notice; here's a sample conversation."

Paragraph 3 — **What's next.** Dates + who owns each next step.

## Subtask templates

### To CTO (plan request)
```
Title: Plan: <Client Name> Bot build
Description:
  - Bot page: <URL>
  - Proposal: <URL>
  - Scope summary: <1-2 sentences>
  - Tech stack signals: <from Proposal + Company CRM>
  - Deadline: <SLA-based>
```

### To Implementation Engineer (setup)
```
Title: Configure <Client Name> voice bot
Description:
  - Bot page: <URL>
  - CTO plan: <URL>
  - Twilio region: SE / NO
  - Language: Swedish / English / both
  - Booking integration: Cal.com / other
  - Deadline: <SLA>
```

### To UX Designer (client dashboard + voice UX review)
```
Title: Design <Client Name> client dashboard + voice UX review
Description:
  - Bot page: <URL>
  - Implementation Engineer's Vapi prompt: <link when available>
  - Brand/language preference: <from Company CRM>
  - Deadline: <SLA>
```

### To Founding Engineer (platform escalation)
```
Title: Platform request from <Client Name> delivery: <specific pattern>
Description:
  - Bot page: <URL>
  - What Implementation Engineer tried as a per-client workaround: <summary>
  - Why it should be platform work: <reasoning>
  - Urgency: <non-blocking / next-sprint / blocks this delivery>
```

## Client Summary escalation rules

- Send draft to Head of Sales first — they approve before Jules forwards to client.
- If Head of Sales doesn't respond in 24h → escalate to CEO.
- Never send a summary to a client directly. Always via Jules.

## Known Bot naming convention

`<Client Name> — <Purpose>` e.g., `Dansken — Reservation Voice Bot`. One Bot per (client × purpose). Multi-purpose deployments get multiple Bot rows.
