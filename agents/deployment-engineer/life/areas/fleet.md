# Fleet — Live VM Roster

Living inventory of every VM Deployment Engineer has spun up. Update on every provision / teardown. Append-only status column; never delete rows (mark churned + keep the history).

## Production clients

| Client slug | VM ID (Hetzner) | Region | Supabase project ID | Subdomain | Amis version | Status | Created | Last healthcheck | Notes |
|---|---|---|---|---|---|---|---|---|---|
| _(none yet — first client lands here)_ | | | | | | | | | |

## Internal dev VMs

| Purpose | VM ID | Tags | TTL | Created | Destroyed | Notes |
|---|---|---|---|---|---|---|
| _(none yet)_ | | | | | | |

## Orphan log

Resources without a matching client row or past TTL without `persistent` tag. See [`deploy-incidents.md`](deploy-incidents.md) for teardown actions taken.

| Resource | ID | Detected | Resolution | Date |
|---|---|---|---|---|
| _(none yet)_ | | | | |

## Teardown archive

Churned clients whose VMs and Supabase projects have been destroyed. Keep the row for the audit trail.

| Client slug | Churn date | VM destroyed | Supabase archived | DPA export location | Notes |
|---|---|---|---|---|---|
| _(none yet)_ | | | | | |

---

**Conventions**
- Status values: `provisioning | active | degraded | rolling-back | churned | teardown-complete`.
- Timestamps in ISO-8601 (UTC).
- Never edit a row's `Created` date.
- When a client moves from `active` to `degraded`, comment the reason in Notes + open an issue for Implementation Engineer.
