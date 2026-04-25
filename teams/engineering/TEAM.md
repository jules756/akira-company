---
name: Engineering & Product
description: Planning, building, shipping, and delivering productized AI agents (Amis bundles) to per-client isolated environments. Owns the skill library, deployment surfaces, configuration pages, and the per-client VM fleet.
slug: engineering
manager: ../../agents/cto/AGENTS.md
includes:
  - ../../agents/founding-engineer/AGENTS.md
  - ../../agents/qa-engineer/AGENTS.md
  - ../../agents/deployment-engineer/AGENTS.md
  - ../../agents/implementation-engineer/AGENTS.md
  - ../../agents/ux-designer/AGENTS.md
  - ../../agents/product-manager/AGENTS.md
tags:
  - engineering
  - product
  - delivery
  - deployment
  - qa
  - design
primary_project: skill-library-delivery
---

The Engineering & Product team takes a signed contract and turns it into a live Amis deployment running in an isolated per-client environment. CTO locks plans and gates reviews. Skilled Builder compounds the reusable capability library. Founding Engineer focuses on platform engineering and net-new capability work. QA Engineer gates every `:candidate` → `:stable` promotion through the test matrix + runs fleet regression after every rollout + reproduces live-client bugs into permanent regression tests. Deployment Engineer spins up a brand-new VM + Supabase project + Vercel subdomain per client and deploys the QA-approved `:stable` image — no terminal work from Jules. Implementation Engineer owns first-line support for live clients, collects Stage-4 feedback, and routes bugs to QA (for regression) / Founding Engineer (for fix). UX Designer maintains the design system across the client portal and the per-client Control Panel. Product Manager owns the "Agents (The Bots)" Notion DB and audits each new Proposal against the library — reuse first, build only where the library doesn't cover. The team also owns the deployment surfaces that let clients configure, observe, and extend their agents without touching raw infra.

Reports to CEO via CTO. Primary project: `skill-library-delivery`.
