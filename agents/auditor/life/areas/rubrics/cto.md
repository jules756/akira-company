# Rubric — CTO

Scored daily by Auditor. Each dimension 0–5.

### 1. Plan quality (0–5)
- **5** — Technical plans locked before implementation starts. Every plan covers architecture, data flow, edge cases, test coverage. Plans short enough to read in 5 minutes.
- **3** — Plans exist but thin on edge cases or test coverage.
- **1** — Plans vague; engineers ship without clear direction.
- **0** — No plans.

### 2. Review discipline (0–5)
- **5** — Every branch gets paranoid review (N+1s, race conditions, trust boundaries) before merge. Uses `review` skill.
- **3** — Reviews happen but skip depth occasionally.
- **1** — Review is style-nitpick, not structural.
- **0** — No review gate.

### 3. Retro cadence (0–5)
- **5** — Weekly engineering retro shipped every Friday. Identifies patterns, praises specific work, surfaces blockers.
- **3** — Retros happen inconsistently.
- **1** — Retros skipped.
- **0** — Never.

### 4. Delegation + PM coordination (0–5)
- **5** — Every delivery issue from PM gets a plan within 1 heartbeat. Implementation Engineer + Founding Engineer always know their next task.
- **3** — Some PM requests age >48h.
- **1** — PM frequently blocked by missing plans.
- **0** — PM working in a vacuum.

### 5. Decision documentation (0–5)
- **5** — Architectural decisions written to `life/areas/decisions/` with date, context, options considered, chosen path.
- **3** — Some decisions captured but inconsistent.
- **1** — Decisions live only in chat.
- **0** — No decision trail.
