---
name: vm-provisioning
slug: vm-provisioning
key: akira-agent/vm-provisioning
description: "Use when provisioning, inspecting, or destroying VMs on Hetzner Cloud for Akira client environments or internal dev sandboxes. Covers the `POST /v1/servers` / `DELETE /v1/servers/{id}` lifecycle, mandatory tagging discipline, cloud-init bootstrap with Docker + GHCR auth + SSH key, and teardown confirmation. Wraps Hetzner REST API calls as an SOP so a non-reasoning model can execute reliably."
metadata:
  sources:
    - kind: internal
      path: skills/akira-agent/vm-provisioning
      repo: jules756/akira-company
      trackingRef: main
---

# VM Provisioning — Hetzner Cloud

Use this skill when Deployment Engineer (or anyone with the right permissions) needs to create or destroy a VM on Hetzner. Every VM Akira runs is created through this flow.

## Env required

- `HETZNER_API_TOKEN` — Hetzner Cloud API token (read+write).
- `AKIRA_VM_SSH_KEY_NAME` — the SSH key name as registered in Hetzner (e.g., `akira-deployment-engineer`).
- `GHCR_READ_TOKEN` — so the bootstrap can `docker login ghcr.io`.

All other config comes from the task body (client slug, env, TTL, etc.).

## Create a VM

### Required task-body fields

```yaml
client_slug: <slug or "internal-dev">
env: prod | dev
purpose: <one-line reason>   # e.g. "client-runtime" or "amis-skill-test"
ttl: <hours or "none">
size: cx22                    # default — override only with CTO approval
region: fsn1 | hel1           # fsn1=Nuremberg, hel1=Helsinki. EEA only.
image: ubuntu-22.04
```

### Tags (required on every create — no exceptions)

```
client=<slug-or-empty>
env=<prod|dev>
purpose=<reason>
ttl=<hours|none>
created-by=deployment-engineer
created-at=<YYYY-MM-DD>
```

Hetzner's API accepts a `labels` map on create. Write all six keys.

### API call

```http
POST https://api.hetzner.cloud/v1/servers
Authorization: Bearer {HETZNER_API_TOKEN}
Content-Type: application/json

{
  "name": "akira-<slug>-<YYYYMMDD>",
  "server_type": "cx22",
  "image": "ubuntu-22.04",
  "location": "fsn1",
  "ssh_keys": ["{AKIRA_VM_SSH_KEY_NAME}"],
  "labels": {
    "client": "<slug>",
    "env": "prod",
    "purpose": "client-runtime",
    "ttl": "none",
    "created-by": "deployment-engineer",
    "created-at": "<YYYY-MM-DD>"
  },
  "user_data": "#cloud-config\n<bootstrap-yaml>"
}
```

Expect a 201 with a server object. Grab `server.id`, `server.public_net.ipv4.ip` — log both to `fleet.md`.

### Cloud-init bootstrap (user_data)

```yaml
#cloud-config
package_update: true
package_upgrade: true
packages:
  - docker.io
  - ca-certificates
  - curl
runcmd:
  - systemctl enable --now docker
  - usermod -aG docker ubuntu
  - echo "$GHCR_READ_TOKEN" | docker login ghcr.io -u jules756 --password-stdin
  - mkdir -p /opt/amis
  - cat > /etc/cron.daily/amis-pull <<'CRON'
#!/bin/bash
docker pull ghcr.io/jules756/amis:stable
docker stop amis 2>/dev/null || true
docker rm amis 2>/dev/null || true
docker run -d --name amis --restart=always --env-file /opt/amis/.env -p 8080:8080 ghcr.io/jules756/amis:stable
CRON
  - chmod +x /etc/cron.daily/amis-pull
```

The `.env` file at `/opt/amis/.env` is written in the Amis deploy step (see `client-onboarding-runbook` skill) — not here.

### Post-create verification

- `GET /v1/servers/{id}` — confirm `status: running`.
- SSH test: `ssh ubuntu@<ip> echo ok` — must succeed within 60s of create.
- If either fails → `DELETE` the server, log the failure in `deploy-incidents.md`, retry once, else escalate to CTO.

## Destroy a VM

```http
DELETE https://api.hetzner.cloud/v1/servers/{id}
Authorization: Bearer {HETZNER_API_TOKEN}
```

Expect 204 No Content. Confirm destruction with `GET /v1/servers/{id}` → 404.

**Before destroying a client-env VM:** export the linked Supabase data per the DPA retention clause (see `client-onboarding-runbook` skill's teardown section). The `vm-provisioning` skill does not export data; it only destroys infrastructure.

## List fleet

```http
GET https://api.hetzner.cloud/v1/servers?label_selector=created-by=deployment-engineer
```

Returns every VM this agent has provisioned. Useful for the hourly fleet-health scan.

## Common pitfalls

- **Untagged VMs.** If a VM ends up without all six labels (e.g., a manual create bypassing this skill), it shows up as an orphan. Log it to `deploy-incidents.md` immediately; never silently adopt.
- **SSH key mismatch.** The `ssh_keys` field takes the key NAME (as registered in Hetzner), not the public-key string. `AKIRA_VM_SSH_KEY_NAME` carries that name.
- **Region drift.** Only `fsn1` (Nuremberg) and `hel1` (Helsinki) — both EEA. Never pick US or APAC regions without CTO approval.
- **Image updates.** Ubuntu LTS only. Ubuntu 24.04 upgrade requires a CTO-approved bootstrap update first.
- **Rate limits.** Hetzner's API is generous but not infinite. Batch operations where possible; back off on 429.
