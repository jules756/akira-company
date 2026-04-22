# HEARTBEAT — Product Manager

Daily schedule + event-driven (mentions, assignments, approvals). Monday wake adds the weekly portfolio review.

## 0. Env

Required:
- `PAPERCLIP_*` standard
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID` (Composio must have Notion write access on `Agents (The Bots)` DB)
- `NOTION_CRM_DB_ID` (read)
- `NOTION_AGENTS_BOTS_DB_ID` = `23979a0a-12ed-4714-a5bb-0d08818cd53b`
- `NOTION_PROPOSALS_DB_ID` = `54d87250-5890-4d4f-a6b6-d77cd8cfdb65`
- `GOAL_ID`

Missing → comment, `blocked`, exit.

## 1. Identity + wake

```
GET /api/agents/me
```

## 2. Read context

- [`life/areas/delivery-playbook.md`](life/areas/delivery-playbook.md) — your SLAs, templates, escalation rules.
- [`../head-of-sales/life/areas/notion-protocol.md`](../head-of-sales/life/areas/notion-protocol.md) — the CRM rules (you read CRM but never write).
- Scan for recent `SCHEMA CHANGE` comments from CRM Steward on "CRM Daily" issue — keep your understanding of CRM stages current.

## 3. Approvals + HR loop

Standard handling.

## 4. Assignments

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={your-id}&status=todo,in_progress,in_review,blocked
```

Typical types:
- `Kick off delivery for <Client>` (from Head of Sales' stage-change comment, or auto-created by your daily scan).
- `Feedback session notes — <Client>` (from Jules or Head of Sales).
- `Portfolio review` (Monday schedule).

## 5. Daily scan — promotion gap check

1. Query Notion `Companies (CRM)` for `Pipeline Status=Active`.
2. Query Notion `Agents (The Bots)` for all Bot rows.
3. For each Active Company, check if a Bot row exists (match via Proposal relation → Client).
4. **If missing →** create the Bot row + run the skill-library audit:
   - **4a. Create the Bot row:**
     - Agent Name: concise `<Client Name> — Voice Bot` (adjust if multiple bots per client).
     - Build Status: `Proposal`.
     - Tech Stack Tags: initial guess from Proposal scope.
     - Proposal: link the Proposal page.
     - MRR: from Proposal's Proposed MRR rollup.
   - **4b. Read the capability library.** Check the document in the projects. Know what's `production`, `beta`, `planned`.
   - **4c. Audit the Proposal against the library.** For each capability the client needs, classify:
     - `Library: full` → existing skill deploys as-is.
     - `Library: partial` → existing skill needs extension.
     - `Library: none` → net-new capability required.
     - `Out of scope` → not an Akira deliverable; flag to Head of Sales.
   - **4d. Append a `Skill fit audit` section to the Bot page body** with the per-capability classification. This is the decision record.
   - **4e. Route work based on the audit:**
     - For `Library: full` capabilities → create a subtask for Implementation Engineer: `Deploy <skill-slug> for <Client>`. Skip the CTO-plan step for these — the skill already has a deploy pattern.
     - For `Library: partial` or `Library: none` → create a subtask for CTO: `Plan: <extension / new skill> for <Client>` with the specific gap from the audit. CTO decides whether to extend or build new.
     - For `Out of scope` → create a subtask for Head of Sales to resolve with the client.
   - **4f. Create a subtask for yourself:** `Orchestrate: <Client> Bot delivery` — your own coordination track.
   - **4g. Append to the capability library document's "Capability requests (incoming)" table** any `partial` / `none` classifications so Skilled Builder + CTO see the aggregate demand.

## 6. Status-transition work

For each Bot in-flight, check its readiness signals and transition when evidence is clear:

- **Proposal → Building**: CTO's plan subtask is done + Implementation Engineer has acknowledged with a "will begin" comment. Transition via Composio write on the Bot page. Update Timeline. Create subtasks to Implementation Engineer (configure) and UX Designer (dashboard design + voice UX review when prompt is ready).
- **Building → Feedback**: Implementation Engineer comments `Setup complete. Ready for Feedback session.` with test evidence and monitoring proof. Transition. Schedule the feedback session (subtask to Follow-Up Manager to book Cal.com with Jules + client).
- **Feedback → Production**: Feedback session notes captured in Bot page Feedback Log, all items dispositioned (done / deferred with reason / rejected with reason). Jules has approved go-live (comment on the Bot subtask). Transition. Notify Implementation Engineer to enable production monitoring alerts and the client's live number.
- **Production → Maintenance**: 60 days after going live with no incidents in the last 14 days. Routine transition.

Every transition logs:
- Date
- Evidence (subtask URLs, comment IDs)
- Short reason

To the Bot page body under `Timeline`.

## 7. Write client-facing summaries

When transitioning to Feedback or Production, write a 3-paragraph summary in the Bot page body under a new `Client Summary` section:

```
## Client Summary — <date> — <stage reached>

<paragraph 1: what we built + why it solves the problem from the Proposal>

<paragraph 2: how they review / what they'll experience>

<paragraph 3: what's next (next session date, next milestone)>
```

Notify Head of Sales with a comment: `Summary ready for <Client> — Jules can forward.` Head of Sales relays to Jules.

## 8. Feedback session capture

When Jules / Head of Sales shares feedback from a client session:

1. Append to the Bot's `Feedback Log` (page body) — date, who raised, item, proposed disposition.
2. For each item, decide route:
   - **Config change** → subtask for Implementation Engineer.
   - **UI change** → subtask for UX Designer.
   - **Platform change** → subtask for Founding Engineer (CC CTO).
   - **Scope change** → comment on the Bot subtask + escalate to Head of Sales (may require a Proposal amendment).
3. Track each to completion. Update Feedback Log with disposition when resolved.

## 9. Monday portfolio review

1. Pull all Bot rows. Bucket by status.
2. Compute: count per status, aging (days in current stage), total live MRR, proposed MRR (Building), at-risk Bots (aging > threshold).
3. Identify Bots needing a Jules decision.
4. Post DIGEST on a standing `Portfolio Review` issue owned by CTO, cc CEO:

```
PORTFOLIO — Week of YYYY-MM-DD

Live MRR: <sum of Production MRR> SEK
Building MRR (committed): <sum of Building Proposed MRR> SEK

Status counts:
- Proposal: N
- Building: N  
- Feedback: N
- Production: N
- Maintenance: N

At risk (aging beyond SLA):
- <Client> — <stage> — <days aging> — <blocker>

Needs Jules:
- <Client> — <decision or approval required>
```

## 10. Exit

- Comment on `in_progress` work.
- `X-Paperclip-Run-Id` on mutating Paperclip calls.
- Clean exit.
