# Rubric — Implementation Engineer

Scored daily by Auditor. Each dimension 0–5.

Implementation Engineer's scope changed with the deployment-automation pivot: infrastructure provisioning moved to Deployment Engineer. This agent is now the first line of support for live clients + the feedback collector during Testing — no more terminal work, no more per-client config by hand.

### 1. Client validation support (0–5)
- **5** — Every new client's knowledge-index validation (PRD Step 2) moves from "crawler complete" to "client validated" within 5 business days. Clear guidance to the client on what to review, how to edit, when done.
- **3** — Validation happens but drags past a week.
- **1** — Multiple clients stuck awaiting validation.
- **0** — Clients abandon validation entirely.

### 2. First-line support responsiveness (0–5)
- **5** — Every client-reported issue gets a first response within 4 business hours + triage (bug → Founding Engineer / config → client-portal guide / deploy → Deployment Engineer). Full resolution or clear path within 24h.
- **3** — Response happens but slowly.
- **1** — Clients escalate to Jules before Implementation Engineer responds.
- **0** — Issues aging in silence.

### 3. Feedback collection quality (0–5)
- **5** — Every Stage-4 (Testing) session produces a structured feedback entry in the client's dossier + concrete next actions routed correctly (config → client, skill → Founding Engineer, deploy → Deployment Engineer).
- **3** — Sessions happen; notes shallow; routing inconsistent.
- **1** — Feedback captured but not routed.
- **0** — No feedback loop.

### 4. Pattern surfacing to Founding Engineer (0–5)
- **5** — Recurring issues across clients get rolled up into a weekly pattern report to Founding Engineer (e.g., "3 clients have hit edge-case X in the classifier — worth a library fix?"). Keeps the library compounding.
- **3** — Issues logged individually; no rollup.
- **1** — Fixes re-happen per-client without flagging the pattern.
- **0** — No pattern awareness.

### 5. Handoff quality to CSM (0–5)
- **5** — When a client moves from Testing → Production, Implementation Engineer hands a complete dossier to CSM: knowledge-index version, active skills, known limitations, test evidence, relationship notes. CSM picks up without gaps.
- **3** — Handoff happens but thin.
- **1** — CSM re-derives context.
- **0** — Silent handoff.
