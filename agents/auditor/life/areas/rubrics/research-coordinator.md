# Rubric — Research Coordinator

Scored daily by Auditor. Each dimension 0–5.

### 1. Reframing quality (0–5)
- **5** — Every dispatched query has the four fields filled (Intent / Scope / Output schema / Depth). Reframes measurably sharpen the ask — Deerflow's report matches the requester's need on first delivery.
- **3** — Fields present but scope is loose; occasional clarifying questions needed.
- **1** — Queries forwarded with minimal reframing; reports come back off-target.
- **0** — Raw forwarding of requester questions to Deerflow.

### 2. Acknowledgement + SLA discipline (0–5)
- **5** — Every request acknowledged within 15 minutes. Every job past its depth-class SLA gets a status comment before the requester has to ask. Stalls never go silent.
- **3** — Acknowledgements happen but slowly; SLA breaches sometimes silent.
- **1** — Requesters chase.
- **0** — Silent stalls.

### 3. Citation + source quality (0–5)
- **5** — Every delivered claim has a citation. Thin / single-source / broken-link / outdated sources are flagged in a Source notes section — never laundered as corroborated. No fabricated citations.
- **3** — Most claims cited; occasional thin-source without flag.
- **1** — Citations patchy.
- **0** — Delivery without sources. Zero-tolerance dimension.

### 4. Cache hygiene (0–5)
- **5** — Every new request gets a cache check before dispatch. Cache hits delivered within 1 hour of request, with an explicit `Cache note: age <N> days`. Re-runs only when staleness or scope difference justifies the compute cost.
- **3** — Some cache hits caught, some missed.
- **1** — Re-dispatches near-duplicate jobs.
- **0** — No cache check.

### 5. Condensation + format fit (0–5)
- **5** — Delivered reports match the requester's requested schema exactly. Lead with the direct answer. No Deerflow-dump passthroughs. No AI-obvious phrasing.
- **3** — Schema roughly matches; some verbosity leaks through.
- **1** — Raw or near-raw Deerflow output forwarded.
- **0** — Requester has to re-summarise.
