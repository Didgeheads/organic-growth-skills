---
name: content-roi-auditor
version: "1.0"
author: jessica-redman
last_updated: 2026-05-06
description: >
  Audits blog posts, articles, and page copy to determine whether the content
  has a commercial path to pipeline — or is vanity content burning budget.
  Scores across 6 dimensions (pipeline alignment, commercial intent, CTA
  strength, specificity, search intent match, content type effectiveness),
  provides line-level feedback, and outputs a prioritised rewrite list.
  Upstream: works standalone or after tov-guidelines / company-context.
  Downstream: feeds content-brief, geo (for AI citation optimisation),
  and editorial-calendar skills.
---

# Content ROI Auditor

Audit any piece of content to determine whether it has a commercial path to pipeline — or is vanity content that looks busy but generates nothing.

Content ROI is NOT about traffic. It's not about rankings. It's about whether this content can move a reader closer to becoming a customer. 60-70% of B2B blog content drives zero pipeline (Demand Gen Report / SiriusDecisions). This skill exists to catch that before you publish, not after.

---

## Process Flowchart

```text
┌──────────────────────────────────────────────────────────────┐
│              CONTENT ROI AUDITOR PROCESS                      │
│           (Two-Phase Workflow with User Gate)                 │
└──────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────┐
│ INPUT                                                         │
│ Required: content text, ICP, value proposition, competitors   │
│ Optional: industry, intended funnel stage, CTA destination    │
│ → If any required input is missing, PROMPT before proceeding  │
└──────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────┐
│ PHASE 1: AUDIT                                                │
│ Step 1.1: Identify content type and intended audience          │
│ Step 1.2: Score pipeline alignment (TOFU/MOFU/BOFU)           │
│ Step 1.3: Score commercial intent signals                     │
│ Step 1.4: Score CTA strength                                  │
│ Step 1.5: Score specificity vs fluff                          │
│ Step 1.6: Score search intent match                           │
│ Step 1.7: Score content type effectiveness                    │
│ Step 1.8: Build scorecard table                               │
│ Step 1.9: Write line-level feedback                           │
│ ✓ Output: Scorecard + dimension breakdowns                    │
└──────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────┐
│ REVIEW GATE: User reviews scorecard                           │
│ Present: Scores, evidence, line-level feedback                │
│ User Actions: [Approve] [Challenge scores] [Add context]      │
│ → User may add context or challenge scores before Phase 2     │
└──────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────┐
│ PHASE 2: RECOMMENDATIONS                                      │
│ Step 2.1: Incorporate user feedback                           │
│ Step 2.2: Prioritise rewrites by impact                       │
│ Step 2.3: Provide specific rewrite suggestions                │
│ Step 2.4: Deliver verdict (commercial path vs vanity)         │
│ ✓ Output: Prioritised rewrite list + verdict                  │
└──────────────────────────────────────────────────────────────┘
```

---

## Claude Code Triggers

**Invoke this skill when user says:**
- "Audit this content"
- "Is this blog post worth publishing?"
- "Does this content drive pipeline?"
- "Review this article for ROI"
- "Is this vanity content?"
- "Score this blog post"
- "Will this content generate leads?"
- "Content audit for [pasted text]"
- "Check if this content has commercial value"

**Do NOT invoke when:**
- User wants to optimise content for AI citations → Use `geo` skill
- User wants to write new content from scratch → Use content-brief or content skill
- User wants tone of voice analysis → Use `tov-guidelines` skill
- User wants SEO keyword analysis → Answer directly or use SEO-specific tool
- User wants to check grammar or readability → Answer directly

---

## Input Requirements

### Required Inputs

| Input | Description | Source |
|-------|-------------|--------|
| **Content text** | The full article, blog post, or page copy to audit | User pastes directly |
| **ICP description** | Who the ideal customer is: job title, company size, challenges, goals. The audit cannot assess pipeline alignment or search intent without knowing who the content should attract | User provides, or prompt them |
| **Value proposition** | What the business offers and the core problem it solves. The audit cannot score commercial intent without knowing what "commercial" means for this business | User provides, or prompt them |
| **Competitors** | 2-5 direct competitors. Used for the interchangeability test (could a competitor publish this?) and to assess differentiation | User provides, or prompt them |

**If any required input is missing, prompt the user before starting the audit.** Do not proceed without all four. The audit is meaningless without knowing who the content is for, what the business sells, and who it competes against.

### Optional Inputs (improve audit accuracy)

