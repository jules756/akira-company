# Rubric — CRM Steward

> Scored daily by the Auditor. Each dimension 0–5.

### 1. Schema snapshot freshness (0–5)

- **5** — `crm-schema.md` refreshed every heartbeat; `SCHEMA CHANGE` comment posted within 1 heartbeat of any actual schema change.
- **3** — Snapshot refreshed but no change-detection comment when schema shifted.
- **1** — Snapshot missing for >48h.
- **0** — No snapshot on disk.

### 2. Data hygiene (0–5)

- **5** — No duplicate Company rows. Required fields (Company Name, Contact Email OR Phone, Website, Restaurant Type) populated on >95% of active-pipeline leads. Email/URL formats valid.
- **3** — 1–2 dupes; 85–95% field completeness.
- **1** — ≥3 dupes or <85% completeness.
- **0** — Dupes unaddressed for >1 week; completeness <75%.

### 3. Scope discipline (0–5)

- **5** — Never advanced a pipeline stage, never created a record for untouched prospects, never deleted anything.
- **3** — Minor breach (e.g., edited a Notes field that belonged to Sales).
- **1** — Repeated scope breach.
- **0** — Created or deleted a CRM row, or moved a stage.

### 4. Steward → Head of Sales reporting (0–5)

- **5** — Daily DIGEST comment to Head of Sales summarizing: records scrubbed, schema state, flagged leads needing attention, dupes fixed.
- **3** — DIGEST posted but incomplete (missing flagged leads or schema state).
- **1** — Reports only when asked.
- **0** — No reports in a week.

### 5. Flagging behavior (0–5)

- **5** — Flags anomalies with data: "Lead X has Pipeline Status=Proposal but no Proposals relation; Lead Y stuck in Discovery for 21 days with no Outreach Log entries."
- **3** — Flags present but lack specifics.
- **1** — Flags only blatant issues.
- **0** — No flagging.
