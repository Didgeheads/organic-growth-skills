---
name: icp-research
version: "1.0"
author: jessica-redman
last_updated: 2026-05-12
description: >
  Produces a structured ICP (Ideal Customer Profile) document by triangulating
  three inputs: what the founder says the ICP is, what the sales team's best
  deals actually look like, and what the competitive landscape suggests.
  Outputs TAM, 2-4 firmographic segments, disqualification criteria, champion
  + economic buyer personas, voice-of-customer themes, and intent signals.
  Standalone — no shared workspace required.
---

# ICP Research

Produce an ICP document that's specific enough to build a list from. Most ICP docs read like a market-sizing pitch deck slide — "B2B SaaS companies" — which is useless to sales, marketing, or RevOps. A good ICP names a segment so tightly that a list-builder can pull it in 10 minutes and the win rate on that list beats the average.

This skill exists to produce that level of specificity.

---

## The triangulation principle

A real ICP comes from triangulating three views. If you only have one, you're guessing.

| Source | What it tells you | Bias to watch for |
|---|---|---|
| Founder / CEO view | Who they *want* to sell to | Aspiration creep — usually too broad |
| Sales team's best deals | Who they *actually* sell to | Recency bias — last quarter's wins |
| Competitive landscape | Where the white space is | Confirmation bias — easy to retro-fit |

Where the three overlap is the ICP. Where they disagree is where you ask the user to make a call.

---

## Inputs to ask for

Before starting, ask the user for whichever of these they can provide:

1. **Company URL** (required) — for context on what they sell
2. **Founder's stated ICP** in 1-2 sentences (required)
3. **Their 5-10 best customers**: company names + (ideally) industry + size (strongly recommended)
4. **Their 3-5 worst-fit deals or churned customers** (recommended — disqualification criteria come from here)
5. **Direct competitors** (optional — used to identify white space)
6. **Any sales call notes, customer interviews, or G2 reviews** (optional — fuels the VoC section)

If the user only has 1-2 of these inputs, run with what you have but flag in the output which sections are weakest and what data would strengthen them.

---

## Output structure

### 1. TAM (Total Addressable Market)
- Top-down estimate with source (industry report, public market data, etc.)
- Bottom-up estimate if possible (e.g., number of US-based companies with >50 employees in target industries × average deal size)
- Mark estimates `[VERIFIED]` (sourced) or `[ESTIMATED]` (your math)

### 2. Firmographic segments (2-4)

For each segment, name it and define it on these axes:

- **Industry / vertical** — be specific (not "tech", but "vertical SaaS for healthcare providers")
- **Company size** — employee count band + revenue band
- **Funding / maturity stage** — Series A-C, bootstrapped, PE-backed, public, etc.
- **Geography** — primary markets
- **Tech / org maturity signals** — "has a dedicated RevOps team", "uses Salesforce", "has a marketing team of 5+"

Specificity test: could a list-builder put this into Apollo or LinkedIn Sales Navigator and get a usable list? If not, sharpen.

### 3. Disqualification criteria

Explicit "not the ICP" rules. Examples:

