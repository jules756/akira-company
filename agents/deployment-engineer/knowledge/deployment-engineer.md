# Rubric — Deployment Engineer

Scored daily by Auditor. Each dimension 0–5.

### 1. Onboarding speed + correctness (0–5)
- **5** — New clients go from signed-contract trigger to `active` Amis healthcheck in under 60 agent-minutes. Every sub-step logged. No manual interventions by Jules.
- **3** — Onboarding completes but with one manual fix or slower than 2 hours.
- **1** — Multiple manual fixes; Jules had to ssh in.
- **0** — Onboarding failed to complete.

### 2. Tagging discipline (0–5)
- **5** — Every VM has all six required labels (`client`, `env`, `purpose`, `ttl`, `created-by`, `created-at`). Zero orphans. Hourly scan confirms.
- **3** — 1–2 orphans detected; resolved within the same day.
- **1** — Orphan count trending up; not all resolved.
- **0** — Untagged VMs running in production. Zero-tolerance dimension.

### 3. Fleet health response (0–5)
- **5** — Every unreachable VM is flagged within the hourly scan, a post comment is filed within 15 minutes, rollback happens within the same heartbeat if warranted.
- **3** — Flagged but slow response.
- **1** — Missed fleet-health scan; degradation noticed by a downstream agent instead.
- **0** — Client outage went undetected for 24h+.

### 4. Churn teardown completeness (0–5)
- **5** — Every `churned` flag triggers DPA export → VM destroy → Supabase archive → Vercel removal → Composio archive, all logged in `fleet.md` and `deploy-incidents.md`. Within 7 days.
- **3** — Teardown completed but missed one step (typically DPA export).
- **1** — Resources lingered past 7 days; cost leakage.
- **0** — Churned client's data persisted, or resources never destroyed.

### 5. Cost + orphan hygiene (0–5)
- **5** — Friday cost report posts on time with accurate deltas. Orphan count is zero or explained. Over-provisioning caught before it becomes visible on the invoice.
- **3** — Report posts but missing orphan breakdown.
- **1** — Over-budget without explanation.
- **0** — No reports; cost surprises Jules.
