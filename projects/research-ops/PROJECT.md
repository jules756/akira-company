---
name: "Research Ops"
slug: research-ops
status: in_progress
color: "#eab308"
---

# Research Ops

All cross-team deep-research flows through one concierge. Research Coordinator reframes team requests into Deerflow-shaped queries, dispatches to the Deerflow VM, polls async completion, and condenses reports back into each requester's preferred format. Individual research *deliveries* land in the consumer's project (a company deep-profile into `sales-pipeline`, a trend brief into `seo-aeo-growth`, etc.). This project tracks the *infrastructure + hygiene* — the coordinator's own work.

## Scope

### Concierge operations
- **Reframing** every incoming request into Intent + Scope + Output schema + Depth before dispatching.
- **Dispatching** to Deerflow via the three-endpoint API (`POST /jobs`, `GET /jobs/{id}`, `GET /jobs/{id}/report`).
- **Polling** async jobs hourly; flagging SLA breaches on the originating issue.
- **Condensing** returned reports into the requester's schema; never forwarding raw Deerflow dumps.

### Cache + queue hygiene
- Freshness windows per topic class: 7d (news / SEO signals), 30d (company profiles / trends), 90d (foundational research).
- Queue ledger maintained in `research-queue.md`; quarterly archive pruning.
- Duplicate-request detection before every dispatch.

### Template library maintenance
- Reframing templates per requester role (Lead Researcher, SEO + AEO Specialist, SEO Trend Researcher, Lead Magnet Creator, Content Writer, Head of Sales, Head of Marketing, CEO).
- Three+ similar requests of a new type → new template row added.

### Source-quality oversight
- Broken links, single-source claims, outdated sources, spam / SEO-farm sources all flagged in delivery; never laundered as corroborated.
- Fabricated citations are a zero-tolerance rubric item.

### Deerflow infrastructure relationship
- Deerflow VM itself is CTO + Deployment Engineer's infra (lives in `fleet-ops`).
- Research Coordinator does NOT own the VM; health escalations route to CTO.
- API contract changes + version upgrades are tracked here via the `deerflow-invocation` skill.

## Primary agents

- **Research Coordinator** (owner) — the single concierge.
- **CEO** — reports to.
- **CTO** — owns the Deerflow VM; escalation point on API outages.
- **Deployment Engineer** — provisions + maintains the Deerflow VM (tracked in `fleet-ops`).

## Consuming agents (who files research requests)

- Lead Researcher — highest volume (company deep-profiles).
- SEO + AEO Specialist — algorithm-change briefs + competitor audits.
- SEO Trend Researcher — trend deep-dives beyond the daily scan.
- Lead Magnet Creator — ideation reports.
- Content Writer — background research for long-form.
- Head of Sales — live-deal intelligence.
- Head of Marketing — campaign / market research.
- CEO / Jules — strategic research.

## References

- [`research-queue.md`](../../agents/research-coordinator/life/areas/research-queue.md) — live queue.
- [`deerflow-prompts.md`](../../agents/research-coordinator/life/areas/deerflow-prompts.md) — reframing templates.
- [`deerflow-invocation`](../../skills/akira-agent/deerflow-invocation/SKILL.md) — API contract.

## Success criteria (ongoing)

- Every request acknowledged within 15 minutes of receipt.
- 100% of deliveries include citations for every factual claim.
- Cache hit rate ≥ 30% (indicates dedup is working; lower = missed reuse).
- Zero Deerflow dumps forwarded raw; every delivery matches the requester's schema.
- Templates updated within a week of detecting three+ similar requests of a new type.
