# n8n Workflows

Free n8n workflows for organic growth, content operations, and GEO.

Built by [Jessica Redman](https://www.linkedin.com/in/jessicaredman7/) at [Didgeheads](https://didgeheads.com).

## Workflows

| Workflow | What it does |
|---|---|
| [Weekly Stale Blog Checker](weekly-stale-blog-checker.json) | Audits your WordPress library every Monday and sends a Slack DM listing posts that haven't been updated in 6+ months |

## Weekly Stale Blog Checker

### Why this matters

76.4% of ChatGPT's most-cited content was updated within the last 30 days (AirOps, 2026 State of AI Search). Pages not updated quarterly are 3x more likely to lose AI citations.

Content freshness is the single biggest predictor of AI visibility. This workflow operationalises it.

### What it does

Every Monday at 9AM the workflow:

1. Pulls every published post from your WordPress library via the REST API
2. Filters down to posts whose `modified` date is older than 6 months
3. Sorts them oldest first
4. Sends a formatted Slack DM with title, last modified date, days since update, and direct link
5. Stays silent on weeks when everything is fresh

Runtime: under 20 seconds. Cost: zero on top of your existing n8n instance.

### How to install

1. In n8n, go to Workflows → Import from File
2. Upload `weekly-stale-blog-checker.json`
3. Configure credentials:
   - **WordPress API**: site URL + application password (Users → Profile → Application Passwords in WordPress admin)
   - **Slack API**: OAuth token with `chat:write` and `im:write` scopes
4. In the Slack node, select the recipient (yourself or your content owner)
5. Activate the workflow

### How to tune the threshold

Open the "Filter Stale Posts" Code node and edit this line:

```javascript
sixMonthsAgo.setMonth(sixMonthsAgo.getMonth() - 6);
```

Change `6` to `3` for fast-moving categories (AI, fintech, SaaS) or `9` for evergreen libraries.

### CMS compatibility

The workflow uses the WordPress REST API. To adapt for other CMSes, swap the WordPress node for an HTTP Request node pointing at your CMS's posts endpoint, and adjust the field names in the Code node (the current code expects `modified`, `title.rendered`, and `link`).

## About

These workflows are part of how I work with marketing teams to build organic growth engines that reduce paid dependency and drive measurable pipeline. If you want help implementing this for your business, [get in touch](https://didgeheads.com).
