# HEARTBEAT — Research Coordinator

Event-driven + hourly sweep of pending Deerflow jobs. Wake triggers: task assignment, `@mention`, scheduled sweep.

## 0. Env

Required:
- `PAPERCLIP_*` standard
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID`
- `GOAL_ID`
- `DEERFLOW_API_URL` — base URL of the Deerflow VM (e.g. `https://deerflow.akira-agent.internal`).
- `DEERFLOW_API_KEY` — bearer token.

Missing → comment, `blocked`, exit.

## 1. Identity + wake

```
GET /api/agents/me
```

Read wake vars: `PAPERCLIP_TASK_ID`, `PAPERCLIP_WAKE_REASON`, `PAPERCLIP_WAKE_COMMENT_ID`.

## 2. Approvals

Handle `PAPERCLIP_APPROVAL_ID` if set (rarely relevant — approvals flow to CEO).

## 3. HR loop

Scan latest Auditor scores against [`../auditor/life/areas/rubrics/research-coordinator.md`](../auditor/life/areas/rubrics/research-coordinator.md). CEO corrective comments → address this heartbeat.

## 4. Assignments + queue sweep

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={your-id}&status=todo,in_progress,in_review,blocked
```

Plus: read [`life/areas/research-queue.md`](life/areas/research-queue.md) for jobs in state `queued | running | synthesising`.

Priority:
1. **Stalled jobs past SLA** — comment on the originating issue + re-check Deerflow health.
2. **`synthesising` jobs** — Deerflow has finished compute; condense + deliver.
3. **`running` jobs** — poll status; update state.
4. **New `todo` research requests** — reframe + dispatch.
5. **`in_review` deliveries awaiting requester follow-up** — pass (their move).

## 5. New-request mode (PRD Steps 1–5 of the 7-step canonical flow)

When a new request arrives:

### 5a. Acknowledge (≤15 minutes)
Comment on the originating issue:
```
Received. Estimated depth: <quick|standard|deep>. Expected delivery: <HH:MM> CET.
```

### 5b. Reframe
Open [`life/areas/deerflow-prompts.md`](life/areas/deerflow-prompts.md). Find the matching template (by requester role or by request type). Fill in:
- **Intent:** <what the requester will do with the result>
- **Scope:** industry + region + timeframe + source types
- **Output schema:** <bullets | table | long-form | citation-dump>
- **Depth:** <quick | standard | deep>

If the request is too ambiguous to reframe (rare — most are scopeable), comment ONE clarifying question and wait.

### 5c. Cache check
Search `research-queue.md` for jobs in `delivered` state whose reframed query is near-match and delivery date is within the freshness window for the topic class:
- News / SEO signals / algorithm changes: 7 days.
- Company profiles / industry trends: 30 days.
- Foundational research / established facts: 90 days.

If a cache hit exists:
- Deliver the cached report immediately with a `Cache note: delivered from <date>, age <N> days` line.
- Append a new row to `research-queue.md` with `cache-reuse: <original-job-id>`.
- Skip to §7 (queue cleanup).

### 5d. Dispatch
Call the Deerflow API using the [`deerflow-invocation`](../../skills/akira-agent/deerflow-invocation/SKILL.md) skill:

```
POST {DEERFLOW_API_URL}/jobs
Authorization: Bearer {DEERFLOW_API_KEY}
Content-Type: application/json

{
  "query": "<reframed query>",
  "depth": "<quick|standard|deep>",
  "output_schema": "<schema>",
  "metadata": {
    "paperclip_issue_id": "<issueId>",
    "requester_agent": "<agent-slug>",
    "requester_agent_id": "<paperclip-agent-id>"
  }
}
```

Grab `job.id` from the 202 response. Log to `research-queue.md`:

```
- <job-id> | <requester> | issue:<issueId> | depth:<depth> | dispatched:<ISO-8601> | state:queued
```

### 5e. Update issue
Comment on the originating issue:
```
Dispatched. Deerflow job: <job-id>. Depth: <class>. Polling every 5 minutes.
```

## 6. Poll mode (hourly sweep — runs every wake)

For every `research-queue.md` row in state `queued | running | synthesising`:

```
GET {DEERFLOW_API_URL}/jobs/{job-id}
Authorization: Bearer {DEERFLOW_API_KEY}
```

Update state from the response. Advance via:
- `queued` → `running` once Deerflow picks it up.
- `running` → `synthesising` when compute finishes, final write is in progress.
- `synthesising` → proceed to §7 Deliver.
- Any → `failed` → comment error on originating issue; ask requester whether to retry.

**SLA check:** if a job is past its depth-class budget (`quick` 5m, `standard` 20m, `deep` 45m) and still `running` or `queued`, post a status comment on the originating issue:
```
Still running (past the <class> SLA). Will deliver when complete; no action needed.
```

If Deerflow's API is unreachable (non-2xx for > 2 consecutive polls): open an issue for CTO tagged `deerflow-outage` with timestamps and error codes. Keep polling; resume normal delivery when healthy.

## 7. Deliver mode

Fetch the completed report:

```
GET {DEERFLOW_API_URL}/jobs/{job-id}/report
```

Parse the response. Condense into the requester's preferred schema. Write the delivery comment on the originating issue:

```
Research delivered: <one-line answer or summary>

<main content in the requested format — bullets | table | prose | citations>

Sources:
- <url1> — <one-line what it's citing>
- <url2> — <...>

<optional> Source notes:
- <flagged broken link / thin-source claim / outdated source>

Deerflow job: <job-id>
Depth: <class>
Runtime: <duration>
Cache: <fresh | reuse-of-<original-job-id>>
```

Set status → `in_review` (requester may have follow-up). Mark `research-queue.md` row as `delivered: <ISO-8601>`.

## 8. Queue cleanup

Weekly (Fridays): prune `research-queue.md` entries older than 90 days, archive to `life/areas/research-queue-archive.md` for audit.

## 9. Exit

- Comment `in_progress` work.
- `X-Paperclip-Run-Id` on mutating calls.
- Clean exit.
