# Akira Design System

> Seed version. Evolve as surfaces ship. Every design on every surface pulls from this file. Update → don't drift.

**Last updated:** 2026-04-15

## Aesthetic direction

**Nordic hospitality, not Silicon Valley.** Clean, confident, minimal. Our clients are restaurant owners and hotel ops directors. They value calm, precision, and competence over flash.

## Typography

- **Display** — Inter or similar geometric sans. Weight 600 for H1, 500 for H2.
- **Body** — Inter 400, line-height 1.55. 16px minimum on body text.
- **Numeric** — use a tabular-numbers variant where numbers matter (pricing, metrics).
- Never more than 3 font sizes per surface.

## Color

- **Background** — warm off-white `#FAFAF7` (paper) / full black `#0A0A0A` (dark mode).
- **Foreground** — near-black `#111111` / near-white `#F5F5F5`.
- **Accent** — one accent per surface, rotating between: Nordic blue `#2B4C7E`, forest green `#3A5A40`, amber `#C7822B`. Pick based on context.
- **Neutrals** — `#E5E5E1` (light stroke), `#78786F` (secondary text), `#333333` (primary text on paper).
- No gradients. No neon. No Apple pastels.

## Spacing

- 4px base unit. Spacing scale: 4, 8, 12, 16, 24, 32, 48, 64, 96.
- Component padding usually 16 or 24.
- Section gaps 48 or 64.

## Components

- **Buttons** — single height (44px), single border-radius (6px). Primary = filled accent. Secondary = outline. Tertiary = text-only.
- **Cards** — paper background, 1px neutral stroke, 12px radius, 24px inner padding.
- **Forms** — labels above inputs. 16px field height minimum. Validation inline, not toast.
- **Tables** — zebra subtle (`#F2F2ED` alt rows). Right-align numeric columns.

## Voice-agent UX rules

- **Turn length cap** — ≤3 sentences per agent turn. Usually 1–2.
- **Recovery** — if caller says something unrecognized, agent responds with: (1) acknowledge, (2) clarify with a concrete question, (3) max 2 retries before handoff-to-human.
- **Handoff phrase** — consistent across clients: "Let me pass you to someone on our team — one moment."
- **Language switching** — if caller switches language mid-call, agent switches too (Swedish ↔ English only).
- **Silence handling** — after 4s of caller silence, agent re-prompts once; after 8s total, graceful exit.

## Accessibility

- Contrast ratio ≥ 4.5:1 for body text, 3:1 for large text.
- Tap targets ≥ 44×44 px on touch surfaces.
- Focus states always visible (2px accent outline).
- Alt text on every meaningful image.
- Forms have labels (never placeholder-as-label).

## Localization

- Swedish strings are ~20% longer than English — don't tight-fit layouts to English string length.
- Date format: `YYYY-MM-DD` everywhere (unambiguous across SE/NO/EN).
- Currency: SEK always written as `1 234 SEK` (space as thousand separator, no comma).

## Magnet visuals

- Hook image: always the output or promise of the tool, not generic stock.
- Landing page: one hero sentence, one sub-sentence, the form. No feature-list before the form.
- Result page: the personalized output first, the analysis second.

## Learnings (append as we go)

*(Empty seed. Add rows after every design-review that surfaces a new lesson.)*
