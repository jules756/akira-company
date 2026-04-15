---
name: "Skill Library & Delivery"
slug: skill-library-delivery
status: in_progress
color: "#8b5cf6"
---

# Skill Library & Delivery

Engineering's two compounding tracks. The library is the moat.

## Scope

### Library expansion
- **Library builds** — Founding Engineer ships new skills to [`skill-library.md`](../../agents/founding-engineer/life/areas/skill-library.md). Build once, deploy N.
- **Extensions + maintenance** — existing skills evolve as new patterns surface. Bugs caught by Implementation Engineer or flagged by Customer Success.
- **Library promotion** — when a skill moves to `production`, Founding Engineer notifies Customer Success Manager → triggers library-sweep for upsells.

### Per-client delivery
- **PM skill-fit audit** — on every new Proposal → Active. Matches client needs to library first.
- **CTO technical plan** — for net-new or extension skills only (library-covered skills skip this).
- **Implementation Engineer deploys** — per-client Vapi / Twilio / Cal.com / email configs, versioned in `life/areas/clients/<slug>/`.
- **UX Designer reviews** — voice-agent UX, client dashboards, design system.
- **PM transitions** — Proposal → Building → Feedback → Production → Maintenance.

## Primary agents

- CTO (team lead)
- Founding Engineer (library builder)
- Implementation Engineer (library deployer)
- UX Designer
- Product Manager (skill-fit auditor + delivery orchestrator)

## References

- [`skill-library.md`](../../agents/founding-engineer/life/areas/skill-library.md) — canonical library inventory.
- [`delivery-playbook.md`](../../agents/product-manager/life/areas/delivery-playbook.md) — SLAs + templates.
- [`design-system.md`](../../agents/ux-designer/life/areas/design-system.md) — Nordic hospitality aesthetic rules.

## Success criteria (ongoing)

- By Y1 end: ≥10 library skills in `production` state.
- Every Proposal audited against the library before any build work starts.
- Every library-ship triggers a CS upsell sweep within 24h.
- No client Bot stuck in Building > 21 days without a stated blocker.
- No library skill `production` > 30 days without a deployment to ≥2 clients (if it's not being used, why did we build it?).
