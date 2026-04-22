# Web Design System — akira-agent.com

> Owned by Web Designer. Marketing-surface visual rules. Aligned on color + type with UX Designer's product `design-system.md`; different in spacing rhythm + page composition (web is whitespace-heavy, dashboards are density-heavy).
>
> **Last updated:** 2026-04-16 (seed)

## Aesthetic

**Nordic hospitality, marketing-funnel side.** Calm, confident, minimal. Whitespace-heavy. Single accent color per page. Typography does the heavy lifting. No gradient noise, no playful illustrations, no emoji-heavy headers.

## Color (aligned with product design system)

Same palette as `../ux-designer/life/areas/design-system.md`:
- **Background:** warm off-white `#FAFAF7` (paper) / `#0A0A0A` (dark).
- **Foreground:** `#111111` / `#F5F5F5`.
- **Accents (rotate per page):** Nordic blue `#2B4C7E`, forest green `#3A5A40`, amber `#C7822B`.
- **Neutrals:** `#E5E5E1` (stroke), `#78786F` (secondary text), `#333333` (primary text on paper).

One accent color per page. Don't mix.

## Typography (web-specific scale)

- **Display H1:** Inter 600, 48-72px desktop / 36-48px mobile.
- **H2:** Inter 600, 32-40px desktop / 24-32px mobile.
- **H3:** Inter 500, 24-28px.
- **Body:** Inter 400, 18-20px desktop / 16-18px mobile, line-height 1.6.
- **Caption / metadata:** Inter 400, 14-16px, color `#78786F`.

Larger than dashboards on purpose — marketing reads from further away, often on phone.

## Spacing (web rhythm)

- Section padding: 80-120px desktop, 48-64px mobile.
- Component padding: 24-32px.
- Card / block gaps: 32-48px.
- Hero vertical: 120-160px above fold to first H1.

## Component library (use v0.dev to generate; reuse patterns)

- **Hero** — H1 + subhead + primary CTA + social proof row (logos or single quote).
- **Comparison table** — Akira vs. n8n freelancer vs. enterprise consultancy.
- **Pricing card** — single card per skill (Voice / Email / Booking / etc.) with monthly cost, install fee range, what's included.
- **Case-study card** — client name (or anonymized), one-line outcome, key metric, CTA to read.
- **Article card** — title + 2-line excerpt + read-time + tag.
- **CTA block** — single button + 1 supporting line + optional secondary action.
- **Footer** — minimal: brand, services, blog, contact, legal.

## Page templates

### Homepage
Hero → social proof strip → 3 service tiles → case-study row → magnet (ROI calculator) → CTA.

### /services/<skill>
Hero → problem section → how it works → ROI math → case study mention → pricing → CTA.

### /case-studies/<slug>
Hero (client + outcome) → problem before → what we built → result with numbers → lesson → CTA.

### /blog/<slug>
Article (Web Writer's markdown rendered) + sidebar with related articles + magnet CTA.

### Magnet landing page
Hero (problem + offer) → form (email capture via Tally embed OR custom React form) → social proof → footer.

## Conversion rules

- **One primary CTA per page.** Above the fold.
- **Hero pattern**: H1 (value prop, specific) → subhead (who it's for) → CTA (one button) → social proof (logos / metric / quote).
- **CTA copy**: action verb + specific outcome. "Book a 20-min call" beats "Get started."
- **Social proof**: real numbers, real names (with permission) or anonymized.
- **No popups on first load.** Exit-intent only, if at all.

## Mobile

- Design at 375px width first.
- Tap targets ≥ 44×44px.
- Stacked layouts default; multi-column desktop.
- No hover-only interactions.

## Performance budget

- Hero LCP < 2.5s on 4G.
- Page total < 1.5MB.
- No autoplaying video on hero.
- Lazy-load below-fold images.
- Self-host fonts (no Google Fonts CDN that blocks rendering).

## Accessibility (table stakes)

- Contrast ≥ 4.5:1 body / 3:1 large text.
- Tap targets ≥ 44×44px.
- Visible focus states (2px accent outline).
- Alt text on every meaningful image.
- Semantic HTML (use proper heading hierarchy, button vs link, etc.).
- Skip-to-content link.

## Localization

- Swedish strings ~20% longer than English. Don't tight-fit.
- Date format: `YYYY-MM-DD` everywhere.
- Currency: `1 234 SEK` (space separator).
- Language switcher in header (when Swedish version ships).

## Patterns to avoid

- Gradient backgrounds (looks AI-generated / startup-y).
- Stock illustrations of generic businesspeople.
- Emoji-heavy headers (one accent emoji max per page; usually none).
- Pop-ups on first load.
- Playing video without user interaction.
- "Animated" hero text (typewriter effect, etc.).
- Multiple CTAs competing for the same conversion.

## Learnings (append as we go)

*(Empty seed. Populate after every major page ship.)*
