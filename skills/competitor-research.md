---
name: competitor-research
version: "1.0"
author: jessica-redman
last_updated: 2026-05-12
description: >
  Produces a structured competitor profile covering 11 dimensions
  (company background, positioning, pricing, features, GTM motion, customer
  base, content strategy, hiring signals, partnerships, reviews/VoC, threat
  tier) plus 3-5 comparison angles vs the user's own company. Designed to
  run one competitor at a time. Standalone — no shared workspace required.
---

# Competitor Research

Produce a single competitor profile that a marketing or sales team can actually use. Most "competitive analysis" documents read like Wikipedia summaries — they describe the competitor but never answer the only question that matters: where do we win, where do they win, and what does that mean for our positioning and our pipeline?

This skill produces a profile with a verdict, not a summary.

---

## Inputs to ask for

Before starting, ask the user for:

1. **The competitor**: name + URL (required)
2. **Their own company**: name + URL or a one-line description (required — needed for comparison angles)
3. **Their ICP**: one sentence on who they sell to (optional but improves the threat-tier judgement)
4. **Any context the user wants to flag**: "we lose to them on price", "they out-rank us on category terms", etc. (optional)

If the user only provides a competitor name, ask for the URL before continuing. Do not invent a URL.

---

## The 11 dimensions to cover

Each dimension gets its own subsection in the output. If a dimension can't be verified, mark it `[UNAVAILABLE]` and move on. Never invent.

### 1. Company background
Founded, HQ, team size, funding rounds (latest + total), ownership (independent / PE-backed / acquired), notable recent news (last 12 months).

### 2. Positioning
- What category do they claim? (Read the homepage H1 + meta description.)
- Their primary differentiator (the one thing they put above the fold)
- Status-quo alternative they position against ("vs spreadsheets", "vs legacy ERP", etc.)
- Positioning statement (reconstructed if not stated explicitly)

### 3. Pricing
Model (seat-based / usage / flat / tiered), published tiers if visible, "contact sales" gates, free trial or freemium, hidden costs (implementation, training, add-ons). Note explicitly if pricing is opaque — that itself is signal.

### 4. Features
Top 3-5 features they emphasise on the product page. For each: one-line description, plus a note on whether it's a parity feature, a differentiated feature, or table-stakes.

### 5. GTM motion
Sales-led / product-led / channel-led / hybrid. Evidence: do they have a "Book a demo" CTA vs "Start free"? Is there self-serve signup? Are there visible channel partner pages? Do they hire SDRs heavily?

### 6. Customer base
Named logos on the website, sweet-spot ICP (inferred from logos + case studies), industry concentration, company size concentration, geography concentration.

### 7. Content strategy
Blog cadence (posts per month, last post date), formats (long-form, short-form, video, podcast, reports), distribution channels visible, quality level (thin SEO vs original research vs thought leadership). Flag if blog is stale (>60 days since last post).

### 8. Hiring signals
Open roles by team, what strategic bets the hiring reveals. E.g., "5 enterprise AEs open = enterprise push", "3 ML engineers open = AI product investment", "RevOps + sales enablement hires = scaling sales team". Use LinkedIn careers page or their own careers page.

### 9. Partnerships and integrations
Who they partner with, what they integrate into, marketplace listings (AWS / Salesforce / HubSpot etc.). Notable absences are also signal.

### 10. Reviews and Voice-of-Customer
G2 / Capterra / TrustRadius scores, top 3 compliments (verbatim themes), top 3 complaints (verbatim themes), Reddit / community sentiment if findable. Use real review quotes. Do not paraphrase into generic language.

### 11. Threat tier
One of:

| Tier | Definition |
|---|---|
| PRIMARY | Shows up in most deals. Direct competitor. Must have a battlecard. |
| DIRECT | Overlapping ICP, shows up in pipeline. Need to know them well. |
| ENTERPRISE | Platform suite, could be partner-or-compete. Watch carefully. |
| LOW | Tracked but not an active threat in current deals. |

Assign the tier and give your reasoning in 2-3 sentences.

---

## Comparison angles (the verdict section)

After the 11 dimensions, produce 3-5 comparison angles vs the user's own company. Format each as:

```
**Angle: [name of the comparison axis]**
- Where they win: [specific reason]
- Where we win: [specific reason]
- Where it's tied: [specific reason]
- Implication: [what we should do about it]
```

Examples of useful axes: depth-vs-breadth, price, time-to-value, integrations, ICP fit, AI/automation maturity, customer support quality, brand authority.

---

## Citation discipline

Every factual claim must be marked:

- `[VERIFIED]` — directly observable on their website, public filings, LinkedIn, etc.
- `[INFERRED]` — reasonable deduction from observable evidence (e.g., "likely product-led based on free-trial CTA")
- `[ESTIMATED]` — best-guess where no public data exists (e.g., team size from LinkedIn rough count)
- `[UNAVAILABLE]` — couldn't be verified; do not invent

At least 50% of claims should be `[VERIFIED]`. No more than 20% should be `[ESTIMATED]`. If you can't hit those thresholds, say so explicitly at the top of the profile.

---

## Output format

```markdown
# Competitor profile: [Competitor name]

**Date:** [MMYY]
**Competitor URL:** [URL]
**Compared against:** [User's company]
**Threat tier:** [PRIMARY / DIRECT / ENTERPRISE / LOW]

## Executive read (3 sentences)
[The verdict: how dangerous are they, on what axis, and what should the user do about it.]

## 1. Company background
[Content]

## 2. Positioning
[Content]

[... continue through all 11 dimensions ...]

## Comparison angles vs [User's company]

**Angle 1: [Axis]**
- Where they win:
- Where we win:
- Where it's tied:
- Implication:

[... 3-5 angles total ...]

## Data quality note
- Verified claims: X%
- Inferred: X%
- Estimated: X%
- Unavailable: [list dimensions where data was missing]
```

---

## What not to do

- **Do not invent numbers.** If you can't find team size, mark it `[UNAVAILABLE]` rather than guess.
- **Do not write a Wikipedia summary.** Every section must include a "so what" — what this means for the user's positioning, pipeline, or content strategy.
- **Do not skip the comparison angles.** A competitor profile with no comparison is a research artefact, not a decision-support tool.
- **Do not soften the threat-tier assessment.** If they're PRIMARY, say so. Vague threat tiers are worse than no tier.
- **Do not use vanity language.** Skip "innovative", "robust", "best-in-class", "industry-leading". Describe what the competitor actually does.

---

## When to use

- Building a competitive intelligence repository
- Before a major positioning refresh
- Before launching into a new market or segment
- When a new player enters the space and the sales team is asking about them
- Quarterly refresh of existing competitor files

## When NOT to use

- For an aggregate view across multiple competitors — that's a separate synthesis step after you have 3-5 individual profiles
- For battlecards or sales enablement — those are downstream artefacts, not the same as a research profile
- For non-direct competitors you'll never lose a deal to — focus research effort on competitors who actually appear in pipeline