| Input | How It Helps |
|-------|--------------|
| **Industry** | Allows content type effectiveness to be scored against industry benchmarks |
| **Intended funnel stage** | Lets the audit check whether the content actually matches its intended stage vs where it naturally falls |
| **Existing CTA destination** | Where the CTA points (if live) — enables assessment of CTA-to-offer alignment |

---

## Process (Step-by-Step)

### Phase 1: Audit

**Purpose:** Score the content across 6 dimensions with evidence from the text.

**Output:** Scorecard table + dimension breakdowns with line-level feedback.

**Steps:**

1. **Step 1.1: Identify content type and intended audience**
   - Classify the content type (blog post, comparison page, case study, how-to guide, thought leadership, product page, landing page, FAQ, news commentary)
   - Identify the implied reader from the content itself (who is this written for?)
   - Compare implied reader against the provided ICP. Note any mismatch — if the content is attracting the wrong audience, that's a pipeline alignment problem
   - **Output:** Content type classification + audience identification + ICP alignment check

2. **Step 1.2: Score pipeline alignment**
   - Map the content to a funnel stage (TOFU / MOFU / BOFU) based on what it actually does, not what the author intended
   - Check: Does the content move the reader toward a commercial action, or does it dead-end?
   - Check: Is there a logical next step from this content to a deeper engagement?
   - Check: Could someone reading this become a qualified lead, or is the audience too broad?
   - Score using the Pipeline Alignment Rubric (see Core Frameworks)
   - **Output:** Funnel stage + pipeline alignment score with evidence

3. **Step 1.3: Score commercial intent signals**
   - Check: Does the content reference a problem that the business solves? (Cross-reference against the provided value proposition)
   - Check: Does it position the brand/author as someone who can help? Does it differentiate from the named competitors?
   - Check: Are there proof points (results, case studies, testimonials, data)?
   - Check: Does it create urgency or consequence for inaction?
   - Check: Is the commercial intent woven naturally, or forced/absent?
   - Score using the Commercial Intent Rubric (see Core Frameworks)
   - **Output:** Commercial intent score with evidence

4. **Step 1.4: Score CTA strength**
   - Check: Is there a CTA at all?
   - Check: Is the CTA specific ("Book a 15-min pipeline review") or generic ("Contact us", "Learn more")?
   - Check: Does the CTA match the funnel stage? (TOFU = lead magnet/subscribe, MOFU = demo/consultation, BOFU = buy/start trial)
   - Check: Is the CTA placed at the right point (after value delivery, not before)?
   - Check: Is there only one primary CTA, or is the reader confused by multiple competing asks?
   - Score using the CTA Strength Rubric (see Core Frameworks)
   - **Output:** CTA score with evidence

