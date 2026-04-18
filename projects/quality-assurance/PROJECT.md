---
name: "Quality Assurance"
slug: quality-assurance
status: in_progress
color: "#a855f7"
---

# Quality Assurance

Every Amis release passes through this project. QA Engineer gates the `:candidate` → `:stable` promotion, runs fleet regression after every rollout, and turns every live-client bug into a permanent regression test. Distinct from the Auditor's work — Auditor scores agents against rubrics; QA scores *code*.

## Scope

### Promotion gate
- **`:candidate` → `:stable` verification** — every build Founding Engineer tags as `ghcr.io/jules756/amis:candidate-<sha>` runs the full test matrix on a fresh internal-dev VM before it can be promoted to `:stable`. Green → promotion approved. Red → blocked with a failing test handed to Founding Engineer.

### Fleet regression
- **Post-rollout** — when Deployment Engineer completes a fleet rollout, QA sends sentinel test emails to one VM per client profile and verifies the handler chain.
- **Nightly** — same suite runs against every live `env=prod` VM at 03:00 CET.
- **Rollback trigger** — any red result triggers Deployment Engineer to revert to the prior `:stable`.

### Bug reproduction
- Every live-client bug Implementation Engineer surfaces gets reproduced on an internal-dev VM running the affected version.
- A failing test enters the matrix permanently. Only after that test goes green on a fixed `:candidate` does the bug close.

### Test matrix + fixtures
- One row per scenario × skill. New skill → new rows authored alongside Founding Engineer's build.
- Synthetic fixtures per client profile (restaurant / hotel / small / multi-location). Never real client data.

### Coverage reporting
- Weekly coverage dashboard to CTO's "Engineering Retro" issue: matrix green/red, bug age buckets, fleet pass-rate trend.

## Primary agents

- **QA Engineer** (owner) — promotion gate, fleet regression, bug reproduction, matrix evolution.
- **Founding Engineer** — writes the unit tests inside the skill repo + fixes bugs QA hands back.
- **Deployment Engineer** — provisions internal-dev VMs for QA + executes rollbacks when QA signals.
- **Implementation Engineer** — sends live-client bugs for regression authoring.
- **CTO** — receives the weekly coverage dashboard.

## References

- [`test-matrix.md`](../../agents/qa-engineer/life/areas/test-matrix.md) — scenarios × skills.
- [`test-incidents.md`](../../agents/qa-engineer/life/areas/test-incidents.md) — append-only log + miss log.

## Success criteria (ongoing)

- 100% of `:candidate` → `:stable` promotions have a documented QA green verdict.
- Every live-client bug converts to a regression test within 24h.
- Fleet-regression pass rate ≥ 99% weekly.
- Zero bugs reach a second live client after the first reports it (regression should have caught the second).
- Promotion-gate median time-to-verdict ≤ 4 hours.
