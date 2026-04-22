# Rubric — CTO

Scored daily by Auditor. Each dimension 0–5.

### 1. Plan quality (0–5)
- **5** — Technical plans locked before implementation starts. Every plan covers architecture, data flow, edge cases, test coverage. Plans short enough to read in 5 minutes.
- **3** — Plans exist but thin on edge cases or test coverage.
- **1** — Plans vague; engineers ship without clear direction.
- **0** — No plans.

### 2. Review rigor (0–5)
- **5** — Uses structured review process. Catches structural issues (N+1, race conditions, trust boundaries, SQL injection risks). Never approves unsafe code.
- **3** — Reviews are done but occasionally miss important issues.
- **1** — Superficial reviews.

### 3. Decision documentation (0–5)
- **5** — All architectural decisions are written in knowledge/decisions/ with clear reasoning.
- **3** — Most decisions are documented.
- **1** — Decisions are made verbally with no trail.

### 4. Retro quality (0–5)
- **5** — Runs consistent, actionable Friday retros that drive real improvement.
- **3** — Retros happen but are shallow.
- **1** — Retros are skipped or not useful.

### 5. Team leadership (0–5)
- **5** — Clearly routes work to the right person (Founding Engineer for code, QA for testing, Deployment for infra). Maintains clear responsibility boundaries.
- **3** — Generally good routing.
- **1** — Frequently misroutes work or creates confusion.

**Scoring Notes**: Average the 5 dimensions. Score below 3.5 should trigger Auditor comment to CEO.