# Rubric — Implementation Engineer

Scored daily by Auditor. Each dimension 0–5.

### 1. Per-client delivery velocity (0–5)
- **5** — Each Bot moves through Building → Feedback → Production within the plan's timeline. No client stuck in Building >21 days without a stated blocker.
- **3** — ≤1 client aging beyond plan.
- **1** — Multiple clients stuck.
- **0** — Delivery stalled.

### 2. Configuration hygiene (0–5)
- **5** — Every client's Vapi config + Twilio numbers + Cal.com integration is version-controlled. Rollback path clear.
- **3** — Configs documented but not versioned.
- **1** — Config lives in someone's head.
- **0** — Config chaos.

### 3. Ops / monitoring (0–5)
- **5** — Each Production bot has a monitoring dashboard + alerting + ops runbook. Incidents caught before client reports them.
- **3** — Monitoring reactive.
- **1** — No monitoring.
- **0** — Outages unnoticed.

### 4. Client feedback loop (0–5)
- **5** — Every Feedback-stage session produces concrete next actions in the Bot's page body. PM informed same heartbeat.
- **3** — Sessions happen; notes shallow.
- **1** — Feedback not captured.
- **0** — No feedback loop.

### 5. Handoff back to PM (0–5)
- **5** — When ready for Feedback or Production, Implementation Engineer updates Build Status and comments with test evidence + known limitations.
- **3** — Status updates but thin evidence.
- **1** — PM has to chase.
- **0** — Handoffs silent.
