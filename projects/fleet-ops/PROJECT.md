---
name: "Fleet Ops"
slug: fleet-ops
status: in_progress
color: "#14b8a6"
---

# Fleet Ops

Everything about the per-client VM fleet — provisioning new clients, keeping them healthy, rolling out skill updates, tearing down churned clients — lives here. Deployment Engineer owns it end-to-end; CTO reviews monthly.

## Scope

### Client VM lifecycle
- **Provisioning** — brand-new Hetzner VM + Supabase project + Vercel subdomain + Composio entity when a signed contract lands. No pre-allocation; created on demand.
- **Bootstrap** — cloud-init installs Docker, authenticates GHCR, preps the `/opt/amis/.env` slot.
- **Amis deploy** — pulls the QA-approved `:stable` image, starts the container, verifies healthcheck + smoke test.
- **Churn teardown** — DPA-compliant data export → VM destroy → Supabase archive → Vercel subdomain removal → Composio entity archive.

### Internal-dev VMs (mode 2)
- On-demand VMs for Jules / CTO / QA Engineer when they need a clean box for testing, staging, or skill experiments.
- Tagged with TTL (default 72h) + auto-destroyed at expiry unless explicitly tagged `persistent`.

### Fleet health + rollouts
- **Hourly healthcheck** on every live VM.
- **Pull-model rollouts** — when Founding Engineer tags a new `:stable` via QA's promotion gate, per-VM cron pulls within 24h.
- **Rollout verification** — Deployment Engineer confirms 100% of prod VMs are on the new version; QA runs fleet regression smoke.
- **Rollback** — on verified regression, revert to prior tag on affected VMs.

### Cost + hygiene
- **Weekly cost report** to CTO's "Infra Spend" issue.
- **Orphan detection** — VMs without required tags or past TTL get cleaned up same-week.

## Primary agents

- **Deployment Engineer** (owner) — provisioning, teardown, rollouts, cost accounting.
- **QA Engineer** — requests dev VMs for promotion-gate tests + runs fleet regression after rollouts.
- **CTO** — approves architectural decisions (region changes, provider switches, tag scheme changes).

Coordinates with Founding Engineer (tag handoffs), CSM (churn trigger), Implementation Engineer (client-side issues that turn out to be deploy-side).

## References

- [`fleet.md`](../../agents/deployment-engineer/knowledge/fleet.md) — live client + dev VM roster.
- [`deploy-incidents.md`](../../agents/deployment-engineer/knowledge/deploy-incidents.md) — append-only incident log.
- [`vm-provisioning`](../../skills/akira-agent/vm-provisioning/SKILL.md) — Hetzner API SOP.
- [`client-onboarding-runbook`](../../skills/akira-agent/client-onboarding-runbook/SKILL.md) — 7-step onboarding.

## Success criteria (ongoing)

- New-client onboarding completes in < 60 agent-minutes from signed-contract trigger to `active` healthcheck.
- Zero untagged VMs in the fleet.
- 100% of live VMs pulling `:stable` within 24h of a rollout.
- Every churned client torn down within 7 days of the `churned` CRM flag, with DPA export logged.
- Weekly cost report posts every Friday without manual nudging.
- Client runtime issues stay isolated from capability-library changes unless a shared fix is required.
