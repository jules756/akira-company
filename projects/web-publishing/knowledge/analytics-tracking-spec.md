# Analytics Tracking Spec — akira-agent.com

> Owned by Web Operations. The single source of truth for what events fire on which pages. Read by Growth Analyst when computing funnel metrics.
>
> **Last updated:** 2026-04-16 (seed)

## Tools

- **GA4** — pageviews + custom events. Property + measurement ID in env.
- **GTM** — tag manager for event firing logic. Container ID in env.
- **Microsoft Clarity** — session recordings + heatmaps (free).
- **Search Console** — query + indexing data.

## Event taxonomy (custom events)

Naming convention: `<category>_<action>` (snake_case).

| Event name | Fires on | Parameters |
|---|---|---|
| `page_view` | Every page (default GA4) | path, title, referrer |
| `cta_click` | Any primary CTA button click | cta_label, cta_destination, page_path |
| `magnet_view` | Magnet landing page loads | magnet_slug, page_path |
| `magnet_form_start` | First field interaction in a magnet form | magnet_slug |
| `magnet_form_submit_success` | Tally form submission success | magnet_slug, email_captured |
| `magnet_result_viewed` | User reaches the personalized result page after submit | magnet_slug |
| `blog_article_view` | Blog article page loads | article_slug, target_query (from frontmatter), category |
| `blog_internal_click` | Click on an internal link from a blog article | source_article, destination_article |
| `blog_external_click` | Click on an external link from a blog article | source_article, destination_url |
| `case_study_view` | Case study page loads | case_study_slug, client_anonymized |
| `nav_click` | Top-nav or footer click | nav_item, destination |
| `contact_form_view` | Contact form visible | page_path |
| `contact_form_submit` | Contact form submission | form_id |
| `cal_booking_click` | Click on Cal.com booking link | source_page, cta_label |
| `scroll_depth` | 25%, 50%, 75%, 100% scroll markers | depth, page_path |

## Conversion events (marked as GA4 conversions)

- `magnet_form_submit_success`
- `contact_form_submit`
- `cal_booking_click`

These three feed Growth Analyst's Friday Board Brief funnel section.

## Attribution model

GA4 default: data-driven attribution. Growth Analyst can override with first-touch / last-touch as needed.

UTM parameters expected on inbound links from external sources:
- `utm_source` — `linkedin`, `email`, `tally`, `direct`, etc.
- `utm_medium` — `social`, `email`, `referral`, `organic`, etc.
- `utm_campaign` — campaign slug.
- `utm_content` — variation identifier (for A/B-style content tests).

## Per-page tagging requirements

| Page type | Required events |
|---|---|
| Homepage | `page_view`, `cta_click`, `nav_click`, `scroll_depth` |
| /services/<skill> | `page_view`, `cta_click`, `nav_click`, `scroll_depth`, `cal_booking_click` |
| /case-studies/<slug> | `page_view`, `case_study_view`, `cta_click`, `scroll_depth` |
| /blog/<slug> | `page_view`, `blog_article_view`, `blog_internal_click`, `blog_external_click`, `scroll_depth` |
| Magnet landing pages | `page_view`, `magnet_view`, `magnet_form_start`, `magnet_form_submit_success`, `magnet_result_viewed` |
| Contact / about | `page_view`, `contact_form_view`, `contact_form_submit`, `cal_booking_click` |

## Microsoft Clarity setup

- Site-wide tag in `<head>`.
- All pages recorded by default.
- Privacy: PII masking enabled (input fields default-masked).

## Audit cadence

Web Operations samples 5 random pages weekly (Friday) and verifies all expected events fire correctly. Mismatch = file fix-task same heartbeat.

## Open items

- [ ] First-time setup: install GA4 + GTM + Clarity tags in Next.js layout.
- [ ] Configure GA4 conversions (mark the three events above).
- [ ] Verify Search Console property + submit sitemap.
- [ ] Bind GA4 + Search Console to Composio for programmatic queries.

## Changelog

*(Empty seed. Append every event added / removed / renamed.)*
