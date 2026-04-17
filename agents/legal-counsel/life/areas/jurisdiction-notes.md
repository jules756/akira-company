# Jurisdiction Notes — SE vs NO

Live reference for drafting contracts in Sweden and Norway. Append as new lessons arrive. Never delete — strike through or supersede.

## Sweden (SE)

- **Legal framework:** Swedish contract law (`Avtalslagen`, 1915). Commercial contracts default to freedom of contract; few mandatory provisions for B2B SaaS.
- **Governing law clause:** `This Agreement shall be governed by the laws of Sweden.` Default venue: Stockholm District Court (`Stockholms tingsrätt`) unless parties agree otherwise.
- **VAT:** 25% standard. Invoicing must show `Moms (25%)` separately. Org number format: `XXXXXX-XXXX` (10 digits with a hyphen).
- **Termination notice:** default 3 months notice is common for annual B2B contracts; explicit clause required.
- **Consumer vs business:** Akira's clients are all businesses (restaurants/hotels as legal entities), so consumer protections under `Konsumentköplagen` do not apply. Always confirm the client is contracting as a business entity.
- **GDPR:** Sweden applies EU GDPR directly via `Dataskyddslagen` (2018:218). DPA is mandatory when any personal data flows — which is every Amis engagement.
- **Language:** contracts are enforceable in English for B2B. Swedish translation optional; if provided, the English version governs in case of conflict (add clause).

## Norway (NO)

- **Legal framework:** Norwegian Contracts Act (`Avtaleloven`, 1918) + case law tradition similar to SE. EU-equivalent via EEA agreement for most commercial provisions.
- **Governing law clause:** `This Agreement shall be governed by the laws of Norway.` Default venue: Oslo District Court (`Oslo tingrett`).
- **VAT:** 25% standard (MVA). Invoicing must show MVA separately. Org number format: 9 digits, often written with spaces (`XXX XXX XXX`).
- **GDPR:** Norway applies GDPR via the Norwegian Personal Data Act (`Personopplysningsloven`, 2018) under the EEA agreement. DPA required. Datatilsynet (NO DPA) is the supervisory authority for NO-based processing.
- **Data residency:** Norwegian clients with public-sector adjacencies occasionally require data processing inside the EEA. Hetzner's Helsinki (FI) and Falkenstein/Nuremberg (DE) datacenters satisfy this; US-only processing does not.
- **Termination notice:** 3 months is standard for annual contracts; shorter terms (monthly) use 30 days.
- **Language:** English contracts are enforceable for B2B. Bokmål translation optional; add a clause stating English governs in conflict.

## Both jurisdictions — defaults we keep consistent

- **Governing law** mirrors the client's jurisdiction (SE client → SE law; NO client → NO law). Never cross-border by default.
- **DPA** is always included, not optional. The annex lists exactly which PII categories flow for the scope of work.
- **IP assignment:** Akira retains ownership of generic skills + library; the client receives a non-exclusive license for the duration of the subscription. Client-specific knowledge indices and config belong to the client.
- **Liability cap:** 12 months of fees paid — default; never waived without Jules approval.
- **Term:** 12-month initial term, auto-renew annually with 3 months notice to cancel. Monthly subscription price quoted in the Proposal.
- **Price currency:** SEK for SE clients, NOK for NO clients. Never quote in EUR unless the Proposal explicitly does.

## Change log

- **2026-04-17** — Initial notes created alongside legal-counsel agent. Derived from SE and NO commercial-contract defaults + EU GDPR alignment. No specific incidents yet; update as real client contracts surface edge cases.
