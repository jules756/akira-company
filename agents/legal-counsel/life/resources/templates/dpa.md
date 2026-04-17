---
name: dpa
title: Data Processing Agreement — GDPR
jurisdiction: EEA (SE + NO)
version: 1.0
last_updated: 2026-04-17
applies_to: All clients
---

# DATA PROCESSING AGREEMENT

**This Data Processing Agreement ("DPA")** is entered into as of **{{effective_date}}** between:

**Akira Agent** (the "Processor") — organisation number **{{akira_org_number}}**.

and

**{{client_legal_name}}** (the "Controller") — organisation number **{{client_org_number}}**.

This DPA is an integral part of the Master Services Agreement between the Parties dated **{{msa_date}}**.

---

## 1. Definitions

Terms not defined in this DPA have the meaning set out in the EU General Data Protection Regulation (GDPR), Regulation (EU) 2016/679.

## 2. Subject Matter and Duration

2.1 **Subject matter.** The Processor processes personal data on behalf of the Controller in order to deliver the services described in the MSA.

2.2 **Duration.** This DPA is in effect for the duration of the MSA. Obligations in respect of data already processed (e.g. deletion, return) survive termination.

## 3. Nature and Purpose of Processing

3.1 The Processor processes personal data solely for the purpose of delivering the agreed services. Processing activities include:

- Ingestion and indexing of historical email messages from Controller-authorised mailboxes (Email Crawler).
- Reading, classifying, and responding to incoming emails.
- Creating, reading, and updating bookings in Controller-authorised systems.
- Sending notifications and approval requests to Controller-designated channels.
- Logging activity for operational monitoring and debugging.

3.2 Processing is performed only on the Controller's documented instructions, including any instructions given via the Controller's configuration interface.

## 4. Categories of Personal Data

The following categories of personal data are processed:

- **Identification data:** names, email addresses, phone numbers of the Controller's customers and staff.
- **Booking data:** party size, date, time, dietary notes, accessibility requirements.
- **Communication content:** bodies of emails exchanged with the Controller's customers.
- **Metadata:** timestamps, language, device information where attached to messages.
- {{additional_pii_categories}}

## 5. Categories of Data Subjects

- The Controller's customers and prospects.
- The Controller's staff to the extent they appear in processed communications.

## 6. Sub-processors

6.1 The Controller authorises the Processor to engage the sub-processors listed in **Annex A**.

6.2 The Processor shall impose data-protection obligations equivalent to those in this DPA on any sub-processor it engages.

6.3 The Processor shall notify the Controller of any intended addition or replacement of sub-processors at least thirty (30) days in advance, giving the Controller the opportunity to object on reasonable grounds.

## 7. Transfers outside the EEA

7.1 Personal data is stored and processed within the European Economic Area. Any transfer outside the EEA requires the Controller's prior written consent and appropriate safeguards under Chapter V GDPR (Standard Contractual Clauses, adequacy decision, or equivalent).

## 8. Security

8.1 The Processor implements technical and organisational measures appropriate to the risk, including:

- Per-client environment isolation (dedicated virtual machine and database per Controller).
- Encryption in transit (TLS 1.2+) and at rest.
- Role-based access control for Provider personnel.
- Secret management via environment-isolated stores; no credentials in source code.
- Operational logging and anomaly detection.
- Periodic security review of deployed environments.

## 9. Data Subject Rights

9.1 The Processor shall assist the Controller in fulfilling its obligations to respond to data subject requests (access, rectification, erasure, portability, restriction, objection). Reasonable assistance is included in the MSA fees.

## 10. Breach Notification

10.1 The Processor shall notify the Controller without undue delay, and in any event within forty-eight (48) hours, after becoming aware of a personal data breach affecting the Controller's data.

## 11. Audit

11.1 The Controller may audit the Processor's compliance with this DPA no more than once per year, on reasonable notice, during normal business hours, and under a confidentiality undertaking. A written summary of controls may be accepted in lieu of an on-site audit at the Processor's discretion.

## 12. Return and Deletion

12.1 Upon termination of the MSA, the Processor shall, at the Controller's choice, return or delete all personal data processed on the Controller's behalf within sixty (60) days. Backups that cannot be practically isolated are deleted in the normal backup-rotation cycle.

## 13. Governing Law

13.1 This DPA is governed by the law specified in the MSA ({{governing_law}}).

---

## Annex A — Sub-processors

| Sub-processor | Purpose | Location |
|---|---|---|
| Hetzner Online GmbH | Virtual machine hosting | Germany / Finland (EEA) |
| Supabase, Inc. | Managed database + auth (per-client project) | EU region |
| Composio | OAuth orchestration for Controller-authorised third-party services | EU region where available |
| Vercel, Inc. | Static hosting for Controller-facing portal | EU edge regions |
| {{additional_subprocessors}} | {{purpose}} | {{location}} |

## Annex B — Authorised Sub-processors Specific to This Engagement

{{engagement_specific_subprocessors}}

---

**Signed on behalf of Akira Agent (Processor):**

Name: ______________________________
Date: ______________________________

**Signed on behalf of {{client_legal_name}} (Controller):**

Name: **{{client_signatory_name}}**
Date: ______________________________