5. **Step 1.5: Score specificity vs fluff**
   - Check: Does the content use specific data points, named examples, and concrete frameworks?
   - Check: Are claims backed by evidence, or are they unsupported assertions?
   - Check: Could you replace the company/brand name with any of the named competitors and the content would still make sense? (If yes, it's generic — test against each competitor specifically)
   - Check: Are there filler sentences that add no information? (Count them)
   - Check: Is the content actionable — could the reader do something with it today?
   - Score using the Specificity Rubric (see Core Frameworks)
   - **Output:** Specificity score with evidence + filler sentence count

6. **Step 1.6: Score search intent match**
   - Identify the most likely search query someone would use to find this content
   - Check: Would someone searching that query be a potential buyer?
   - Check: Does the content satisfy the search intent (informational, navigational, commercial, transactional)?
   - Check: Is the content attracting the right audience, or drawing in people who will never buy?
   - Check: Would this content rank for queries that matter commercially, or only for high-volume/low-intent terms?
   - Score using the Search Intent Rubric (see Core Frameworks)
   - **Output:** Search intent score with evidence + likely query identification

7. **Step 1.7: Score content type effectiveness**
   - Cross-reference the content type (from Step 1.1) against the Content Type Effectiveness Matrix (see Core Frameworks)
   - Check: Is this content type proven to drive pipeline in this context?
   - Check: Would a different content type serve the same topic better?
   - Score using the Content Type Rubric (see Core Frameworks)
   - **Output:** Content type score with evidence + alternative format suggestion if applicable

8. **Step 1.8: Build scorecard table**
   - Compile all 6 dimension scores into the scorecard format
   - Calculate overall verdict based on scoring rules (see Scoring System)
   - **Output:** Complete scorecard table

9. **Step 1.9: Write line-level feedback**
   - Quote specific sentences or paragraphs from the content
   - Explain what's working or failing at each quoted line
   - Tie each piece of feedback to the relevant dimension
   - **Output:** Line-level feedback with quoted evidence

**Phase 1 Checkpoint:**
- [ ] Content type identified
- [ ] All 6 dimensions scored with evidence
- [ ] Scorecard table compiled
- [ ] Line-level feedback written with quoted text
- [ ] Overall verdict determined

**User Review Point:** Present Phase 1 scorecard and feedback. User may challenge scores, add context, or approve.

---

### Phase 2: Recommendations

**Purpose:** Turn the audit into an actionable rewrite plan.

**Output:** Prioritised rewrite list + verdict.

**Steps:**

1. **Step 2.1: Incorporate user feedback**
   - If user challenged any scores, re-evaluate with new context
   - If user provided additional context (funnel stage, industry), re-score affected dimensions
   - **Output:** Adjusted scores (if applicable)

2. **Step 2.2: Prioritise rewrites by impact**
   - Rank all "Needs Work" and "Missing" dimensions by impact on pipeline
   - Priority order: Commercial intent > CTA strength > Pipeline alignment > Specificity > Search intent > Content type
   - For each, estimate effort (Quick fix / Moderate rewrite / Major restructure)
   - **Output:** Prioritised rewrite list

3. **Step 2.3: Provide specific rewrite suggestions**
   - For each priority item, provide a specific rewrite or addition
   - Quote the original text, then show the improved version
   - Explain why the rewrite improves commercial ROI
   - **Output:** Before/after rewrites with rationale

4. **Step 2.4: Deliver verdict**
   - State clearly: "This content has a commercial path" or "This is vanity content"
   - If vanity: explain what would need to change to make it commercial
   - If commercial: note what's already working and what would make it stronger
   - Estimate the gap: "This content is [X changes] away from driving pipeline"
   - **Output:** Final verdict with action summary

**Phase 2 Checkpoint:**
- [ ] All "Needs Work" and "Missing" items have rewrite suggestions
- [ ] Rewrites are specific (before/after), not generic advice
- [ ] Verdict is clear and actionable
- [ ] Effort estimates provided

---

## Core Frameworks

### The 6 Audit Dimensions

| # | Dimension | What It Measures | Why It Matters |
|---|-----------|-----------------|----------------|
| 1 | **Pipeline alignment** | Does this content map to a buyer journey stage and move the reader forward? | Content that doesn't connect to a journey is a dead end — it educates but never converts |
| 2 | **Commercial intent** | Does the content reference problems the business solves and position the brand as the answer? | Content without commercial intent is a public service, not a marketing asset |
| 3 | **CTA strength** | Is there a clear, specific, stage-appropriate next step? | Pages with specific CTAs convert 2-3x vs generic "contact us" (HubSpot) |
| 4 | **Specificity** | Does the content use concrete data, named examples, and actionable frameworks? | Specific content builds trust and demonstrates expertise. Generic content is interchangeable |
| 5 | **Search intent match** | Would someone searching for this topic be a potential buyer? | High-traffic content that attracts the wrong audience is worse than low-traffic content that attracts buyers |
| 6 | **Content type effectiveness** | Is this content format proven to drive pipeline? | "Best of" guides drive 56% of AI citations. Comparison pages convert 2-4x vs blog posts (G2/Radix). Format matters |

---

### Scoring System

Each dimension is scored as one of three levels:

| Score | Criteria | What It Means |
|-------|----------|---------------|
| **Strong** | Clear evidence in the content. No changes needed for this dimension | This dimension is doing its job. The content actively contributes to pipeline through this lens |
| **Needs Work** | Partial evidence or weak execution. Improvable without major restructuring | The intent is there but the execution is falling short. Specific fixes can elevate this |
| **Missing** | No evidence. This dimension is absent from the content entirely | The content has a structural gap here. Addressing this may require significant additions or restructuring |

**Overall Verdict Rules:**

| Condition | Verdict |
|-----------|---------|
| 0-1 dimensions scored "Missing", no more than 2 "Needs Work" | **Commercial path** — this content can drive pipeline with minor improvements |
| 2-3 dimensions scored "Missing" or 3+ "Needs Work" | **At risk** — this content needs meaningful work before it will generate returns |
| 4+ dimensions scored "Missing" | **Vanity content** — this content has no commercial path in its current form |

---

### Pipeline Alignment Rubric

| Score | Criteria |
|-------|----------|
| **Strong** | Content clearly maps to TOFU, MOFU, or BOFU. There is a logical next step to deeper engagement. The reader could feasibly become a qualified lead |
| **Needs Work** | Content loosely relates to the buyer journey but doesn't move the reader forward. No clear next step. Audience is too broad or too narrow |
| **Missing** | Content has no connection to the buyer journey. It educates on a topic with no path to the business. The reader has no reason to engage further |

**Funnel stage definitions:**

| Stage | Reader mindset | Content should... | Example content types |
|-------|---------------|-------------------|----------------------|
| **TOFU** (Top of funnel) | "I have a problem" | Name the problem, build awareness, establish expertise | How-to guides, industry reports, educational blog posts, checklists |
| **MOFU** (Middle of funnel) | "I'm evaluating solutions" | Compare options, show proof, address objections | Comparison pages, case studies, webinars, frameworks, "best of" guides |
| **BOFU** (Bottom of funnel) | "I'm ready to act" | Remove friction, prove ROI, make the next step easy | Product pages, pricing pages, ROI calculators, free trials, consultations |

---

### Commercial Intent Rubric

| Score | Criteria |
|-------|----------|
| **Strong** | Content explicitly references a problem the business solves. Positions the brand/author as someone who can help. Includes proof points (data, results, testimonials). Creates natural urgency without being pushy |
| **Needs Work** | Content touches on relevant problems but doesn't connect them to the business. Proof points are vague or missing. Commercial angle feels forced or disconnected from the educational content |
| **Missing** | Content is purely informational with no connection to what the business offers. Could be published by anyone. No proof points. No positioning |

---

### CTA Strength Rubric

| Score | Criteria |
|-------|----------|
| **Strong** | Specific CTA that matches the funnel stage ("Download the 2026 GEO checklist" for TOFU, "Book a 15-minute pipeline review" for MOFU/BOFU). Placed after value delivery. One clear primary CTA |
| **Needs Work** | CTA exists but is generic ("Contact us", "Learn more"), mismatched to funnel stage (BOFU CTA on TOFU content), or competing with multiple CTAs. Placement is awkward |
| **Missing** | No CTA at all. The content ends without asking the reader to do anything. Complete dead end |

---

### Specificity Rubric

| Score | Criteria |
|-------|----------|
| **Strong** | Uses specific data points with sources. Names real companies, tools, or examples. Provides actionable frameworks the reader can use today. Claims are backed by evidence. Content is not interchangeable — it could only come from this author/brand |
| **Needs Work** | Some specific points but padded with filler. Claims without evidence. Examples are hypothetical rather than real. Some sections are actionable, others are vague |
| **Missing** | Entirely generic. No data, no named examples, no frameworks. All assertions are unsupported. You could swap the brand name and nothing would change. High filler-to-substance ratio |

---

### Search Intent Rubric

| Score | Criteria |
|-------|----------|
| **Strong** | The most likely search query for this content would be typed by a potential buyer. The content satisfies the search intent fully. Attracts the right audience with commercial potential |
| **Needs Work** | The search query has some commercial relevance but the audience is mixed. Content partially satisfies intent. May attract some buyers alongside a large non-buyer audience |
| **Missing** | The most likely search query attracts an audience that will never buy. Content targets high-volume/low-intent terms. The traffic this would generate has no commercial value |

---

### Content Type Effectiveness Matrix

| Content Type | Pipeline Effectiveness | Why | Best For |
|-------------|----------------------|-----|----------|
| **Comparison pages** | Very High | Readers are actively evaluating. 2-4x conversion vs standard blog posts (G2/Radix) | MOFU — prospects comparing solutions |
| **"Best of" guides** | Very High | 56% of all AI citations come from "best of" content (G2/Radix). High commercial intent | MOFU — prospects building shortlists |
| **Case studies** | High | Proof of results. Most effective when matched to prospect's industry | MOFU/BOFU — prospects validating |
| **How-to guides** | Medium-High | Builds expertise and trust. Effective when the "how" connects to your offering | TOFU/MOFU — prospects learning |
| **Product/service pages** | High | Direct commercial intent. Converts when paired with proof and specific CTAs | BOFU — prospects ready to act |
| **Industry reports/data** | Medium | Builds authority and earns backlinks. Commercial value depends on gating strategy | TOFU — prospects researching |
| **Thought leadership** | Medium-Low | Builds brand but rarely drives direct pipeline unless paired with a strong CTA and commercial framing | TOFU — brand building |
| **News commentary** | Low | Short shelf life, low search value, rarely connects to commercial outcomes | Brand awareness only |
| **Company announcements** | Very Low | Only relevant to existing customers/investors. Zero pipeline value for acquisition | Retention, not acquisition |

---

## Output Format

### Phase 1 Output (Scorecard + Feedback)

```markdown
# Content ROI Audit

**Content type:** [Classification from Step 1.1]
**Implied audience:** [Who this content is written for]
**Identified funnel stage:** [TOFU / MOFU / BOFU]
**Audit date:** [Date]

---

## Scorecard

| Dimension | Score | Key Evidence |
|-----------|-------|-------------|
| Pipeline alignment | [Strong/Needs Work/Missing] | [One-line evidence summary] |
| Commercial intent | [Strong/Needs Work/Missing] | [One-line evidence summary] |
| CTA strength | [Strong/Needs Work/Missing] | [One-line evidence summary] |
| Specificity | [Strong/Needs Work/Missing] | [One-line evidence summary] |
| Search intent match | [Strong/Needs Work/Missing] | [One-line evidence summary] |
| Content type effectiveness | [Strong/Needs Work/Missing] | [One-line evidence summary] |

**Overall: [Commercial path / At risk / Vanity content]**

---

## Dimension Breakdowns

### 1. Pipeline Alignment — [Score]

**Funnel stage:** [TOFU / MOFU / BOFU]
**Evidence:**
> "[Quoted text from content that supports the score]"

**Assessment:** [2-3 sentences explaining the score with reference to the rubric]

### 2. Commercial Intent — [Score]

**Problem referenced:** [Yes/No — what problem?]
**Brand positioning:** [Yes/No — how?]
**Proof points found:** [List any data, results, testimonials]

**Evidence:**
> "[Quoted text]"

**Assessment:** [2-3 sentences]

### 3. CTA Strength — [Score]

**CTA found:** [Yes/No]
**CTA text:** "[Exact CTA text]"
**Stage match:** [Does the CTA match the funnel stage?]
**Placement:** [Where in the content does it appear?]

**Assessment:** [2-3 sentences]

### 4. Specificity — [Score]

**Data points found:** [Count — list them]
**Named examples:** [Count — list them]
**Filler sentences:** [Count]
**Interchangeability test:** [Could you swap the brand name? Yes/No]

**Evidence:**
> "[Quoted filler example]"
> "[Quoted specific example]"

**Assessment:** [2-3 sentences]

### 5. Search Intent Match — [Score]

**Likely search query:** "[Query]"
**Searcher profile:** [Who would search this?]
**Buyer potential:** [Would this searcher become a customer?]

**Assessment:** [2-3 sentences]

### 6. Content Type Effectiveness — [Score]

**Content type:** [Classification]
**Pipeline effectiveness rating:** [From matrix]
**Better format available:** [Yes/No — what?]

**Assessment:** [2-3 sentences]

---

## Line-Level Feedback

| Line/Section | Feedback | Dimension |
|-------------|----------|-----------|
| "[Quoted text]" | [Specific feedback] | [Which dimension] |
| "[Quoted text]" | [Specific feedback] | [Which dimension] |
```

### Phase 2 Output (Recommendations)

```markdown
## Rewrite Priorities

### Priority 1: [Dimension] — [Effort: Quick fix / Moderate / Major]

**Current:**
> "[Quoted original text]"

**Suggested rewrite:**
> "[Improved version]"

**Why this matters:** [How this change improves pipeline potential]

### Priority 2: [Dimension] — [Effort]

[Same format]

---

## Verdict

**[Commercial path / At risk / Vanity content]**

[2-3 sentence summary: what's working, what's not, and what the content is X changes away from]

### If publishing as-is:
- [What will happen — will it generate pipeline, build brand only, or waste budget?]

### To make this content drive pipeline:
1. [Highest-impact change]
2. [Second-highest]
3. [Third]
```

---

## What Good Looks Like

### Example: Failing Audit

**Input:** A 1,200-word blog post titled "The Importance of Digital Marketing in 2026"

**Expected scorecard:**

| Dimension | Score | Key Evidence |
|-----------|-------|-------------|
| Pipeline alignment | Missing | No connection to buyer journey. Educates on a topic too broad to attract qualified leads |
| Commercial intent | Missing | No reference to what the business offers. No proof points. Could be published by anyone |
| CTA strength | Missing | Article ends with "Stay tuned for more insights." No CTA |
| Specificity | Missing | No data points, no named examples. Entire article is unsupported assertions |
| Search intent match | Needs Work | "digital marketing 2026" has volume but zero commercial intent for this business |
| Content type effectiveness | Needs Work | Thought leadership format with no commercial framing or CTA |

**Verdict: Vanity content** — This article has no commercial path. It covers a topic too broad to attract qualified leads, makes no connection to what the business offers, and ends without asking the reader to do anything. To salvage it: narrow the topic to a specific problem the business solves, add proof points from client work, and replace the sign-off with a stage-appropriate CTA.

**Why this is a good audit:**
- Every "Missing" score is backed by specific evidence (or cited absence)
- The verdict explains why and what to fix
- No generic advice — everything references the actual content

---

### Example: Passing Audit

**Input:** A 1,800-word comparison page titled "HubSpot vs Salesforce for B2B SaaS: Pipeline Attribution Compared"

**Expected scorecard:**

| Dimension | Score | Key Evidence |
|-----------|-------|-------------|
| Pipeline alignment | Strong | MOFU content targeting prospects actively evaluating CRM tools. Clear path to demo request |
| Commercial intent | Strong | References the exact problem (attribution gaps) the business solves. Includes 3 client data points |
| CTA strength | Strong | "Book a 15-minute attribution walkthrough" — specific, stage-matched, placed after the comparison table |
| Specificity | Strong | 7 named data points, feature-by-feature comparison table, real pricing figures |
| Search intent match | Strong | "HubSpot vs Salesforce attribution" — high commercial intent, searcher is evaluating tools |
| Content type effectiveness | Strong | Comparison pages convert 2-4x vs standard blog posts. Ideal format for MOFU |

**Verdict: Commercial path** — This content is built to drive pipeline. The comparison format matches MOFU intent, every section builds toward the CTA, and the proof points are specific enough to build trust. To make it stronger: add a client testimonial from someone who switched, and include a TL;DR comparison table at the top for AI citation potential (see GEO skill).

**Why this is a good audit:**
- "Strong" scores still include specific evidence, not just "looks good"
- The verdict acknowledges what works AND suggests improvements
- Cross-references the GEO skill for additional optimisation

---

## Anti-Hallucination Guardrails

1. **Every score needs evidence from the content.** Quote specific text. If you can't find evidence, the score is "Missing" — don't infer intent that isn't on the page.
2. **Don't start without required inputs.** ICP, value proposition, and competitors are required. If the user hasn't provided them, stop and ask. Don't guess or infer these from the content.
3. **Don't invent search queries.** The "likely search query" should be the most obvious query based on the title, H1, and opening paragraph. Don't create aspirational queries.
4. **Don't upgrade scores for good intentions.** A blog post that "could" drive pipeline with changes still scores based on what it does now, not what it could become.
5. **Filler count must be real.** Count actual filler sentences (sentences that add no new information). Don't estimate.
6. **Rewrites must be specific.** Every rewrite suggestion must quote original text and provide a replacement. "Add more specificity" is not a rewrite.
7. **The interchangeability test is pass/fail.** If you could replace the brand name with a competitor and the content still works, it fails. Be honest.

---

## Quality Checklist (Pre-Delivery)

### Phase 1 Quality
- [ ] Content type correctly identified
- [ ] All 6 dimensions scored
- [ ] Every score has quoted evidence from the content
- [ ] Scorecard table is complete
- [ ] Line-level feedback quotes specific text
- [ ] Likely search query identified and assessed
- [ ] Filler sentence count is a real count, not an estimate
- [ ] Overall verdict follows the scoring rules (not a gut feeling)

### Phase 2 Quality
- [ ] All "Needs Work" and "Missing" dimensions have specific rewrite suggestions
- [ ] Every rewrite quotes the original and provides a replacement
- [ ] Rewrites are prioritised by pipeline impact
- [ ] Effort estimates provided (Quick fix / Moderate / Major)
- [ ] Verdict is clear: commercial path, at risk, or vanity
- [ ] Action summary is specific and numbered

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-05-06 | Initial skill creation with two-phase audit workflow, 6 dimensions, scoring rubrics, output templates, and worked examples |

---

Created by Jessica Redman, Didgeheads (didgeheads.com)
Based on: Demand Gen Report, SiriusDecisions, HubSpot, G2/Radix, Aberdeen Group, AirOps (2026 State of AI Search)
