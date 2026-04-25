# Akira Agent

![Org Chart](images/org-chart.png)

> **Akira Agent** — AI workforce for Scandinavian hospitality. This repo is a [Paperclip](https://paperclip.ing) company package: the agents, skills, routines, and org structure that run the company.
>
> **Product:** productized AI agents (the *Amis* bundle — email classifier + Q&A + small-booking + complex-booking + integration) deployed to isolated per-client environments. Each client gets their own VM, Supabase project, Vercel subdomain, and configured Amis runtime. Target: 500K–1M SEK MRR, Sweden + Norway.
>
> **Platform:** Paperclip (control plane) + Vercel (all web surfaces) + Supabase (backend state) + Vapi + Twilio (voice) + Hetzner (per-client VMs). Non-reasoning Grok on most roles; reasoning tiers on deciders.

## What's Inside

| | Count |
|---|---|
| Agents | 26 |
| Teams | 3 operating teams + 1 Web publishing subteam + Chief of Staff |
| Projects | 12 |
| Skills | 74 |
| Scheduled routines | 19 |

## How the team is organised

Everything is a tree rooted at CEO. The Chief of Staff routes work; the Auditor observes; Legal Counsel files contracts; the operating teams deliver, with Web nested under Marketing as the publishing subteam.

```
CEO
├── Chief of Staff           # routing, handoffs, duplicate checks, queue triage
├── Auditor                  # scores agents against role rubrics
├── Legal Counsel            # MSA + DPA + eSignature via Google Drive
├── Research Coordinator     # Deerflow concierge for every team's research asks
├── Customer Success Manager  # retention, upsell, referral
├── CTO                      # locks plans, gates reviews
│   ├── Founding Engineer        # writes the Amis skills
│   ├── QA Engineer              # :candidate → :stable promotion gate + fleet regression
│   ├── Deployment Engineer      # spins up per-client VMs on Hetzner
│   ├── Implementation Engineer  # first-line client support + Stage-4 feedback
│   ├── UX Designer              # portal UX + voice-agent UX
│   └── Product Manager          # Agents (The Bots) Notion DB
├── Head of Sales
│   ├── CRM Steward
│   ├── Follow-Up Manager
│   ├── Lead Researcher
│   └── Growth Analyst
└── Head of Marketing
    ├── LinkedIn Engagement Agent
    ├── Content Writer
    ├── Lead Magnet Creator
    ├── SEO + AEO Specialist
    ├── SEO Trend Researcher
    ├── Web Writer
    ├── Web Designer
    └── Web Operations
```

**QA vs Auditor** — QA tests code (test matrix, fleet regression, bug reproduction). Auditor scores agent behaviour against rubrics. Different scope; no overlap.

## Operating model

Paperclip runs through four explicit flows:
- lead and reputation
- new client deployment
- skill library growth
- maintenance

The Chief of Staff routes work into the right flow before execution.

See [`docs/paperclip-routines.md`](docs/paperclip-routines.md) for the manual Paperclip routine creation list. Agent docs describe behavior; this file is the external routine catalog to create from.

## How a client goes live

1. **Head of Sales** closes → Proposal status `Signed` in Notion.
2. **Legal Counsel** drafts MSA + DPA + SOW → files in Google Drive → eSignature after Jules approves.
3. **Deployment Engineer** spins up a Hetzner VM + Supabase project + `<slug>.akira-agent.com` Vercel subdomain + Composio entity (when available).
4. **Email Crawler** indexes 12 months of client Gmail → writes `tone.md` / `questions.md` / `booking_patterns.md` / `edge_cases.md` / `INDEX.md` to the client's Supabase.
5. **Client** validates the knowledge index + fills the config panel in their portal.
6. **Deployment Engineer** deploys the QA-approved `:stable` Amis Docker image to the client's VM. Healthcheck 200 → `active`.
7. **Implementation Engineer** runs the Stage-4 Testing week, collects feedback, hands off to **CSM** at Production.

## Teams

### Sales
Drives pipeline from cold Nordic hospitality prospects to signed Proposals. Head of Sales, CRM Steward, Follow-Up Manager, Lead Researcher, Growth Analyst.

### Marketing
Builds demand + converts engagement. Head of Marketing, LinkedIn Engagement, Content Writer, Lead Magnet Creator. Cross-posts with Web for the SEO/AEO loop.

### Web (Marketing subteam)
Owns `akira-agent.com` end-to-end as Marketing's publishing subteam. SEO + AEO Specialist, SEO Trend Researcher, Web Writer (14 articles/week), Web Designer, Web Operations. Next.js + Vercel single stack.

### Engineering & Product
Builds + ships + supports the Amis agent bundle. CTO, Founding Engineer, QA Engineer, Deployment Engineer, Implementation Engineer, UX Designer, Product Manager.

## Stack

- **Control plane:** Paperclip (open-source orchestrator).
- **Runtime per agent:** `opencode_local` adapter with `azure-grok/grok-4-1-fast-non-reasoning` (reasoning tiers on specific roles when promoted).
- **Web:** Next.js on Vercel (website, magnet pages, per-client Control Panel subdomains).
- **Backend:** Supabase (Postgres + Auth + Storage + Realtime + Edge Functions). One project per client for GDPR isolation.
- **Voice:** Vapi + Twilio, EU regions.
- **Client VMs:** Hetzner Cloud (CX22, Nuremberg / Helsinki regions), provisioned on-demand by Deployment Engineer.
- **Images:** GHCR (`ghcr.io/jules756/amis:stable`).
- **OAuth aggregator (future):** Composio. Currently using direct API credentials per service; Composio ↔ Paperclip integration isn't production-ready yet.

See [`teams/`](teams/) for per-team manifests and [`agents/<slug>/AGENTS.md`](agents/) for per-agent operating manuals.

## Project directory

| Project | Status | Primary owner |
|---|---|---|
| `onboarding` | in_progress | CEO |
| `demand-creation` | in_progress | Head of Marketing |
| `sales-pipeline` | in_progress | Head of Sales |
| `skill-library-delivery` | in_progress | CTO |
| `quality-assurance` | in_progress | QA Engineer |
| `fleet-ops` | in_progress | Deployment Engineer |
| `customer-success` | in_progress | Customer Success Manager |
| `research-ops` | in_progress | Research Coordinator |
| `legal-compliance` | in_progress | Legal Counsel |
| `company-ops` | in_progress | CEO |
| `web-publishing` | in_progress | Head of Marketing |
| `seo-aeo-growth` | in_progress | SEO + AEO Specialist |

## Import into Paperclip

```bash
pnpm paperclipai company import https://github.com/jules756/akira-company
```

Secrets + env wiring are maintained in a separate `SECRETS-CHECKLIST.md` outside this repo (the Paperclip importer expects a clean company folder). The checklist enumerates every API key the platform needs before the company can run live.

## Design principles

1. **Tree-only org.** Every agent reports to exactly one manager.
2. **One job per agent.** Split role when "and" shows up more than twice in a capability line.
3. **Skills over prose.** If an `AGENTS.md` is long, extract the procedure into a skill.
4. **Heartbeat cadence = accountability cadence.** Daily heartbeat → daily ownership.
5. **Auditor before scale.** Observational rubric scoring, no corrective authority.
6. **QA before rollout.** Every `:candidate` → `:stable` passes the promotion gate.
7. **Memory is infrastructure.** PARA + daily notes + fact extraction for strategic roles.
8. **On-demand infra.** Client VMs exist because the client exists — created on onboarding, destroyed on churn.
9. **Pick boring.** Vercel over VPS. Supabase over hand-rolled DB. Cal.com over custom scheduling.

## License

Private — not for distribution. Akira Agent internal.

---

Built on [Paperclip](https://paperclip.ing). Maintained by Jules ([jules@akira-agent.com](mailto:jules@akira-agent.com)).
