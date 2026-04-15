# Lead Magnet Pipeline — Akira Agent

> Ranked list of candidate magnets. Updated weekly (or when an idea lands). Head of Marketing approves before build starts.
>
> **Last updated:** 2026-04-15

## Ranking criteria (0–5 on each)

- **ICP pull** — does the target persona actively want this?
- **Signal quality** — do signups become qualified leads (vs. freebie hunters)?
- **Buildability** — can we ship in Tally alone, or does it need Founding Engineer?
- **Uniqueness** — does this exist already? (If competitors have it, why is ours better?)

## Active pipeline

### 1. Email Replacement ROI Calculator  **[FLAGSHIP — ship first]**

- **Score:** ICP pull 5 · Signal 5 · Buildability 5 · Uniqueness 4. **Total 19/20.**
- **Target:** multi-location Nordic restaurants / hotels where staff spend meaningful time on email (inquiry triage, group bookings, supplier comms).
- **Core pitch (from company-goal.md):** *"You're paying 6,000–12,000 SEK/month for someone to handle emails. We do it for 3,700 SEK and it never calls in sick."*
- **Inputs:**
  - Hours/week spent on email by staff (seed default: 5–10h).
  - Average hourly rate of the staff member(s) handling it (seed default: 280 SEK/h).
  - Number of locations (for multi-location ops the calc scales).
- **Calculation:**
  - Monthly staff cost = hours/week × 4.33 × hourly_rate.
  - Akira email skill cost: 3,700 SEK / month / location.
  - Setup cost: 20,000–57,000 SEK (use 35K mid-range as default).
  - **Monthly saving = staff_cost − akira_cost.**
  - **Payback period = setup_cost / monthly_saving** (target: 3–5 months).
  - **Year 1 net savings = (monthly_saving × 12) − setup_cost.**
- **Output page (shown after email capture):**
  - Their personalized savings ("Du sparar X SEK/mois" or equivalent in their language).
  - Payback period in months.
  - Year 1 net savings.
  - A 3-paragraph explainer of what the email skill actually does.
- **Capture:** Tally form with email field BEFORE showing the personalized output.
- **Follow-up:** Beehiiv 3-email nurture over 10 days. If monthly saving >8,000 SEK → subtask to Head of Sales to prioritize.
- **Build target:** Live in Tally within 7 days. Tally's conditional logic handles the math natively.
- **Languages:** Swedish + English (launch); French (later via Jules); never Norwegian.
- **Source:** the vision's canonical pitch — Email skill is the thin-edge wedge for new clients.

### 2. "10 Questions Before Replacing Your Phone Line with an AI Agent" (diagnostic quiz)

- **Score:** ICP pull 4 · Signal 4 · Buildability 5 · Uniqueness 3. **Total 16/20.**
- **Target:** owner/ops director evaluating voice automation.
- **Format:** 10-question Tally quiz → personalized readiness score → email gate for the detailed recommendations PDF.
- **Why it converts:** positions Akira as consultative; surfaces whether the lead is ready or too early.

### 3. Nordic Hospitality AI Benchmark Report 2026 (gated PDF)

- **Score:** ICP pull 3 · Signal 4 · Buildability 4 · Uniqueness 5. **Total 16/20.**
- **Target:** operators curious about peer adoption.
- **Format:** short report — survey of publicly observable Nordic restaurant/hotel AI adoption, reservation volumes, common pain points. Gated via Tally.
- **Source data:** Apollo + LinkedIn public signals + web search. Lead Researcher and LinkedIn Engagement contribute source material.

## Shipped / Live

*None yet.*

## Killed

*None yet.*
