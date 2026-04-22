---
name: "Customer Success"
slug: customer-success
status: in_progress
color: "#ec4899"
---

# Customer Success

Retention + library-driven upsell + referral. The second human function at Akira; supported end-to-end by the Customer Success Manager agent.

## Scope

- **Daily retention scan** — every Production / Maintenance Bot gets a health check. Churn signals flagged same-heartbeat.
- **Library-ship sweep** — every time Skilled Builder ships a new capability, CS sweeps all Production clients for fit → creates upsell subtasks for Head of Sales.
- **Monthly upsell review** — even without new skills, proactively re-scan each client's skill stack vs. library gaps.
- **90-day referral asks** — triggered automatically for every Production client at the 90-day mark.
- **Weekly retention brief** — posted to CEO's standing Governance Review issue every Friday.
- **Churn response** — any flagged signal (≥3 unresolved incidents / 30-day silence / explicit pause / late payment) gets a SIGNAL + recommended action within 24h.

## Primary agents

- Customer Success Manager (owner)
- Coordinates with Head of Sales (upsell closes), Implementation Engineer (tech triage), Content Writer (drafts), Head of Marketing (referral draft approvals)

## References

- [`retention-playbook.md`](../../agents/customer-success-manager/life/areas/retention-playbook.md) — SLAs + templates + churn signals.
- [`client-health.md`](../../agents/customer-success-manager/life/areas/client-health.md) — per-client scorecard.
- Use the document in the projects for the capability inventory — what's deployable for upsells.

## Success criteria (ongoing)

- **NRR ≥ 120%** (rolling 3-month).
- **Referrals ≥ 0.33 per Production client per quarter.**
- **Churn ≤ 10% annual.**
- Every library-ship triggers a sweep within 24h.
- Zero missed 90-day referral asks.
