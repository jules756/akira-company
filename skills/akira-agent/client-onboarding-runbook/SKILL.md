---
name: client-onboarding-runbook
slug: client-onboarding-runbook
key: akira-agent/client-onboarding-runbook
description: "Use when onboarding a new Akira client from signed-contract to live Amis deployment. Mechanical 7-step runbook implementing Email Handler PRD §8 — Deployment Engineer owns steps 1, 3, 6 (provisioning + crawler + deploy); steps 2, 4, 5, 7 sit with the client, Implementation Engineer, or Jules. Use this skill when you see a `[client-onboard]` or `[contract-signed]` task, or when Legal Counsel has filed a signed contract and the client is ready to enter Building stage."
metadata:
  sources:
    - kind: internal
      path: skills/akira-agent/client-onboarding-runbook
      repo: jules756/akira-company
      trackingRef: main
---

# Client Onboarding Runbook

Implements Email Handler PRD §8. Deployment Engineer executes steps 1, 3, 6; the remaining steps belong to the client or other agents.

## Preconditions (check before starting)

- **Signed contract filed** in `Akira / Clients / <slug> / Contracts /` (Legal Counsel did this; verify the doc exists).
- **Proposal status** in Notion is `Contracted`.
- **Client slug** is decided (lowercase, hyphenated, matches CRM company_slug field).
- **Scope** — which Amis skills are active for this client — is on the Proposal.

If any precondition is missing, comment the gap back to the triggering issue and set `blocked`. Do not begin provisioning.

## The 7 steps (PRD §8)

| Step | Action | Owner | Automated? |
|---|---|---|---|
| 1 | Run email crawler (48 weeks) | Deployment Engineer | Yes — skill `amis-email-crawler` |
| 2 | Client reviews + validates knowledge index | Client | No — client portal |
| 3 | Spin up VM + Supabase + subdomain + Composio entity | Deployment Engineer | Yes — this runbook |
| 4 | Client fills config panel | Client | No — client portal |
| 5 | Register client ID + API keys in Composio | Jules | Semi — Deployment Engineer preps the entity; Jules authorises |
| 6 | Deploy agent with 4 Amis skills + integration | Deployment Engineer | Yes — this runbook |
| 7 | Agent goes live | — | Automatic once Step 6 healthcheck passes |

## Execute order

**Steps 3 → 1 → 2 → 4 → 5 → 6 → 7.**

Step 3 runs first because the Email Crawler (Step 1) needs the client's Supabase project to write into. The PRD lists them numerically; the execution order is dependency-driven.

## Step 3 — Provision infrastructure

Use the [`vm-provisioning`](../vm-provisioning/SKILL.md) skill.

1. **VM** — create with labels `client=<slug> env=prod purpose=client-runtime ttl=none`.
2. **Supabase project** — `POST` to `https://api.supabase.com/v1/projects` with `SUPABASE_MGMT_TOKEN`. Name: `akira-<slug>`. Region: EU. Plan: free tier to start (upgrade on usage).
   - Wait for `status=ACTIVE_HEALTHY` before continuing.
   - Run the Amis schema migration (SQL file shipped in the Amis image under `/opt/amis/schema/`).
3. **Vercel subdomain** — via Vercel REST API with `VERCEL_TOKEN`, add `<slug>.akira-agent.com` as a domain on the shared multi-tenant Next.js project. Set env vars: `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `CLIENT_SLUG`. Trigger a deploy.
4. **Composio entity** — create entity `akira-<slug>`. Note the entity ID; it becomes `CLIENT_COMPOSIO_ENTITY_ID` on the VM.
5. **Tenant registry** — insert a row into the master Akira Supabase `clients` table: `slug`, `vm_id`, `supabase_project_id`, `subdomain`, `composio_entity_id`, `provisioned_at`.

Comment progress after each sub-step. Log the fleet row in [`../deployment-engineer/life/areas/fleet.md`](../../agents/deployment-engineer/life/areas/fleet.md).

## Step 1 — Email Crawler

Use the [`amis-email-crawler`](../amis-email-crawler/SKILL.md) skill. Parameters:

- `composio_entity_id`: the entity from Step 3.
- `gmail_account`: from the client's portal onboarding form.
- `date_range`: last 12 months.
- `target_supabase_url`: the client's Supabase project URL.
- `passes`: 48 (one per week).

The crawler runs as a Hermes-adapter job. Expect 2–6 hours of runtime depending on inbox size. The crawler writes `tone.md`, `questions.md`, `booking_patterns.md`, `edge_cases.md`, `INDEX.md` to the client's Supabase.

When the crawler comments complete on your issue, reassign to **Implementation Engineer** for PRD Step 2 (client validation).

## Steps 2, 4, 5 — wait for client / Jules / Implementation Engineer

Set your issue to `blocked` with a clear message naming what you're waiting on. Heartbeat exits.

The Client Portal UX (PRD §6.4) walks the client through Step 2 (review knowledge index) and Step 4 (config panel). Jules handles Step 5 (authorise Composio connections from the portal submission).

When Implementation Engineer comments "client validated knowledge index + config + auth" on the issue → unblock and proceed to Step 6.

## Step 6 — Deploy Amis

Preconditions:
- `<slug>` VM running.
- Supabase project populated with validated knowledge index + client config.
- Composio entity has all required OAuth connections authorised.

Actions:
1. `ssh ubuntu@<vm-ip>` (via `AKIRA_VM_SSH_KEY`).
2. Write `/opt/amis/.env`:
   ```
   SUPABASE_URL=<client-supabase-url>
   SUPABASE_SERVICE_KEY=<client-service-key>
   CLIENT_COMPOSIO_ENTITY_ID=<entity-id>
   CLIENT_SLUG=<slug>
   PORTAL_WEBHOOK_URL=https://<slug>.akira-agent.com/api/amis-webhook
   COMPOSIO_API_KEY=<from-env>
   ```
3. `docker pull ghcr.io/jules756/amis:stable`.
4. `docker run -d --name amis --restart=always --env-file /opt/amis/.env -p 8080:8080 ghcr.io/jules756/amis:stable`.
5. Wait 30s for startup. `GET http://<vm-ip>:8080/health` → expect 200 with JSON body `{status: "ok", version: "<v>", skills: [...]}`.
6. **Smoke test.** Send a test email to the client's monitored Gmail with subject `[akira-smoke-test]`. Verify the classifier picks it up within 5 minutes (check Amis logs via `docker logs amis`).
7. Flip the Amis agent from `paused` to `active` in Paperclip.
8. Comment on the onboarding issue:
   ```
   Deployed: <client-slug>
   VM: <id> / IP: <ip>
   Subdomain: https://<slug>.akira-agent.com
   Supabase: <project-id>
   Amis: <version>
   Healthcheck: 200
   Smoke test: passed at <timestamp>
   ```
9. Reassign to Implementation Engineer for Stage 4 (Testing) monitoring.

## Step 7 — Live

Automatic once Step 6 passes. Product Manager flips the Notion Bot row to `Production`.

## Common failure modes

- **Supabase schema migration fails.** Usually transient; retry once. Persistent → flag Founding Engineer, the schema might have drifted.
- **GHCR auth fails on cloud-init.** Token scope may be wrong (`read:packages` required). Verify, rotate if needed.
- **Composio OAuth missing on Step 6.** Client hasn't finished the portal onboarding. Keep issue `blocked`; Implementation Engineer nudges.
- **Smoke test doesn't fire.** Email Crawler hasn't finished (still indexing) OR classifier schema mismatch. Check Amis logs first; escalate to Founding Engineer if it's a skill-side bug.
