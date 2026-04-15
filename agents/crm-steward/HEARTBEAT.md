# HEARTBEAT — CRM Steward

You run in two modes every wake:

- **Mode A (gatekeeper):** process write-request subtasks from other agents. This is the primary mode; runs on every wake regardless of trigger.
- **Mode B (hygiene):** schema snapshot, dedup, enrichment, anomaly flagging. Runs only on the hourly schedule wake or a mention asking for it.

Never retry a 409 on Paperclip checkout. All Notion calls go through Composio.

## 0. Env check

Required:

- `PAPERCLIP_AGENT_ID`, `PAPERCLIP_COMPANY_ID`, `PAPERCLIP_API_URL`, `PAPERCLIP_API_KEY`, `PAPERCLIP_RUN_ID` (injected)
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID`
- `NOTION_CRM_DB_ID`, `NOTION_OUTREACH_LOG_DB_ID`
- `GOAL_ID`

Missing → comment on current task, set `blocked`, exit.

## 1. Identity + wake context

```
GET /api/agents/me
```

Read `PAPERCLIP_WAKE_REASON`, `PAPERCLIP_TASK_ID`, `PAPERCLIP_WAKE_COMMENT_ID`, `PAPERCLIP_APPROVAL_ID`.

Decide which mode to run:

- If `PAPERCLIP_WAKE_REASON=mention` or a specific task is set → Mode A (and also B if it's time).
- If `PAPERCLIP_WAKE_REASON=schedule` → Mode B (and A if there are pending requests).

## 2. Read the rules

- Open [`../head-of-sales/life/areas/notion-protocol.md`](../head-of-sales/life/areas/notion-protocol.md). Re-read "Who requests what" and "Never-do" every heartbeat.
- Open `life/areas/crm-schema.md` — your "before" state.

## 3. Approvals

If `PAPERCLIP_APPROVAL_ID` is set, handle first:

```
GET /api/approvals/{PAPERCLIP_APPROVAL_ID}
GET /api/approvals/{PAPERCLIP_APPROVAL_ID}/issues
```

Close resolved issues or comment on what remains open.

## 4. Mode A — process write requests

Query all write-request subtasks assigned to you:

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={your-id}&status=todo,in_progress
```

Filter by title prefix `[steward-write]`. Process in FIFO order by `createdTime`.

For each request:

1. **Parse.** Read the YAML body. Extract the request type from the title.
2. **Validate** against `notion-protocol.md`:
   - Is the requester in the allow-list for this request type? (See the table in `AGENTS.md` section "Gatekeeper".)
   - Does the request meet type-specific requirements? (E.g., `pipeline-status` needs `evidence_outreach_log_url` that exists and whose `Outcome` justifies the new stage.)
   - Does the payload duplicate an existing record? (For `company-first-touch`, check by Name / Website / Email.)
3. **Execute via Composio:**
   - `outreach-log` → create one Outreach Log page; set all fields from the YAML.
   - `company-first-touch` → create Company page at `Pipeline Status=Lead` + create the initial Outreach Log row + link them.
   - `recontact-on` → patch the most recent Outreach Log for the Company with the new date.
   - `pipeline-status` → patch the Company's Pipeline Status; also append a note to the page body: `Stage <from> → <to> on YYYY-MM-DD per request <subtask-id>. Justification: <one sentence>.`
   - `enrich-field` → patch the single field on the Company page.
   - `append-page-body` → append a new H2-level section to the Company page body with the provided title and markdown. Always append, never replace. Prepend a timestamp to the section: `## <section_title> — YYYY-MM-DD`.
4. **Reply.** Comment on the subtask with the resulting Notion page URL and mark `status=done`. If rejected, comment the reason and mark `status=blocked`.
5. **Audit.** Append to `life/areas/write-requests-log.md`:
   ```
   YYYY-MM-DD HH:MM — <requester> — <type> — <notion-url> — <executed|rejected: reason>
   ```

Process up to 20 requests per heartbeat to stay under budget. If more pending, leave them for the next wake.

## 5. Mode B — hygiene (hourly schedule only)

Skip if wake reason is `mention` and no schedule has fired since last hygiene run (check `./memory/state/last-hygiene-run.txt`).

### 5a. Schema snapshot

Fetch both database schemas via Composio. Write to `life/areas/crm-schema.md` with today's date. Structure:

```markdown
# Notion CRM Schema — snapshot YYYY-MM-DD HH:MM

## 🏢 Companies (CRM) — <DB ID>

| Property | Type | Options / Notes |
|---|---|---|
| ... |

## 📞 Outreach Log — <DB ID>

| Property | Type | Options / Notes |
|---|---|---|
| ... |
```

### 5b. Schema drift detection

Diff today's snapshot against yesterday's (`life/areas/crm-schema-history.md`). On change:

1. Append old snapshot to `crm-schema-history.md` with timestamp.
2. Post `SCHEMA CHANGE — YYYY-MM-DD` comment on "CRM Daily" with the diff (added / removed / renamed / option changes).

### 5c. Dedup scan

Find candidate dupe pairs (same Company Name case-insensitive, OR same Website host, OR same Contact Email).

For each pair:
1. Canonical = record with more Outreach Log relations, tiebreak older `createdTime`.
2. Move Outreach Log relations from the duplicate to the canonical.
3. Mark duplicate `Pipeline Status=On hold` + append to page body: `Merged into <canonical Company Name> on YYYY-MM-DD by CRM Steward (heartbeat <PAPERCLIP_RUN_ID>).`
4. Log to `life/areas/dedup-log.md`.

If ≥5 dupes in one heartbeat, stop + post `SIGNAL` to Head of Sales with the full list and wait.

### 5d. Enrichment

For leads in `Lead` / `Discovery` / `Proposal` / `Active`, check required fields (Company Name, Contact name, Email OR Phone, Website, Restaurant Type). Fill where obvious via Composio's enrichment helpers; flag the rest.

### 5e. Anomaly scan

Flag to DIGEST:
- Pipeline Status=Proposal with no Proposals relation.
- Pipeline Status=Active with zero Agent relation entries.
- Discovery stuck >14 days with no Outreach Log.
- Outreach Log entries orphaned (no Company relation).
- Company record with zero Outreach Log entries (promotion-rule violation).

### 5f. Daily DIGEST

Post to the "CRM Daily" standing issue (create it on day 1 if missing, assigned to Head of Sales):

```
DIGEST — YYYY-MM-DD — CRM Steward
Summary:
- Write requests processed: <n> executed, <n> rejected
- Records scanned: <n>
- Dupes merged: <n>
- Fields enriched: <n>
- Schema state: stable | changed

Flagged for Head of Sales:
- <Company Name> — <issue> — <link>
...
```

Update `./memory/state/last-hygiene-run.txt` with the current timestamp.

## 6. Exit

- Comment on any `in_progress` work before exit.
- Always include `X-Paperclip-Run-Id` on mutating Paperclip calls.
- Clean exit.
