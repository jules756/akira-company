# Rubric — Web Designer

Scored weekly by Auditor (web-design is on-event). Each dimension 0–5.

### 1. Design-system consistency (0–5)
- **5** — Every page on akira-agent.com pulls from `web-design-system.md`. No one-off styles. Hero / type / spacing / color discipline holds.
- **3** — System mostly applied; some inconsistencies.
- **1** — Ad-hoc page styling.
- **0** — System undefined or ignored.

### 2. Conversion-orientation (0–5)
- **5** — Every page designed has a single primary CTA, conversion-optimized hero, evidence section (logos / testimonials / metrics), and a clear next step. Layouts pass conversion-psychology checks.
- **3** — Most pages have CTA + evidence; some lack hierarchy.
- **1** — Pretty but not converting.
- **0** — No CTA discipline.

### 3. Speed + responsiveness (0–5)
- **5** — Designs deliver Core Web Vitals green on mobile (LCP < 2.5s, CLS < 0.1, INP < 200ms) at first ship. Mobile-first considered, not patched in.
- **3** — Mostly green, occasional mobile issues caught post-deploy.
- **1** — Mobile breaks frequent.
- **0** — Mobile broken or unconsidered.

### 4. Turnaround (0–5)
- **5** — Design requests from Web Writer (article OG image), Lead Magnet Creator (landing page), or Head of Marketing (campaign page) shipped within 1–2 heartbeats.
- **3** — Some requests age >3 days.
- **1** — Backlog grows.
- **0** — Unresponsive.

### 5. v0 / Vercel ecosystem usage (0–5)
- **5** — Uses v0.dev for rapid landing-page generation, Vercel-native components, vercel-labs design-guidelines patterns. Doesn't reinvent shadcn / Tailwind primitives.
- **3** — Some Vercel-native usage.
- **1** — Building from scratch every time.
- **0** — Not using the stack.
