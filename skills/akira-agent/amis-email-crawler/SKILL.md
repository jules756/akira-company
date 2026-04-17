---
name: amis-email-crawler
slug: amis-email-crawler
key: akira-agent/amis-email-crawler
description: "Use when running the one-time historical email extraction for a new client, per Email Handler PRD §3. Crawls 12 months of client Gmail via Composio, processes week-by-week (48 passes) to avoid token overflow, and writes the structured knowledge index (tone.md, questions.md, booking_patterns.md, edge_cases.md, INDEX.md) to the client's Supabase. Invoked by Deployment Engineer during client onboarding, typically as a Hermes-adapter continuous agent. Not a recurring job — runs once per client, then exits."
metadata:
  sources:
    - kind: internal
      path: skills/akira-agent/amis-email-crawler
      repo: jules756/akira-company
      trackingRef: main
---

# Amis Email Crawler

One-time historical email extraction. Implements Email Handler PRD §3.

## When to run

Exactly once per new client, during onboarding (PRD Step 1). Do not run again after the initial crawl — the ongoing email classifier + handler skills take over.

Re-runs are allowed only if:
- The knowledge index was corrupted and needs regenerating.
- The client explicitly requests a re-index (e.g., after a significant menu or operations change).

Both cases require CTO approval and a `re-crawl` tag in the task body.

## Parameters (from the invoking task)

```yaml
composio_entity_id: akira-<slug>
gmail_account: <client-email@domain>
date_range:
  start: <YYYY-MM-DD>   # 12 months ago
  end: <YYYY-MM-DD>     # today
passes: 48              # 52 weeks minus buffer for quiet weeks
target_supabase_url: <client-supabase-url>
target_supabase_service_key: <client-service-key>
classifier_keywords:    # override if client has non-English ops
  - reservation
  - table
  - booking
  - party size
  - group
  - event
```

## Execution model

Runs as a Hermes-adapter continuous job. Expected runtime: 2–6 hours depending on inbox volume.

Processes emails **week by week, oldest first**. Each weekly pass:
1. Queries Gmail via Composio for messages in the week's date range matching the keyword filter.
2. Incrementally refines the four output files (`tone.md`, `questions.md`, `booking_patterns.md`, `edge_cases.md`).
3. Commits the updated files to the client's Supabase.
4. Updates `INDEX.md` to describe what each file now contains.

Incremental-refinement is load-bearing: dumping 12 months into one LLM call causes token overflow + hallucination. Week-by-week keeps context tight and traceable.

## Outputs — the knowledge index

Written to the `knowledge_index` table in the client's Supabase (one row per file):

### `tone.md`
- Writing style (formal / casual / warm).
- Typical phrasings for greetings, closings, apologies.
- Language(s) detected + proportion (e.g., "Swedish 80%, English 20%").
- Signature format.

### `questions.md`
- Common questions the restaurant receives (accessibility, dog policy, hours, parking, dietary).
- Standard answers the restaurant has given, verbatim where possible.

### `booking_patterns.md`
- How small bookings were handled (typical flow, confirmation wording).
- How complex bookings were handled (approval chain, menu selection, pre-payment).
- Typical follow-up sequences (day-before reminder, thank-you).

### `edge_cases.md`
- Complaints and how they were resolved.
- Unusual requests (allergies, celebrations, private rooms, VIPs).
- Escalation patterns (when the owner was looped in).

### `INDEX.md`
- Master manifest. For each of the four files: summary of contents + when the agent should reference it.
- The classifier + handler skills read `INDEX.md` first at startup to know which files matter for a given email.

## Client validation handoff

When all 48 passes complete:
1. Comment on the onboarding issue:
   ```
   Crawler complete.
   Files written: tone.md (<N chars>), questions.md (<N>), booking_patterns.md (<N>), edge_cases.md (<N>), INDEX.md (<N>).
   Coverage: <M weeks with booking-related emails> out of 48.
   Reassigning to Implementation Engineer for client validation (PRD Step 2).
   ```
2. Reassign to Implementation Engineer.
3. Exit the Hermes job.

Implementation Engineer guides the client through reviewing the files via the client portal.

## Security + data handling

- Gmail access is OAuth-scoped via Composio — the crawler never holds raw Gmail credentials.
- Emails are processed in-memory and never persisted outside the client's Supabase.
- The crawler itself runs on the client's VM (not a shared Akira VM) — all processing stays within the client's tenancy.
- Crawler logs (not email contents) are shipped to the client's Supabase `agent_logs` table for audit.

## Common failure modes

- **Gmail rate limit.** Composio backs off; the crawler retries with exponential delay. If 3 retries fail on the same week, skip + log; continue.
- **Token overflow on a huge week.** Fall back to day-level processing for that week.
- **Empty inbox weeks.** Normal (holiday seasons, pre-opening periods). Log "no booking-related emails" in `INDEX.md` for that week.
- **Non-English content.** The crawler respects `classifier_keywords`; Swedish/Norwegian keywords should be added by Implementation Engineer before kick-off if the client operates in those languages.
