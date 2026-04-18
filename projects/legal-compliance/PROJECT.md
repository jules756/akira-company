---
name: "Legal & Compliance"
slug: legal-compliance
status: in_progress
color: "#94a3b8"
---

# Legal & Compliance

Every contract, amendment, and data-protection obligation Akira carries lives here. Legal Counsel owns drafting + filing + eSignature dispatch; Jules is the final signer on everything. GDPR obligations flow through this project as well since Akira processes PII for every client (email bodies, booking data, voice recordings).

## Scope

### Contract lifecycle
- **New contracts** on `Signed` proposals — MSA + DPA always; SOW when scope is bespoke. Filed in Google Drive at `Akira / Clients / <slug> / Contracts / YYYY-MM-DD-<type>.gdoc`, sent via Google Drive eSignature after Jules approves.
- **Amendments** for live-client scope changes — when CSM flags a material scope change (new skill, price change, term extension), a supersession contract is drafted; prior active is suffixed `-supersededby-YYYY-MM-DD` and kept.
- **NDAs** — rare; pre-sales only when a prospect asks before the Proposal stage.

### Template library
- SE + NO jurisdictions covered by separate MSA templates.
- DPA aligned with EU GDPR via Sweden's `Dataskyddslagen` / Norway's `Personopplysningsloven`.
- Template updates trigger a version bump + a note in the jurisdiction-notes log.

### Data protection + GDPR
- DPA annex always reflects the exact PII the client's Amis skills process.
- Per-client data-export requests (data subject rights) route through this project.
- DPA retention during churn teardown coordinated with Deployment Engineer.

### Hard approval gate
- Legal Counsel never sends a contract for eSignature without Jules's explicit approval comment on the draft. Auditor's rubric enforces this (zero-tolerance dimension).

## Primary agents

- **Legal Counsel** (owner) — drafts, files, sends after approval.
- **Head of Sales** — triggers new-contract workflow when a Proposal is marked `Signed`.
- **Customer Success Manager** — triggers amendment workflow on live-client scope changes.
- **CEO** — reviews contract volume + exception cases.
- **Jules** — the human signer. Never bypassed.

## References

- [`jurisdiction-notes.md`](../../agents/legal-counsel/life/areas/jurisdiction-notes.md) — SE vs NO quirks.
- [`life/resources/templates/`](../../agents/legal-counsel/life/resources/templates/) — MSA-SE, MSA-NO, DPA, SOW, NDA.

## Success criteria (ongoing)

- Every signed Proposal has a filed + signed MSA + DPA within 5 business days.
- Zero contracts sent without Jules's explicit approval comment.
- Every superseded contract retained with proper suffix; zero deletions.
- Template drift detected within 3 reuses of a hand-edited clause (→ template update).
- Data-subject access requests acknowledged within 48 hours per GDPR.
