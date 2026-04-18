---
name: deerflow-invocation
slug: deerflow-invocation
key: akira-agent/deerflow-invocation
description: "Use when dispatching, polling, or retrieving reports from the Deerflow deep-research engine running on an external Akira VM. Wraps the HTTP API (POST /jobs, GET /jobs/{id}, GET /jobs/{id}/report) with the exact payload shape, auth, and error handling. Used by research-coordinator — no other agent calls Deerflow directly."
metadata:
  sources:
    - kind: internal
      path: skills/akira-agent/deerflow-invocation
      repo: jules756/akira-company
      trackingRef: main
---

# Deerflow Invocation

Deerflow is an open-source deep-research framework (<https://github.com/bytedance/deer-flow>) running on a dedicated Akira VM. This skill wraps the three API calls the research-coordinator needs: dispatch a job, poll status, fetch the report.

**Only `research-coordinator` invokes Deerflow.** Other agents route their research asks through the coordinator — never call the API directly.

## Env required

- `DEERFLOW_API_URL` — base URL of the Deerflow VM (e.g. `https://deerflow.akira-agent.internal` or the Hetzner-assigned IP with HTTPS).
- `DEERFLOW_API_KEY` — bearer token for auth.

## Dispatch a job

```http
POST {DEERFLOW_API_URL}/jobs
Authorization: Bearer {DEERFLOW_API_KEY}
Content-Type: application/json

{
  "query": "<reframed query string — full context, not an abbreviation>",
  "depth": "quick" | "standard" | "deep",
  "output_schema": "bullets" | "table" | "long-form" | "citation-dump",
  "metadata": {
    "paperclip_issue_id": "<issueId>",
    "requester_agent": "<agent-slug>",
    "requester_agent_id": "<paperclip-agent-id>",
    "submitted_at": "<ISO-8601>"
  }
}
```

**Response:** `202 Accepted`

```json
{
  "id": "<job-uuid>",
  "state": "queued",
  "estimated_runtime_sec": 480
}
```

Save `id` into `research-queue.md` + post the job id on the originating Paperclip issue.

## Poll job status

```http
GET {DEERFLOW_API_URL}/jobs/{job-id}
Authorization: Bearer {DEERFLOW_API_KEY}
```

**Response:**

```json
{
  "id": "<job-uuid>",
  "state": "queued" | "running" | "synthesising" | "completed" | "failed",
  "started_at": "<ISO-8601 or null>",
  "progress": 0.0 - 1.0,
  "error": "<string if failed, else null>"
}
```

Advance the state in `research-queue.md`. When `state == "completed"`, proceed to fetch report.

## Fetch the report

```http
GET {DEERFLOW_API_URL}/jobs/{job-id}/report
Authorization: Bearer {DEERFLOW_API_KEY}
```

**Response:**

```json
{
  "id": "<job-uuid>",
  "query": "<original reframed query>",
  "depth": "<class>",
  "output_schema": "<schema>",
  "runtime_sec": 742,
  "summary": "<one-paragraph TL;DR>",
  "content": "<full report in the requested schema — markdown>",
  "sources": [
    { "url": "<url>", "title": "<title>", "date": "<ISO-8601 or null>", "snippet": "<what was cited>" }
  ],
  "confidence_flags": [
    { "claim": "<short claim>", "note": "single-source | outdated | broken-link | etc." }
  ]
}
```

Condense into the requester's preferred delivery format per [`../../../agents/research-coordinator/HEARTBEAT.md`](../../../agents/research-coordinator/HEARTBEAT.md) §7.

## Error handling

- **401 Unauthorized** — `DEERFLOW_API_KEY` wrong or rotated. Comment the failure, set `blocked`, ping CTO.
- **404 on `/jobs/{id}`** — job was purged (Deerflow retention is 30 days). Treat as lost; re-dispatch if still needed.
- **5xx** — Deerflow health issue. Retry twice with backoff (10s, 60s). If still failing: open a `deerflow-outage` issue for CTO with timestamps. Resume polling when health returns.
- **Unreachable (connection refused / timeout)** — same as 5xx.
- **`state: failed`** on a job — read the `error` field. Most common causes:
  - "query too broad" → split into narrower sub-queries.
  - "no sources found" → scope or timeframe too tight; broaden.
  - "rate-limited upstream" → wait 15 min, retry unchanged.

Log every failure + resolution to [`../../../agents/research-coordinator/life/areas/research-queue.md`](../../../agents/research-coordinator/life/areas/research-queue.md) row `notes` field.

## Depth-class expectations

| Depth | Max sources | Typical runtime | Use for |
|---|---|---|---|
| `quick` | 5 | 1–5 min | Specific fact, single-source-ok queries |
| `standard` | 20 | 5–15 min | Scoped topic with multi-source triangulation |
| `deep` | 50+ | 15–45 min | Wide topic, long-form report, multiple angles |

Queries expected to exceed 45 min are too broad — split them.

## Job cancellation

```http
DELETE {DEERFLOW_API_URL}/jobs/{job-id}
Authorization: Bearer {DEERFLOW_API_KEY}
```

Rare — only use when a newer duplicate request invalidates an in-flight job, or when the requester explicitly cancels.

## Health endpoint

```http
GET {DEERFLOW_API_URL}/health
```

Returns 200 with `{"status": "ok", "version": "<v>"}` when healthy. Research-coordinator hits this at the start of each heartbeat; failure routes to CTO.
