# Client Health — Customer Success Manager

> Living scorecard. One section per Production / Maintenance client. Updated every heartbeat. Append-only for status changes (dated).
>
> Reads from: `Agents (The Bots)` DB, `Companies (CRM)`, Implementation Engineer's per-client incidents.md + runbook.md, Bot page Feedback Logs.
>
> **Last updated:** 2026-04-16 (seed — no Production clients yet)

## Summary (rolling)

| Metric | Value |
|---|---|
| Production clients | 0 |
| Maintenance clients | 0 |
| Total MRR | 0 SEK |
| NRR (rolling 3m) | N/A |
| Churn this quarter | 0 |
| Referrals requested this quarter | 0 |
| Referrals received (pipeline) | 0 |

## Per-client scorecards

*(Empty seed — first row appears when PM flips a Bot to `Build Status=Production`.)*

### Template

```
## <Client Name>
- **Bot URL:** <Notion page URL>
- **Company URL:** <Notion Company page URL>
- **Production-live date:** YYYY-MM-DD
- **Months in Production:** <N>
- **Skills deployed:** [Voice, Email, ...]
- **Skills eligible (from library sweep):** [Social, Reviews]
- **Current MRR:** <SEK>
- **NRR trajectory:** stable / expanding / at-risk
- **Days since last incident:** <N>
- **Days since last client-initiated touch:** <N>
- **Last feedback session:** YYYY-MM-DD — <outcome>
- **Referral asked:** YYYY-MM-DD / never
- **Referrals received:** [<Company1>, <Company2>]
- **Upsells in flight:** [<Skill> with Head of Sales, opened YYYY-MM-DD]

### Status log
- YYYY-MM-DD — moved to Production. Voice skill live. Initial MRR 3,700 SEK.
- ...
```
