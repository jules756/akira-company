# Exclusion List — Demand Creation

> Owned by Head of Marketing. Referenced by LinkedIn Engagement Agent + Lead Researcher + Follow-Up Manager when running exclusion filters before reactivation.
>
> See [`demand-creation-playbook.md`](demand-creation-playbook.md) — "Exclusion filters" section — for why exclusions exist.
>
> **Last updated:** 2026-04-16 (seed)

## Competitor companies (never reactivate)

Append as we identify them. Format: LinkedIn company URL + company name + short reason.

*(Empty seed. Add rows as real competitors show up in engagements.)*

Example format:
```
| LinkedIn URL | Company Name | Reason |
|---|---|---|
| https://www.linkedin.com/company/example | Example AI | Direct competitor — voice agents for restaurants |
```

## Industry block-list

Verticals that engage with Jules's content but aren't ICP. Don't reactivate; log and move on.

- Software consultancies (they often engage with AI content but don't match hospitality ICP)
- AI agencies (peer / competitor; don't pitch)
- Academia / research institutions
- Student / job seekers

## Individual block-list (if any)

People who should never receive reactivation for any reason. Typically: prospects who explicitly asked to be left alone, former employees of active clients, personal contacts Jules wants kept separate from sales flow.

*(Empty seed.)*

## Rule

If an engager matches any entry here → LinkedIn Engagement logs them as `Tier: excluded — <reason>` in `engagement-log.md` and stops. No flag to Lead Researcher, no reactivation, no sheet entry.