- Under 50 employees (they can't afford us / don't have the org maturity)
- Pre-Series A (no budget)
- Healthcare or government (compliance overhead we can't service)
- Solo founders / agencies / consultants (they want done-for-you, we sell tooling)
- US-only / EU-only (depending on data residency, sales coverage, etc.)

The disqualification list is often more useful to sales than the qualification list. It tells them what to *say no to*. Build it from the worst-fit deals and churned customers the user shared.

### 4. Champion persona

The person who uses the product day-to-day and drives adoption internally.

- **Title** (and 2-3 title variants — "Head of Marketing", "Marketing Director", "VP Marketing")
- **JTBD (Jobs to be done)** — what they're trying to accomplish at work, in their own language
- **Daily pains** — 3-5 concrete pain points (specific, not "they're busy")
- **Success metrics** — how their performance is measured
- **What they read / who they follow / where they hang out** — for content distribution

### 5. Economic buyer persona

The person who signs the cheque. Usually NOT the champion.

- **Title** — typically one or two levels up from the champion
- **What they care about** — almost always: business outcomes, risk reduction, predictability of return
- **What they need to approve** — board pressure? finance committee? procurement?
- **Champion ↔ economic buyer dynamic** — does the champion need to build the business case, or does the EB already know the category?

### 6. Voice-of-Customer themes

Recurring phrases from real customers — interviews, sales calls, reviews, support tickets. **Never invent these.** If no sources exist, mark VoC `[UNAVAILABLE]` and tell the user this is the most important thing to add next.

Group themes by:
- **Trigger phrases** — what's happening when they go looking for a solution? ("Our agency stopped scaling with us", "We can't tell which channels are working")
- **Pain phrases** — what they say is broken
- **Outcome phrases** — what they say "good" looks like
- **Status-quo alternative they mention** — what they were doing before (spreadsheets, another tool, doing nothing)

### 7. Intent signals

Observable behaviour suggesting an account is in-market. Examples:

- Hiring patterns (e.g., posted a "Head of Demand Gen" role — about to invest in marketing)
- Funding events (Series B in last 90 days — budget is fresh)
- Tech stack changes (just adopted HubSpot — likely shopping for the next layer)
- Org structure changes (new CMO in last 6 months — looking for early wins)
- Content / event signals (attending Saastr — actively benchmarking)
- Negative signals (their existing competitor just had a major outage / pricing change / acquisition)

For each signal, name how the user could detect it (tool, source, manual or automated).

---

## Common pitfalls

- **"We serve everyone."** Push back. Every company has a sweet spot. Use the user's actual best customers to find it. If the user resists, ask them: "Of your last 10 deals, which 3 had the fastest sales cycle and highest contract value?" That's the ICP.
- **Too many segments.** Start with 2-3 maximum. The user can always expand later. Five segments equals no segmentation.
- **Champion vs economic buyer confusion.** The champion uses the product. The economic buyer signs the cheque. They're usually different people. If the user insists they're the same, dig — there's usually a hidden EB (CFO, CEO, board) above the stated decision-maker.
- **Aspirational ICP.** Founders often describe the ICP they *want to grow into*, not the one paying today. Both are valid — but label them separately. "Current ICP" vs "Aspirational ICP". Don't blur them.
- **Invented VoC.** If you have no customer interviews, reviews, or call notes, do not generate plausible-sounding quotes. Mark the section `[UNAVAILABLE]`. Fake VoC is worse than no VoC because it leads to fake messaging.

---

## Quality checks before delivering

- [ ] Each segment is list-buildable (industry, size, geography, signal — at minimum)
- [ ] Disqualification criteria are explicit and based on the user's worst-fit deals
- [ ] Champion and economic buyer are clearly distinct personas
- [ ] VoC themes are from real sources or marked `[UNAVAILABLE]`
- [ ] TAM has a source citation or is marked `[ESTIMATED]` with the math shown
- [ ] At least 3 intent signals named, each with a detection method

---

## Output format

```markdown
# ICP Research: [Company name]

**Date:** [MMYY]
**Sources used:** [list what the user provided]

## Executive read (3-4 sentences)
[Who they sell to, what makes that segment buy, what makes them not buy, what's the highest-leverage segment to focus on next.]

## 1. TAM
[Content]

## 2. Firmographic segments
### Segment A: [Name]
[Content]
### Segment B: [Name]
[Content]
[etc.]

## 3. Disqualification criteria
[Bulleted list]

## 4. Champion persona
[Content]

## 5. Economic buyer persona
[Content]

## 6. Voice-of-Customer themes
[Content]

## 7. Intent signals
[Content]

## Gaps and next steps
- [What's missing that would sharpen this ICP]
- [Where the three triangulation views disagreed]
- [The single highest-priority action the user should take to improve ICP confidence]
```

---

## What not to do

- **Do not write a market-sizing slide.** "B2B SaaS, $10M-$100M revenue" is not an ICP, it's a TAM filter.
- **Do not generate VoC quotes.** Better to mark `[UNAVAILABLE]` than fake voice.
- **Do not list five segments.** Two or three. Force the prioritisation.
- **Do not skip disqualification.** Sales needs the "no" list as much as the "yes" list.

---

## When to use

- Pre-positioning work (positioning needs an ICP to point at)
- Pre-content strategy (you can't write for an audience you haven't defined)
- After 12 months of GTM data, to refine the original ICP hypothesis
- When sales win rate drops and the team suspects they're chasing the wrong segments

## When NOT to use

- For a quick "who's our buyer?" answer — this is a 2-3 hour deep dive, not a slide
- For market-sizing only — TAM is one section of this, not the whole thing
- For persona-only work — personas without firmographic segments and intent signals are half an ICP
