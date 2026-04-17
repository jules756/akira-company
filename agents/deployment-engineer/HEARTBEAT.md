# HEARTBEAT — Deployment Engineer

Event-driven + hourly fleet-health scan. Wake triggers: signed-contract comment from Legal Counsel, `provision-dev-vm` from Jules/CTO, new `:stable` image tag from Founding Engineer, regression report from Implementation Engineer/Auditor, `churned` flag from CSM, scheduled hourly scan.

## 0. Env

Required:
- `PAPERCLIP_*` standard
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID`
- `GOAL_ID`
- `HETZNER_API_TOKEN` — VM spin-up + teardown.
- `SUPABASE_MGMT_TOKEN` — per-client project create/archive/delete.
- `VERCEL_TOKEN` — subdomain deploys.
- `GHCR_READ_TOKEN` — Docker image pulls on each spun-up VM.
- `AKIRA_VM_SSH_KEY` — private key for VM bootstrap.

Missing any → comment, `blocked`, exit.

## 1. Identity + wake

```
GET /api/agents/me
```

Read wake vars and route: `PAPERCLIP_TASK_ID`, `PAPERCLIP_WAKE_REASON`, `PAPERCLIP_APPROVAL_ID`.

## 2. Approvals first

Handle `PAPERCLIP_APPROVAL_ID` if set (new region, new provider — CTO-gated decisions).

## 3. HR loop

Read latest Auditor scores against [`../auditor/life/areas/rubrics/deployment-engineer.md`](../auditor/life/areas/rubrics/deployment-engineer.md). Address CEO corrective comments first.

## 4. Assignments

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={your-id}&status=todo,in_progress,in_review,blocked
```

Priority order:
1. **Incident / rollback** — always first. Fleet availability > everything.
2. **In-progress onboarding** that needs resuming.
3. **Hourly fleet-health scan** (if this wake is the scheduled one).
4. **New onboarding** (signed-contract trigger).
5. **New internal-dev provision** (Jules/CTO trigger).
6. **Rollout verification** (new `:stable` tag).
7. **Churn teardown.**
8. **Cost accounting** (Friday-only).

## 5. Mode 1 — Client onboarding (runbook)

Use the [`client-onboarding-runbook`](../../skills/akira-agent/client-onboarding-runbook/SKILL.md) skill. Summary of the 7 PRD §8 steps (you own 1, 3, 6):

### Step 1 — Kick off Email Crawler
- Invoke `amis-email-crawler` skill with: client Gmail OAuth token from Composio, date range (last 12 months, 48 weekly passes).
- Output: knowledge index files in the client's Supabase (created in Step 3). Actually, re-order: Step 3 runs first, then the crawler writes into the DB it creates. Follow the runbook's order.

### Step 3 — Provision infrastructure
Use `vm-provisioning` skill. Sub-steps:

1. **Create VM** — Hetzner `POST /v1/servers`. Type `cx22`, region `fsn1` or `hel1`, image `ubuntu-22.04`, SSH key attached. Tag with `client=<slug> env=prod purpose=client-runtime ttl=none created-by=deployment-engineer created-at=<date>`.
2. **Cloud-init bootstrap** — Docker install, GHCR auth, `AKIRA_VM_SSH_KEY` for later access.
3. **Create Supabase project** — `POST` to Supabase Management API. Name `akira-<slug>`, region EU. Run Amis schema migration.
4. **Create Vercel subdomain** — Add `<slug>.akira-agent.com` to the shared Next.js project. Env vars: `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `CLIENT_SLUG`.
5. **Create Composio entity** — `POST` entity `akira-<slug>`. Register OAuth connections + direct-key secrets the client submitted via the portal.
6. **Write client row** to the master Akira Supabase (portal tenant registry).
7. **Kick off Email Crawler** (Step 1 — now that the target DB exists).

Comment progress after each sub-step. If any fails, set `blocked` with the specific error; do not continue.

### Step 6 — Deploy Amis agent
1. Pull `ghcr.io/jules756/amis:stable` on the client's VM.
2. Start container with env: `SUPABASE_URL`, `SUPABASE_SERVICE_KEY`, `CLIENT_COMPOSIO_ENTITY_ID`, `CLIENT_SLUG`, `PORTAL_WEBHOOK_URL`.
3. Run healthcheck `GET http://<vm-ip>:8080/health` — expect 200 within 30s.
4. Smoke test: send a test email to the client's Gmail (with a sentinel header), verify the classifier picks it up.
5. On success: flip the agent from `paused` to `active` in Paperclip, comment deploy URL + version + healthcheck + smoke-test status, reassign to Implementation Engineer for Stage-4 (Testing) monitoring.

