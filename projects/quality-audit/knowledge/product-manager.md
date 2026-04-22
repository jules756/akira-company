# Rubric — Product Manager (Client Delivery)

Scored daily by Auditor. Each dimension 0–5.

### 1. Active-client coverage (0–5)
- **5** — Every Notion CRM Company at `Pipeline Status=Active` has a corresponding row in `Agents (The Bots)` with non-null Build Status. Daily scan catches gaps within one heartbeat.
- **3** — ≤1 active client missing a Bot row.
- **1** — Multiple active clients untracked.
- **0** — Tracker broken.

### 2. Build Status accuracy (0–5)
- **5** — Every Bot's Build Status reflects reality. Transitions happen within 1 heartbeat of Implementation Engineer / UX Designer handoff. No stale Production rows that are actually broken.
- **3** — 1–2 stale statuses.
- **1** — Frequent drift.
- **0** — Statuses meaningless.

### 3. Documentation quality per bot (0–5)
- **5** — Every Bot page body contains: scope (from Proposal), tech stack, build plan, deployment notes, feedback log. Updated on every status change.
- **3** — Docs exist but outdated on some Bots.
- **1** — Docs stub-only.
- **0** — No documentation.

### 4. Cross-functional coordination (0–5)
- **5** — PM is the connective tissue. Subtasks to CTO (plans), Implementation Engineer (configs), UX Designer (designs) are specific, deadline-tagged, and tracked to completion.
- **3** — Coordination works but some balls dropped.
- **1** — Multiple balls dropped per week.
- **0** — Team disconnected.

### 5. Client-facing handoffs (0–5)
- **5** — When a Bot moves to Feedback or Production, PM writes a client-ready summary (scope, what's built, what to review). Jules can forward with no editing.
- **3** — Summaries need tuning.
- **1** — Technical, not client-ready.
- **0** — No summaries.
