# VM-RUNBOOK (Flattened provisioning steps)

## Hetzner Provisioning
1. API call: create CX22 server (fsn1 region)
2. SSH key injection (AKIRA_VM_SSH_KEY)
3. Docker install + Amis pull
4. Healthcheck endpoint setup

## Client Onboarding (7 steps)
1. Spin VM
2. Create Supabase project
3. Create Vercel subdomain
4. Composio entity
5. Email Crawler
6. Amis deploy + smoke test
7. Hand off to Implementation Engineer