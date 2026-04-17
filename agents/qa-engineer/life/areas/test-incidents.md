# Test Incidents

Append-only log of every bug QA caught (or missed). Sibling of Deployment Engineer's [`deploy-incidents.md`](../../deployment-engineer/life/areas/deploy-incidents.md) — different scope: this tracks code-level regressions; that one tracks infrastructure.

## Format

```
### YYYY-MM-DD — <one-line summary>

**Detected by:** <promotion-gate | fleet-regression | nightly | live-client-report>
**Skill(s):** <classifier | qa-handler | small-booking | complex-booking | integration | email-crawler>
**Amis version:** <v>
**Affected clients:** <slugs, or "none — caught pre-rollout">
**Bug issue:** <link>
**Repro steps:** <numbered>
**Root cause (once known):** <one or two sentences from Founding Engineer>
**Test added:** <test name + matrix row>
**Fixed in:** <:candidate SHA, later promoted to :stable vX.Y.Z>
**Re-run verified green on:** <YYYY-MM-DD>
```

---

## Log

_(no incidents yet — first entry lands here when a bug is caught)_

---

## Miss log (bugs that escaped to production)

When a bug reaches a live client that the promotion gate should have caught, log it here with a retro: what scenario should have been in the test matrix, added now as a permanent row.

_(none yet)_
