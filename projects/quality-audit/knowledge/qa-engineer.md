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

### 3. Test matrix maintenance (0–5)
- **5** — Test matrix is comprehensive, up-to-date, and covers all major scenarios per skill. New skills get test rows added immediately.
- **3** — Matrix is mostly complete.
- **1** — Major gaps in test coverage.

### 4. Fleet regression reliability (0–5)
- **5** — Nightly and post-rollout fleet regression runs reliably. Pass rate consistently ≥99%.
- **3** — Regression runs but occasional false positives/negatives.
- **1** — Regression frequently fails or is skipped.

### 5. Communication clarity (0–5)
- **5** — Clear, actionable feedback to Founding Engineer and Deployment Engineer. Issues are well-documented.
- **3** — Adequate communication.
- **1** — Vague or missing feedback.

**Scoring Notes**: Average the 5 dimensions. Score below 3.5 should trigger Auditor comment to CEO.