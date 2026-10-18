# Organic Growth Skills & Workflows

Free Claude Code skills and n8n workflows for organic growth, GEO, and AI search visibility.

Built by [Jessica Redman](https://www.linkedin.com/in/jessicaredman7/) at [Didgeheads](https://didgeheads.com).

## Skills

| Skill | What it does |
|---|---|
| [GEO](skills/geo.md) | Optimise content for AI model citations (ChatGPT, Claude, Gemini, Perplexity, Google AI Overviews) |
| [GEO Content Recce](skills/geo-content-recce.md) | Score 25-40 articles against a 10-criterion weighted GEO rubric (weights anchored to published AI citation research), double-code every section opening, run significance and robustness tests, classify the writing habits hurting AI visibility, and produce a client-facing report with SVG charts, before/after rewrites and a scores CSV. Includes the full Python scoring script. This is the free version. Members get every update first, plus the monthly research brief and scoreboard: https://botworthyindex.com |
| [Content ROI Auditor](skills/content-roi-auditor.md) | Audit any blog post or article to determine if it has a commercial path to pipeline or is vanity content burning budget |
| [ICP Research](skills/icp-research.md) | Produce a specific, list-buildable ICP by triangulating founder view, sales team's best deals, and competitive landscape. Outputs segments, personas, VoC themes, and intent signals |
| [Competitor Research](skills/competitor-research.md) | Produce a single-competitor profile across 11 dimensions (positioning, pricing, GTM, content, hiring signals, threat tier) plus comparison angles vs your own company |
| [Asset Repurposer](skills/asset-repurposer.md) | Turn one source asset (talk, podcast, blog, deliverable) into a 2-3 week multi-channel release plan, with every output written through a different audience lens so you can post repeatedly without sounding repetitive |

## Prompts

| Prompt | What it does |
|---|---|
| [AI Search Query Tracking List Builder](prompts/ai-search-query-tracking-list.md) | Builds a 30-50 prompt tracking list of the conversational queries real buyers use in ChatGPT, Perplexity, Gemini, and Google AI Mode, sourced from your actual customer language instead of brainstormed keywords |

## n8n Workflows

| Workflow | What it does |
|---|---|
| [Weekly Stale Blog Checker](n8n-workflows/weekly-stale-blog-checker.json) | Audits your WordPress library every Monday and sends a Slack DM listing posts that haven't been updated in 6+ months. See [n8n-workflows/README.md](n8n-workflows/README.md) for install instructions. |

## How to use these skills in Claude Code

### Option 1: Copy into your project

Copy any `.md` file from the `skills/` folder into your project's `.claude/commands/` directory. Then use it as a slash command:

```
/geo
```

### Option 2: Reference directly

You can also paste the content of any skill into a Claude Code conversation as context.

## What is GEO?

GEO (Generative Engine Optimization) is how you get your content cited and recommended by AI models. Traditional SEO gets you ranked in search results. GEO gets you recommended when someone asks an AI for help.

Key stats:
- 58% collapse in Position 1 CTR for AI Overview keywords (Dec 2023 to Dec 2025)
- 37% of consumers now start searches with AI instead of Google
- AI referral traffic converts at 23x vs traditional organic
- 60% of AI Overview citations come from URLs that don't rank in the top 20

## What is the Content ROI Auditor?

60-70% of B2B blog content drives zero pipeline. The Content ROI Auditor scores any piece of content across 6 dimensions to tell you whether it has a commercial path or is vanity content burning budget.

The 6 dimensions:
- **Pipeline alignment** — does this content map to a buyer journey stage?
- **Commercial intent** — does it reference problems the business solves?
- **CTA strength** — is there a clear, specific next step?
- **Specificity** — concrete data and named examples, or generic filler?
- **Search intent match** — would someone searching this topic be a potential buyer?
- **Content type effectiveness** — is this format proven to drive pipeline?

Each dimension is scored (Strong / Needs Work / Missing) with quoted evidence from the content, and the skill outputs a prioritised rewrite list with specific before/after suggestions.

## Contributing

Got a skill that helps with organic growth? Open a PR.

## About

These skills are part of how I work with clients to build organic growth engines that reduce paid dependency and drive measurable pipeline. If you want help implementing this for your business, [get in touch](https://didgeheads.com).
