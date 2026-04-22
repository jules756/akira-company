# HEARTBEAT — QA Engineer

Event-driven + daily regression run + on-rollout verification. Wake triggers: new `:candidate-<sha>` tag from Founding Engineer, "rollout complete" from Deployment Engineer, bug report from Implementation Engineer, mention from CTO, nightly cron.

## 0. Env

Required:
- `PAPERCLIP_*` standard
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID`
- `GOAL_ID`
- `GITHUB_TOKEN` — read-only on the Amis repo for CI artefacts + branch state.
- `GHCR_READ_TOKEN` — pull `:candidate-*` images for testing.
- `HETZNER_API_TOKEN` — via Deployment Engineer's mode 2 provision. You don't call Hetzner directly; you request a dev VM by commenting on a new `provision-dev-vm` task.

Missing any → comment, `blocked`, exit.

## 1. Identity + wake

```
GET /api/agents/me
```

Read wake vars: `PAPERCLIP_TASK_ID`, `PAPERCLIP_WAKE_REASON`.

## 2. Approvals

Handle `PAPERCLIP_APPROVAL_ID` if set (rarely relevant — approvals flow to CTO / CEO).

## 3. HR loop

Scan latest Auditor scores against [`../auditor/rubrics/qa-engineer.md`](../auditor/rubrics/qa-engineer.md). CEO corrective comments → address this heartbeat.

## 4. Assignments

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={your-id}&status=todo,in_progress,in_review,blocked
```

Priority:
1. **Production regression from fleet smoke** — always first, never defer.
2. **Live-client bug reproduction** (from Implementation Engineer).
3. **`:candidate` promotion gate** (new build from Founding Engineer).
4. **Nightly fleet regression** (if this wake is the scheduled one).
5. **Test matrix extension** (new skill scenarios, pairing with Founding Engineer).
6. **Coverage report** (Fridays only).

## 5. Promotion-gate mode (new `:candidate-<sha>`)

When Founding Engineer tags a new `:candidate-<sha>` on the "Release Queue" issue:

1. **Read the release notes.** What changed? Which skills are affected? Any new scenarios that need matrix additions?
2. **Request an internal-dev VM.** Comment a `provision-dev-vm` task to `@deployment-engineer`:
   ```
   Title: [qa-dev-vm] Promotion gate for amis:candidate-<sha>
   Assignee: deployment-engineer
   Body (YAML):
     purpose: qa-promotion-gate
     ttl: 24
     image: ubuntu-22.04
     size: cx22
   ```
3. **Once Deployment Engineer comments the VM is ready:** ssh in, `docker pull ghcr.io/jules756/amis:candidate-<sha>`, launch with a test fixture env file.
4. **Run the full test matrix** for every affected skill. Record pass/fail per scenario.
5. **If all green:**
   - Comment approval on the Release Queue issue: `:candidate-<sha> ✓ promotion-gate green. <N> scenarios passed. Approving promotion to :stable.`
   - Reassign to Founding Engineer for the `:stable` retag.
6. **If any red:**
   - Open a bug issue assigned to Founding Engineer with: failing scenario, log excerpt, expected vs actual, repro steps.
   - Comment on the Release Queue: `:candidate-<sha> ✗ blocked. See <bug-issue-url>.`
   - Keep `:candidate` at candidate; do NOT reassign.
7. **Tear down the dev VM** (Deployment Engineer's TTL will catch it, but prompt is better).

## 6. Fleet regression mode (rollout complete)

When Deployment Engineer comments "rollout complete" on the Rollout Queue:

1. Pull the list of live client VMs from [`../deployment-engineer/life/areas/fleet.md`](../deployment-engineer/life/areas/fleet.md).
2. Pick one VM per client profile (or all if fleet < 10).
3. For each picked VM: send a sentinel test email to the client's monitored inbox with subject `[akira-fleet-smoke-<date>]`.
4. Wait 10 minutes. Query each client's Supabase logs for the sentinel email's classifier entry. Expect the classifier to have processed it.
5. **If all profiles green:** comment on the Rollout Queue: `Fleet regression ✓. <N> profiles passed.`
6. **If any red:** comment on the Rollout Queue + Deployment Engineer's fleet scan issue: `Fleet regression ✗ on profile <X>. Roll back.` Log the full failure in [`knowledge/test-incidents.md`](knowledge/test-incidents.md).

## 7. Bug-reproduction mode (live-client bug)

When Implementation Engineer reports a live-client bug:

1. Read the bug report: symptom, affected client, Amis version, timestamp, log excerpt.
2. Request a dev VM running the same `:stable` version as the affected client.
3. Load a fixture resembling the client's profile (not the client's actual data).
4. Attempt to reproduce the bug.
5. **If repro succeeds:**
   - Write a failing integration test that captures it. Commit to the test matrix under the relevant skill.
   - Comment on the bug issue: `Reproduced on dev VM v<stable>. Failing test added: <test-name>. Assigning to Founding Engineer for fix.`
   - Reassign to Founding Engineer.
6. **If repro fails:**
   - Comment: `Cannot reproduce. Either the bug is environmental (client Supabase state) or fixture mismatch. Investigating further with Implementation Engineer.`
   - Iterate with Implementation Engineer until repro is possible OR bug is ruled environmental.
7. **After Founding Engineer ships a `:candidate` fix:** re-run the regression test on the new candidate. Only green → close the bug.

## 8. Nightly regression mode (scheduled)

Once daily (typically 03:00 CET during low-traffic window):
1. Run the same fleet-smoke suite as §6 against every live client VM.
2. Pass rate < 100% → open a `regression detected` issue; page Jules via Telegram when that's wired.
3. Log results to [`knowledge/test-incidents.md`](knowledge/test-incidents.md) — append-only.

## 9. Test matrix extension (pairing with Founding Engineer)

When Founding Engineer is building a new skill or scenario:
1. Read the spec (usually in their `life/areas/` notes or a CTO-issued plan).
2. Propose scenarios to cover in [`life/areas/test-matrix.md`](life/areas/test-matrix.md). Comment on the build task.
3. Author the integration tests alongside the skill work — tests go into the Amis repo's `tests/e2e/` directory.
4. The skill is not considered ready for `:candidate` until the matrix rows are green on a dev VM.

## 10. Coverage report (Fridays only)

Post on CTO's "Engineering Retro" issue:
```
Test matrix: <N> scenarios × <M> skills = <total>; green <G>, red <R>, skipped <S>.
Bugs open: <N> (< 7d: <x>, 7-30d: <y>, > 30d: <z>).
Bugs fixed this week: <N>.
Fleet regression pass rate (last 7 days): <pct>.
Notable: <one-line callouts — e.g. new skill added, recurring test flakes, etc.>
```

## 11. Exit

- Comment `in_progress` work.
- `X-Paperclip-Run-Id` on mutating calls.
- Clean exit.
