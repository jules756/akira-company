# Test Matrix — Amis Skills

Scenarios × skills. Each scenario is one row: a precondition, an input, the expected output, pass criteria. New skill → new rows. Bug found in production → new regression row tagged with the bug ID.

## Format

```
### <Skill> — <Scenario name>

**Precondition:** <fixture + config + client profile>
**Input:** <what the skill receives>
**Expected output:** <what the skill should produce>
**Pass criteria:** <exact checks — keyword presence, timing, side-effect on DB row, etc.>
**Added:** <YYYY-MM-DD>
**Last run:** <date + green/red>
**Tags:** <regression, bug-<id>, new-skill, edge-case>
```

---

## Classifier

_(scenarios populated as Founding Engineer ships the classifier skill)_

Seed scenarios to author with Founding Engineer:
- Q&A email in Swedish (dog policy question) → classified as `qa`.
- Q&A email in English (opening hours) → classified as `qa`.
- Small booking email (party of 4, tonight) → classified as `small-booking`.
- Complex booking email (party of 17, private room, pre-paid menu) → classified as `complex-booking`.
- Complaint email (cold food, wants refund) → classified as `complaint`.
- Mixed email (booking + complaint about previous visit) → classified as `complex-booking` with `complaint-flag`.
- Empty body email (only attachment) → classified as `unknown`, routed to human.
- Non-English-non-Swedish-non-Norwegian (French) → classified as `unknown`, routed to human.

## Q&A Handler

_(populated when the skill ships)_

Seed:
- Question that matches `questions.md` exactly → respond with stored answer in client tone.
- Question not in `questions.md` → respond with "I'll have someone get back to you" + flag for human.
- Multi-question email → answer what's stored, flag the unknowns.

## Small Booking Handler

_(populated when the skill ships)_

Seed:
- Booking request under the threshold, open slot → create booking, send confirmation.
- Booking request under threshold, slot not available → suggest alternatives from the next 3 available slots.
- Booking request on a blackout day → polite refusal with the next available date.
- Booking request with dietary notes → booking created + notes written to booking record.

## Complex Booking Handler

_(populated when the skill ships)_

Seed:
- Large party above approval threshold, needing approval → draft response + hold via Telegram for Jules/client approval.
- Private-room request → draft response with event-coordinator escalation.
- Pre-payment flow requested → create booking in held state, defer confirmation to client.

## Integration Skill

_(populated when the skill ships)_

Seed:
- Create booking via Cal.com API → booking row appears in Cal's admin.
- Send outbound email via Gmail → message appears in `Sent`.
- Push escalation to Telegram channel → message received at the configured chat.

## Email Crawler

_(populated when the skill ships)_

Seed:
- 12 months of booking-related emails → 48 weekly passes produce tone.md / questions.md / booking_patterns.md / edge_cases.md / INDEX.md.
- Empty-inbox week → weekly pass logs "no booking-related emails"; doesn't break the index.
- Rate-limit from Gmail → crawler backs off and retries; no data loss.

## Full-stack smoke (fleet regression)

One scenario per client profile:
- Restaurant profile: test email → classifier → small-booking handler → booking created → confirmation email sent.
- Hotel profile: test email → classifier → complex-booking handler → Telegram approval request.
- Multi-location group profile: test email → routed to correct location's config.

---

## Regression log

Every bug found in production becomes a permanent row here. Cross-linked to the bug issue.

| Added | Bug ID | Skill | Scenario | Status |
|---|---|---|---|---|
| _(first entry lands here when it happens)_ | | | | |
