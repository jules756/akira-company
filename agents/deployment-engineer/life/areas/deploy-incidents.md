# Deploy Incidents

Append-only log of every deployment incident: failed provisioning, orphan resource, regression rollback, teardown issue. Never edit past entries; supersede with a new dated entry if something is corrected.

## Format

```
### YYYY-MM-DD — <one-line summary>

**Affected:** <VM IDs, client slugs, Amis versions>
**Trigger:** <what fired the incident>
**Detected:** <how — hourly scan / Implementation Engineer / Auditor / client>
**Action taken:** <what you did>
**Root cause (if known):** <one or two sentences>
**Follow-up:** <Founding Engineer issue link if a skill fix is needed>
```

---

## Log

_(no incidents yet — first entry lands here when it happens)_
