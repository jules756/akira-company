# Deerflow Prompt Templates

One template per request type. Use these to reframe incoming requests from team agents into Deerflow-shaped queries. Append new templates as new request patterns appear — three+ similar requests = new template.

## Template shape

Every reframed query has four fields:

- **Intent** — what the requester will do with the result (a decision, a draft, a data point).
- **Scope** — industry + region + timeframe + source types.
- **Output schema** — `bullets | table | long-form | citation-dump`.
- **Depth** — `quick | standard | deep`.

Send the fields to Deerflow as part of the query body; Deerflow's prompt engine honours each.

---

## Template — Lead Researcher: company deep-profile

**When to use:** Lead Researcher has a Tier-1 prospect and needs a full intelligence dossier before Head of Sales crafts outreach.

**Reframe:**

```
Intent: Build an outreach-ready profile for {{Company Name}} that Head of Sales can use to craft a personalised cold message.
Scope:
  - Company: {{Company Name}} ({{Location}}, {{restaurant type}})
  - Region: {{SE | NO}}
  - Timeframe: last 18 months
  - Source types: company website, trade press, local news, LinkedIn, Google reviews, booking-platform listings
Output schema: long-form, with these sections — Company snapshot / Decision-maker profile / Recent signals / Operational pain hypotheses / Outreach hooks / Competitor landscape
Depth: standard (8–12 min expected)
```

---

## Template — SEO + AEO Specialist: algorithm-change impact brief

**When to use:** Major Google / LLM-platform update. Specialist needs a digest + Akira-specific action items.

**Reframe:**

```
Intent: Brief the SEO + AEO Specialist on the practical impact of {{update name}} on hospitality-vertical content sites, with concrete action items for akira-agent.com.
Scope:
  - Update: {{update name + date}}
  - Industry: hospitality / restaurants / hotels
  - Region: global (for impact) + SE/NO (for specifics)
  - Timeframe: from the update's announcement through latest observed SERP data
  - Source types: Google Search Central, SEO industry blogs (Search Engine Land, SEJ), LLM-platform changelogs, credible case studies
Output schema: structured report — What changed / Who's affected / Observed impact / Contradicting signals / 5 action items for akira-agent.com, ranked by effort × impact
Depth: deep (20–30 min expected)
```

---

## Template — SEO Trend Researcher: trend deep-dive

**When to use:** Daily scan surfaced something worth a full report (beyond the weekly synthesis).

**Reframe:**

```
Intent: Produce a decision-quality brief on {{trend name}} so Head of Marketing + SEO + AEO Specialist can decide whether to build content / pages / tools around it.
Scope:
  - Trend: {{trend name}}
  - Region: SE + NO primary, Nordics secondary
  - Timeframe: last 6 months + leading indicators for next 6 months
  - Source types: industry reports, search-volume tools, social signal tracking, trade press
Output schema: executive brief — TL;DR / Signal strength / Who's riding the trend / Entry angles for Akira / 90-day opportunity window / Risks
Depth: standard (8–12 min expected)
```

---

## Template — Lead Magnet Creator: ideation report

**When to use:** Creator needs fresh magnet ideas for the next sprint.

**Reframe:**

```
Intent: Generate a ranked shortlist of lead-magnet concepts Akira can ship in the next 30 days for {{segment}}, each with a hook, a format, and a source-of-inspiration.
Scope:
  - Audience: {{Nordic restaurateurs | group operators | hotels}} — specific segment
  - Region: SE + NO
  - Timeframe: what they're searching / struggling with RIGHT NOW (last 90 days)
  - Source types: Reddit + industry forums + competitor lead magnets + Google Trends + TikTok hospitality creators
Output schema: ranked table — Concept / Hook / Format (PDF | Tool | Calculator | Newsletter series) / Build effort (S/M/L) / Signal strength / Inspiration source
Depth: standard
```

---

## Template — Content Writer: background for long-form

**When to use:** Writer is drafting a pillar piece and needs factual + context research.

**Reframe:**

```
Intent: Provide the Content Writer with citation-ready facts + statistics + expert quotes + counter-arguments for the article "{{article title}}".
Scope:
  - Topic: {{article topic}}
  - Region: {{relevant to the piece}}
  - Timeframe: {{relevant to the piece}}
  - Source types: academic + trade press + industry surveys + expert interviews (published)
Output schema: citation dump — one fact per bullet, each with a URL, a date, and a one-line relevance note
Depth: standard
```

---

## Template — Head of Sales: live-deal intelligence

**When to use:** A specific deal needs sharper context before a Jules call or proposal.

**Reframe:**

```
Intent: Arm Jules for the {{stage}} conversation with {{Company Name}} with intelligence that changes the conversation — competitive pressure, recent news, known pain.
Scope:
  - Company + any parent group
  - Region: their operating markets
  - Timeframe: last 6 months (news) + current state (operations)
  - Source types: news + review platforms + LinkedIn activity of decision-maker + booking-platform listings + local press
Output schema: call brief — Decision-maker recent activity / Company news / Operational signals / Competitive pressure / 3 questions Jules should ask / 2 landmines to avoid
Depth: quick to standard depending on urgency
```

---

## Template — Head of Marketing: campaign / market research

**When to use:** Planning a campaign or making strategic marketing decisions.

**Reframe:**

```
Intent: Support Head of Marketing's decision on {{campaign concept | channel bet | positioning shift}} with market evidence.
Scope:
  - Market: {{segment + region + timeframe}}
  - Source types: industry reports + competitor analysis + trade press + adjacent-market learnings
Output schema: strategic brief — Market state / Competitor moves / Customer signals / Akira's positioning options / Recommendation + rationale
Depth: deep
```

---

## Template — CEO / Jules: strategic research

**When to use:** High-stakes decision needing external input.

**Reframe:**

```
Intent: Inform a strategic decision Jules is about to make on {{topic}}.
Scope:
  - Topic: {{specific}}
  - Source types: named analysts, industry primary research, strategic frameworks from published thinkers
Output schema: decision memo — Question being decided / Three options / Evidence for each / Recommendation with caveats
Depth: deep
```

---

## Depth-class runtime budgets

- **Quick** (≤5 min Deerflow runtime) — single specific question, 3–5 sources, bullets or short paragraph.
- **Standard** (5–15 min) — scoped topic, 10–20 sources, structured report.
- **Deep** (15–45 min) — wide topic, 30+ sources, long-form with multiple angles.

If a reframe seems to need longer than 45 min, it's too broad. Split into two Deep jobs.

---

## Change log

- **2026-04-18** — Initial templates for Lead Researcher, SEO + AEO Specialist, SEO Trend Researcher, Lead Magnet Creator, Content Writer, Head of Sales, Head of Marketing, CEO.
