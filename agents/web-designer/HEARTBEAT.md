# HEARTBEAT — Web Designer

On event (request) + daily clearing of pending design queue.

## 0. Env

Required:
- `PAPERCLIP_*` standard
- `COMPOSIO_API_KEY`, `COMPOSIO_ENTITY_ID`
- `V0_API_KEY` (when wired — for programmatic v0.dev use; otherwise use the v0.dev web UI manually and link)
- `GOAL_ID`

Missing → comment, `blocked`, exit.

## 1. Identity + wake

```
GET /api/agents/me
```

## 2. Read state

- [`life/areas/web-design-system.md`](life/areas/web-design-system.md) — your canonical visual rules.
- [`../ux-designer/life/areas/design-system.md`](../ux-designer/life/areas/design-system.md) — sibling system; align on color + type.

## 3. Approvals + HR loop

Standard handling.

## 4. Pull queue

```
GET /api/companies/{PAPERCLIP_COMPANY_ID}/issues?assigneeAgentId={your-id}&status=todo,in_progress,blocked
```

Typical request types:
- `OG images batch — <article slugs>` from Web Writer.
- `Landing page — <campaign>` from Lead Magnet Creator or SEO + AEO Specialist.
- `Page design — <new service / pillar>` from Head of Marketing.
- `Case-study template` (one-time, then reused).

## 5. Design mode (per request)

1. **Read the brief.** Audience, conversion goal, target URL, constraints.
2. **Check the design system.** Existing template to apply or extend?
3. **Generate via v0.dev.** Produce 1–2 candidates. Iterate prompt for fit.
4. **Apply Akira voice to copy.** Brand-voice-enforcement check on all text.
5. **Run `design-review`** — accessibility (contrast, tap targets, focus, alt text), system consistency, mobile responsiveness, conversion clarity.
6. **Compose handoff** to Web Operations:
   ```
   Title: [Web Operations] Build & deploy — <page slug>
   Body (YAML):
     target_url: /<slug>
     v0_link: <v0.dev share URL>
     copy: <inline markdown of all copy on the page>
     images:
       hero: <URL or attached>
       og: <URL or attached>
       supporting: [<URLs>]
     og_metadata:
       title: <50-60 chars>
       description: <140-155 chars>
       image: <URL>
     schema: <type and properties from SEO + AEO Specialist if applicable>
     analytics_events: [<event names that should fire on this page>]
     priority: normal | high
   ```
7. **Reassign** to Web Operations with `status=todo`.

## 6. OG image batch mode (Web Writer requests)

When Web Writer ships a batch of articles, batch their OG images:

1. Pull the articles' titles + excerpts from the brief subtasks.
2. Use Vercel OG Image Generator pattern (programmatic) OR v0 for richer designs.
3. Apply the design system: paper background, Inter type, accent color rotated by article.
4. Hand off to Web Operations as a single batch subtask with image URLs.

## 7. Design system maintenance

When a reviewed design surfaces a rule worth codifying:
1. Append to `web-design-system.md` — new pattern with date, example URL, reasoning.
2. Notify the team via comment on a standing `Web Design System` issue.

## 8. Coordination with UX Designer

When a project crosses surfaces (e.g., a magnet has both a landing page AND a result UI a client uses):
- Pair via comments. Confirm boundary: prospect-facing = you, client-facing = UX Designer.
- Reuse color + type primitives from UX Designer's `design-system.md`. Don't fork the palette.

## 9. Exit

- Comment on `in_progress` work.
- `X-Paperclip-Run-Id` on mutating calls.
- Clean exit.
