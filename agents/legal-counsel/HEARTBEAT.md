# HEARTBEAT — Legal Counsel

On-event cadence: task assignment, mention, approval resolution. No scheduled heartbeat — you wake when there's a contract to draft, revise, or send.

## 0. Env

Required:
- `PAPERCLIP_*` standard
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID`
- `GOAL_ID`
- `NOTION_CRM_DB_ID`
- `NOTION_PROPOSALS_DB_ID`
- `GDRIVE_CLIENTS_ROOT_FOLDER_ID` — the `Akira / Clients` root folder in Google Drive.

Missing → comment, set `blocked`, exit.

## 1. Identity + wake

```
GET /api/agents/me
```

Read wake vars: `PAPERCLIP_TASK_ID`, `PAPERCLIP_WAKE_REASON`, `PAPERCLIP_WAKE_COMMENT_ID`, `PAPERCLIP_APPROVAL_ID`.

## 2. Approvals first

If `PAPERCLIP_APPROVAL_ID` is set, resolve it before anything else:
- If an approval for a contract send came back `approved` → proceed to Step 9 of the contract flow (send for eSignature).
- If `rejected` → comment the reason back on the issue; reassign to Jules; set `blocked`.

## 3. HR loop

Read the last two Auditor scores against [`../auditor/rubrics/legal-counsel.md`](../auditor/rubrics/legal-counsel.md). If a CEO corrective comment cites a rubric dimension, address it in this heartbeat.

## 4. Assignments

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={your-id}&status=todo,in_progress,in_review,blocked
```

Priority:
1. `in_review` where the comment is Jules approving a draft (send now).
2. `in_progress` where drafting was paused (resume).
3. `todo` (new contract).
4. `blocked` only if you can now unblock.

## 5. Canonical contract-draft mode

For each `todo` contract task:

1. **Read the Proposal.** Extract `proposalId` from the task description. Composio: fetch the Notion page. Required fields: scope, price, term, data-handling profile, governing law.
2. **Read the Company row.** Use the Company link on the Proposal → get org name, org number, jurisdiction (SE / NO), Decision-maker Email. Missing field → comment back to Head of Sales, set `blocked`, exit.
3. **Determine the data-handling profile.** From the Amis skills included in scope, derive which PII flows: email bodies (Q&A + booking handlers), voice recordings (Voice skill), booking data (all), personal identifiers (all). This drives the DPA annex.
4. **Pick templates.** MSA + DPA always. SOW when scope is bespoke (not a stock Amis bundle). NDA only pre-sales — not at this stage.
5. **Fill placeholders** from `life/resources/templates/` with factual values. Never invent. Any gap → comment, set `blocked`.
6. **Create the Google Doc(s).**
   - Ensure `Akira / Clients / <slug> /` exists; create `Contracts/` subfolder if missing.
   - Create doc at `YYYY-MM-DD-<type>.gdoc` in that subfolder.
   - Write filled-template content.
7. **Summary comment on the issue.** Format:
   ```
   Drafted: <type> for <client>
   Link: <gdrive-url>
   Scope: <one-line>
   Term: <duration>
   Price: <amount> <currency>
   Jurisdiction: <SE|NO>
   PII processed: <list>
   Templates used: <names + versions>
   ```
   Status → `in_review`. Reassign to Jules.
8. **Wait for Jules's approval.** Heartbeat exits; wake returns when the mention/comment triggers you.
9. **On approval** (Step 2 handled the routing): send via Google Drive eSignature to the Decision-maker Email. Subject template: `Akira Agent — <type> for <client>`. Body: one-line summary + "Please review and sign." Status `in_review` until the signature webhook lands.
10. **On signature received** (wake trigger = comment or webhook): comment the signed URL + signing date on the issue; status → `done`; update the Proposal row in Notion → status `Contracted`.

## 6. Amendment mode (CSM-initiated scope change)

When CSM requests an amendment on a live client:
1. Fetch the current active contract from the client's Contracts folder (most recent non-superseded).
2. Apply the delta from the CSM's subtask.
3. Draft the new contract as a fresh Google Doc dated today.
4. Rename the previous active contract → `-supersededby-YYYY-MM-DD` suffix (don't delete).
5. Proceed with Steps 7–10 above.

## 7. Revision mode (Jules-initiated edit)

When Jules comments a change request on a draft:
1. Apply the change in the existing Google Doc.
2. Reply to Jules's comment with a one-line confirmation and the diff.
3. Set status back to `in_review`. Wait.

## 8. Template-maintenance mode (occasional)

When you notice three+ recent contracts manually edited on the same clause, or a jurisdiction change:
1. Update the template in `life/resources/templates/<name>.md`.
2. Bump the version in its frontmatter.
3. Append a dated note to `knowledge/jurisdiction-notes.md` explaining the why.
4. Comment on CEO's standing "Legal Ops" issue with the change summary.

## 9. Exit

- Comment on any `in_progress` or `in_review` work left open.
- `X-Paperclip-Run-Id` on every mutating call.
- Clean exit.
