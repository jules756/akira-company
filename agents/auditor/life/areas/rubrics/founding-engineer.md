# Rubric — Founding Engineer

Scored daily by Auditor. Each dimension 0–5.

### 1. Shipping velocity (0–5)
- **5** — Features land on time vs. the CTO-approved plan. No undisclosed blockers aging.
- **3** — 1–2 blockers aging.
- **1** — Multiple stuck items.
- **0** — Nothing shipped this week.

### 2. TDD + verification discipline (0–5)
- **5** — Every non-trivial change uses `test-driven-development` and `verification-before-completion`. Passing tests = actually passing.
- **3** — Tests exist but coverage gaps.
- **1** — Shipping without tests.
- **0** — No test discipline.

### 3. Skill reuse + building (0–5)
- **5** — Repeat patterns get extracted into reusable skills (e.g., email-skill to replace n8n). Library compounds.
- **3** — Some patterns reused; no new skills built.
- **1** — Copy-pasting instead of extracting.
- **0** — No skill hygiene.

### 4. Hand-off quality to Implementation Engineer (0–5)
- **5** — Core-platform features ship with integration docs, example configs, deployment notes. Implementation Engineer can use them without re-engineering.
- **3** — Docs thin but code readable.
- **1** — Implementation Engineer has to reverse-engineer.
- **0** — Hand-off broken.

### 5. Magnet builds (when tasked) (0–5)
- **5** — Magnets needing backend logic ship on Vercel + Supabase within spec. Deployed, versioned, documented.
- **3** — Ships but over budget.
- **1** — Stalls.
- **0** — Not picked up.
