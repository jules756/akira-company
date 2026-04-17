# Rubric — Legal Counsel

Scored per-contract (and weekly summary) by Auditor. Each dimension 0–5.

### 1. Fidelity to source (0–5)
- **5** — Every filled value in every draft traces to a field in the Proposal or the CRM. No invented clauses. No invented terms.
- **3** — One or two placeholders filled with reasonable defaults, flagged in the summary comment.
- **1** — Multiple placeholders filled without source.
- **0** — Fabricated clauses.

### 2. Filing discipline (0–5)
- **5** — Every contract lands at `Akira / Clients / <slug> / Contracts / YYYY-MM-DD-<type>.gdoc`. Superseded versions correctly suffixed. No loose docs outside the tree.
- **3** — Correct folder, but naming inconsistency (e.g., missing date prefix).
- **1** — Docs land outside the client subtree or overwrite without suffixing.
- **0** — Deletion of a superseded contract.

### 3. Approval gate discipline (0–5)
- **5** — Zero contracts sent without Jules's explicit approval comment. Every send is traceable to a prior approval.
- **3** — One edge-case send (e.g., a revision to an already-approved draft).
- **1** — Multiple sends without recorded approval.
- **0** — Any contract sent that was not approved. Zero-tolerance dimension; one incident drops the monthly score by two full points.

### 4. Summary-comment quality (0–5)
- **5** — Every draft comment includes: link, scope, term, price, jurisdiction, PII processed, templates + versions used. Scannable in under 20 seconds.
- **3** — Comment present, missing one field (usually PII).
- **1** — Comment is just a link.
- **0** — No comment.

### 5. Template maintenance (0–5)
- **5** — When a clause has been manually edited on three+ drafts, the template gets updated with a version bump and a jurisdiction-notes entry. Library stays current.
- **3** — Noticed the pattern but didn't update the template.
- **1** — Repeatedly edits the same clause per client, no template learning.
- **0** — Template drift actively creates risk (clauses on new drafts contradict older signed contracts).
