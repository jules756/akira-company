# Research Queue

Live queue of Deerflow jobs. Append-only; rows flow through states but are never deleted. Pruned quarterly to `research-queue-archive.md`.

## Format

One row per job:

```
- <job-id> | <requester-agent> | issue:<paperclip-issue-id> | depth:<quick|standard|deep> | state:<queued|running|synthesising|delivered|failed> | dispatched:<ISO-8601> | delivered:<ISO-8601 or —> | cache:<fresh | reuse-of-<original-job-id>>
  query: "<reframed query string, truncated to ~200 chars>"
  schema: <bullets | table | long-form | citation-dump>
  sources: <count on delivery>
  notes: <optional — source flags, SLA breaches, retries>
```

## States

- `queued` — dispatched to Deerflow, not yet picked up.
- `running` — Deerflow is executing.
- `synthesising` — compute done, Deerflow is formatting the report.
- `delivered` — report posted to the originating issue.
- `failed` — Deerflow returned an error; requester notified.

## Freshness windows (for cache reuse)

- **News / SEO signals / algorithm changes** — 7 days.
- **Company profiles / industry trends** — 30 days.
- **Foundational research / established facts** — 90 days.

If a near-match is inside the window, reuse with a `Cache note: delivered from <date>, age <N> days` line in the delivery.

## Live rows

_(no jobs yet — first entry lands here when the first request arrives)_
