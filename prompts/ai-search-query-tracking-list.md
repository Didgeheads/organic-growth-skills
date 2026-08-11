# AI Search Query Tracking List Builder

Use this prompt to build a representative list of AI search queries to track — the prompts real buyers actually type into ChatGPT, Perplexity, Gemini, and Google AI Mode when they're researching a purchase in your category.

The core principle: people talk to LLMs conversationally, in full sentences with real context, not in short keyword fragments like they use on Google. This prompt builds the list from your real customer language (form fills, sales calls, reviews, Search Console), not from brainstormed keywords.

## How to use it

1. Paste the prompt below into Claude (or your LLM of choice).
2. Fill in your business context.
3. Pull raw exports from whichever sources apply to you — you don't need all of them, just use what you have.
4. Paste the raw text under each source. Leave sources blank if they don't apply.

---

```
You are helping me build a representative AI search query tracking list. This list will be used to monitor how my brand shows up when real buyers use LLMs like ChatGPT, Perplexity, Gemini, and Google AI Mode.

The core principle: people talk to LLMs conversationally, in full sentences with real context, not in short keyword fragments like they use on Google. So this list needs to be built from real human language, not brainstormed keywords.

### My business context

- Product or service: [DESCRIBE WHAT YOU SELL]
- Main competitors or alternatives: [LIST COMPETITORS / ALTERNATIVES]
- Priority audiences or buyer types: [E.G. SMALL BUSINESS OWNERS, ENTERPRISE IT BUYERS, AGENCIES, ETC.]
- Priority markets or languages (if relevant): [E.G. US, UK, ENGLISH, SPANISH]
- What I most need visibility into right now: [E.G. A SPECIFIC PRODUCT LINE, A NEW MARKET, A COMPETITIVE THREAT]

### My raw language sources

Below, I'll tell you which software I pulled real customer language from, then paste the raw export underneath it. Not every source will apply to me, only use what I've filled in.

**Website form fills**
Software used: [E.G. TYPEFORM, HUBSPOT FORMS, YOUR CMS]
Paste raw form responses here (especially open text fields like "what do you need help with?"):
[PASTE HERE]

**Sales call transcripts**
Software used: [E.G. GONG, CHORUS, FIREFLIES]
Paste raw transcripts or transcript excerpts here:
[PASTE HERE]

**Customer reviews**
Software used: [E.G. G2, CAPTERRA, TRUSTPILOT]
Paste raw review text here:
[PASTE HERE]

**Search Console long-tail queries**
Software used: [E.G. GOOGLE SEARCH CONSOLE]
Paste the query list here (ideally queries with low impressions/high specificity, not your top head terms):
[PASTE HERE]

**Other sources** (support tickets, community posts, live chat logs, etc.)
Software used: [NAME IT]
Paste raw data here:
[PASTE HERE]

### What I need you to do

1. Read through everything I've pasted and pull out the recurring phrases, questions, objections, and constraints people are actually using in their own words. Quote or closely paraphrase real language, don't invent generic phrasing.
2. Turn that language into full, conversational prompts that sound like something a person would actually type into an LLM, complete with context like budget, team size, use case, industry, or a tool they're already using. Avoid short keyword-style fragments.
3. Tag each prompt with:
   - Journey stage (discovery, problem-solving, evaluation, comparison, alternatives, validation, transactional, post-purchase)
   - Branded or non-branded
   - Which source(s) the language came from
   - Business priority (high, medium, low) based on the context I gave you above
4. Make sure the list isn't skewed toward one journey stage. If most of my raw language is "best X" discovery phrasing, go back through the sales and review data specifically for comparison and validation language too (e.g. "[brand] vs [competitor]," "is [brand] worth it for [use case]," "alternatives to [competitor] that also do [thing]").
5. Include a mix of branded and non-branded prompts on purpose, don't default to only one type.
6. Deduplicate and cut near-identical prompts. I want a focused list, not a bloated one.
7. Give me 30 to 50 prompts to start. Flag any priority areas from my business context above where you didn't have enough raw language to build strong prompts, so I know where to collect more.

### Output format

Give me a table with these columns: Prompt | Journey Stage | Branded/Non-branded | Source | Priority

After the table, note any gaps you found so I know what to go collect more language for next.
```

---

Built by [Jessica Redman](https://www.linkedin.com/in/jessicaredman7/) at [Didgeheads](https://didgeheads.com).
