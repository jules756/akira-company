---
name: "Skill Library & Delivery"
slug: skill-library-delivery
status: in_progress
color: "#8b5cf6"
---

# Skill Library & Delivery

Engineering's compounding track: build once, ship to all clients. The library is the moat. This project covers what Founding Engineer builds + how it's gated by QA + how it's delivered per-client. Infrastructure (VM fleet) lives in [`fleet-ops`](../fleet-ops/PROJECT.md). Code-side testing lives in [`quality-assurance`](../quality-assurance/PROJECT.md).

## Scope

### Library expansion (Founding Engineer)
- **Library builds** — new skills ship to [`skill-library.md`](../../agents/founding-engineer/life/areas/skill-library.md). Build once, deploy N.
- **Extensions + maintenance** — existing skills evolve as new patterns surface from Implementation Engineer's pattern reports.
- **CI tags** — every merge produces `ghcr.io/jules756/amis:candidate-<sha>`. Never self-promoted to `:stable` — QA owns that gate.
- **Library promotion** — when a skill moves to `production` state, Founding Engineer notifies Customer Success Manager → triggers a library-sweep for upsells.

### Release gate (QA Engineer)
- **Promotion-gate verdict** — every `:candidate-<sha>` runs the QA test matrix on a fresh internal-dev VM. Green → promoted to `:stable`. Red → blocked with a failing test for Founding Engineer.
- Details of the gate + regression workflow live in [`quality-assurance`](../quality-assurance/PROJECT.md).

### Per-client delivery
- **PM skill-fit audit** — on every new Proposal → matches client needs to the existing library first. Build only when the library doesn't cover.
- **CTO technical plan** — for net-new skills only (library-covered skills skip architectural planning).
- **Deployment Engineer provisioning** — spins up the client's VM + Supabase + subdomain + deploys `:stable` Amis. Details in [`fleet-ops`](../fleet-ops/PROJECT.md).
- **Implementation Engineer support** — Stage-2 knowledge-index validation + Stage-4 Testing feedback + first-line live-client support. Surfaces patterns back to Founding Engineer.
- **UX Designer** — voice-agent UX, client Control Panel design, design system maintenance.
- **PM transitions** — Proposal → Building → Feedback → Production → Maintenance on the Agents (The Bots) Notion DB.

## Primary agents

- **CTO** (team lead).
- **Founding Engineer** — library builder.
- **QA Engineer** — promotion gate + fleet regression (owns [`quality-assurance`](../quality-assurance/PROJECT.md)).
- **Deployment Engineer** — per-client infra (owns [`fleet-ops`](../fleet-ops/PROJECT.md)).
- **Implementation Engineer** — client support + feedback + pattern surfacing.
- **UX Designer** — voice-agent UX + design system.
- **Product Manager** — skill-fit audit + delivery orchestration.

## References

- [`skill-library.md`](../../agents/founding-engineer/life/areas/skill-library.md) — canonical library inventory.
- [`delivery-playbook.md`](../../agents/product-manager/life/areas/delivery-playbook.md) — SLAs + templates.
- [`design-system.md`](../../agents/ux-designer/life/areas/design-system.md) — Nordic hospitality aesthetic rules.

## Success criteria (ongoing)

- By Y1 end: ≥10 library skills in `production` state.
- Every Proposal audited against the library before any build work starts.
- Every library-ship triggers a CS upsell sweep within 24h.
- No client Bot stuck in Building > 21 days without a stated blocker.
- No library skill `production` > 30 days without a deployment to ≥2 clients.
- Every `:candidate` promoted to `:stable` has a QA green verdict on file.
