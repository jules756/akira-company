# Rubric — QA Engineer

Scored daily by Auditor. Each dimension 0–5.

### 1. Promotion-gate discipline (0–5)
- **5** — Every `:candidate` → `:stable` promotion has a QA green verdict documented on the Release Queue. No promotions slipped through untested. Average time-to-verdict ≤ 4 hours.
- **3** — Gate is enforced but slow (>12h average).
- **1** — Promotions have happened without QA verdict.
- **0** — Gate bypassed repeatedly. Zero-tolerance dimension.

### 2. Bug reproduction quality (0–5)
- **5** — Every Implementation-Engineer-reported bug gets a reproducible failing test in the matrix within 24h. Founding Engineer has everything they need to start the fix without follow-up questions.
- **3** — Most bugs reproduced; some handed back thin.
- **1** — Repro steps incomplete; Founding Engineer has to re-investigate.
- **0** — Bug reports forwarded without reproduction work.

### 3. Fleet regression coverage (0–5)
- **5** — Every rollout triggers a fleet regression run within 1 hour of "rollout complete." Nightly regression runs 7/7 days. Rollback happens within the same heartbeat when regression fails.
- **3** — Fleet regression runs but with delay or occasional gaps.
- **1** — Rolls out without regression verification.
- **0** — No fleet regression.

### 4. Test matrix hygiene (0–5)
- **5** — Matrix stays current with shipping skills. Every new skill has coverage before its first `:candidate`. Every production bug converts to a permanent regression row. Flakes are fixed, not ignored.
- **3** — Matrix lags 1–2 skills; flakes exist but are triaged.
- **1** — Matrix stale; flakes hide real regressions.
- **0** — Matrix untouched while skills evolve.

### 5. Separation of concerns from Auditor + Founding Engineer (0–5)
- **5** — QA tests code; never edits skill code; never scores agent behaviour. Clear handoffs to Founding Engineer (bugs) and Deployment Engineer (regression-triggered rollback). Auditor and QA boundaries are crisp.
- **3** — Slight scope drift (e.g., suggesting a code fix instead of a test).
- **1** — QA edits code or agent rubrics.
- **0** — QA conflated with Auditor role, confusing routing.