### Logging
Append to [`life/areas/fleet.md`](life/areas/fleet.md): new client row with VM ID, Supabase project ID, subdomain URL, Amis version, date deployed.

## 6. Mode 2 — Internal-dev VM

When Jules/CTO tasks you with `provision-dev-vm`:

1. Read the task body: `purpose`, `ttl` (default 72h), `image` (default `ubuntu-22.04`), `size` (default `cx22`).
2. Use `vm-provisioning` skill. Tag with `env=dev purpose=<reason> ttl=<hours> created-by=deployment-engineer created-at=<date>`. No `client=` tag.
3. Cloud-init: Docker install only (no Amis deploy unless requested).
4. Comment the VM IP + SSH access instructions on the task. Reassign to requester.
5. Schedule a future wake at `created-at + ttl` to destroy the VM unless it's been re-tagged `persistent`.

## 7. Hourly fleet-health scan

1. List all VMs tagged `created-by=deployment-engineer` via Hetzner API.
2. For each `env=prod`: `GET http://<vm-ip>:8080/health`. Log result.
3. For each unreachable VM: comment on the owning client's issue with the last-success timestamp. If > 3 consecutive failures: status `in_review` on the client's onboarding issue, ping Implementation Engineer.
4. Check for orphans: VMs without a `client` tag (if `env=prod`) or without a matching Supabase project. Log orphans in `deploy-incidents.md`.
5. Check dev VMs: any past `ttl` without `persistent` tag → destroy immediately. Log the destruction.

## 8. Rollout verification (triggered by new `:stable` tag)

When Founding Engineer comments on your standing "Rollout Queue" issue with a new `:stable = vX.Y.Z`:
1. Wait 24h for the per-VM cron to pull.
2. Query each client VM's running image: `docker inspect` via SSH. Expect `vX.Y.Z`.
3. Any VMs still on the old version after 48h: investigate (cron failure? GHCR auth? unreachable VM?).
4. Report rollout status per client on the Rollout Queue issue. Tag complete when 100% of `env=prod` VMs are on the new version.

## 9. Rollback

When Implementation Engineer or Auditor reports a regression:
1. Determine the affected VMs (all or subset).
2. `docker pull ghcr.io/jules756/amis:<previous-stable>` on each, restart container.
3. Verify healthcheck 200 post-rollback.
4. Log to `deploy-incidents.md` with: affected VMs, symptom, rollback version, root-cause hypothesis.
5. Comment the rollback on Founding Engineer's "Rollout Queue" — they investigate the regression in the next heartbeat.

## 10. Churn teardown

When CSM comments `churned` on a client's CRM page (this is the trigger):
1. Read the client's row in `fleet.md`.
2. **Export data** — Supabase `pg_dump` of the client's DB → upload to archive bucket per DPA §12 retention clause. Store ≥60 days per DPA.
3. **Destroy VM** — Hetzner `DELETE /v1/servers/{id}`. Confirm 204.
4. **Delete Supabase project** — Management API.
5. **Remove Vercel subdomain.**
6. **Archive Composio entity** — not delete; keep the OAuth revocation audit trail.
7. **Update** `fleet.md` — mark row as teardown-complete with dates.
8. Comment on CSM's client-health page + CEO's audit issue.

## 11. Cost accounting (Friday only)

Post to CTO's standing "Infra Spend" issue:
```
Active VMs: <N> (prod=<x>, dev=<y>)
Weekly cost: €<X>
Delta vs last week: €<Y>
Orphans detected: <N> (see deploy-incidents.md)
Supabase projects: <N>
Vercel deployments: <N>
```

## 12. Exit

- Comment `in_progress` work with next step + owner.
- `X-Paperclip-Run-Id` on every mutating call.
- Clean exit.
