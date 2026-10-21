---
name: "geo-content-recce"
description: "Run a GEO content recce for a client: score a set of web articles against a weighted GEO rubric, find the writing patterns hurting AI visibility, and write it up with charts and before/after rewrites."
---

# GEO content recce

This is the free version. Members get every update first, plus the monthly research brief and scoreboard: https://botworthyindex.com

Created by Jessica Redman (jmredman.com), creator of the Botworthy Score. The Botworthy Score is a 0 to 100 score for how likely AI models are to quote a page, across 10 criteria weighted by published research into what AI models actually cite. This skill is the free version of the Botworthy Score; the definition and criteria are at https://botworthyindex.com/botworthy-score

A repeatable, evidence-based study of how well a client's content is written for AI search (GEO/AEO). It scores 25-40 articles against a 10-criterion weighted rubric whose weights are anchored to published AI citation research, double-codes every section opening, runs significance and robustness tests, classifies the writing habits behind weak sections, and produces a client-facing report with charts and before/after rewrites.

Built and tested on a FTSE 100 company's knowledge centre. The rubric is on v2 weights (October 2026); see "The rubric and the evidence behind it" in Step 6.

## When to use
- "Run the GEO recce on [client / site section]"
- "Score this content against the GEO rubric"
- "What writing patterns do their writers need to change?"
- Re-scoring the same pages after rewrites or training, to measure change.

## What it produces
1. A client-facing report (a Notion page, doc or wherever you keep reports) with: in-brief callout, method, rubric table, results, consistent patterns table, before/after table, limitations, next steps.
2. Five SVG charts: points lost by criterion, article x criterion heatmap, question vs statement headings, opening habits, score by content type.
3. A CSV of every article's scores.
4. The working folder (raw text, coded data, scripts) so the study can be rerun.

## Before starting, confirm
- **Client and scope:** which site section or URL list. Aim for 25-40 articles of one family (knowledge centre, blog, guides). Fewer than 20 weakens the statistics.
- **Where it goes:** ask where the report should live (Notion, a doc, etc.). Check before publishing anything to an external system.
- **Client-facing:** the report must not mention internal sources, decks, tools used to build it, or the consultant's own talks. State findings and method only.
- **Compliance-sensitive sectors** (financial services, health, legal): flag any rewrite that adds a generalisation ("usually", "most") for the client's compliance review.

Set up a task list with these steps and tick them off.

## Step 1. Build the corpus
- Collect article URLs from the section's listing pages. Listing pagination is often JavaScript-driven (`?page=2` returns page 1). If so, use WebSearch with `allowed_domains` and several topical queries to find the rest, and record how many of the total you reached.
- Save to `urls.txt`. Work in a fresh folder, e.g. `recce-<client>/`, and copy in `geo_recce.py` from the appendix.

## Step 2. Fetch each article verbatim
Split the URLs across 4 parallel general-purpose subagents (about 8 each). Give each this instruction:

> For each URL, use WebFetch with this exact prompt: "Return the complete main article body as Markdown, word for word, without summarising, shortening or paraphrasing. Preserve every heading with its level (#, ##, ###), every paragraph, every bulleted or numbered list item, and every table (as a Markdown table). Exclude site navigation, cookie banners, footer, related-article cards and quote/CTA widgets. On the first line write 'TITLE: ' and the H1. On the second line write 'UPDATED: ' and any published/updated/reviewed date shown on the page, or 'none'. On the third line write 'AUTHOR: ' and any author or reviewer shown, or 'none'. Then a blank line, then the article body." Write each result exactly as returned to `<folder>/raw/<slug>.md` (slug = last path segment). If a fetch fails or looks summarised (under 150 words, or phrases like 'the article discusses'), retry once; if it still fails, write `FETCH_FAILED` and the error on line 1. Reply with slug, word count and OK/FAILED per URL.

If WebFetch asks for permission on the client domain and times out, ask the user to paste the domain in chat, then retry.

## Step 3. Assign content types
Read the titles and openings and group the articles into 3-5 content types by the job they do (e.g. coverage question, product explainer, policy process, how-to advice). Write `types.json` as `{"<slug>": "<type>"}`.

## Step 4. Computed metrics
`python3 geo_recce.py metrics` writes `metrics.json` and `openers.json` (every H2/H3 with its first two sentences). Check the opener count looks right (roughly 6-12 per article). Sections that open with a table or a list aren't coded, because there's no opening sentence to judge; the script counts them in `openers_skipped.json`. Report that count in the method section.

## Step 5. Code every section opening, twice
**Coder A (all openers).** One general-purpose subagent:

> Read `<folder>/openers.json`: sections from web articles, each with an id, heading, and the first and second sentence after it. Code every item independently using only the text given. Be strict and consistent.
> - "opener": "A" Direct answer: the FIRST sentence on its own answers the heading's question, or states the section's key point if the heading is a statement. For a yes/no question it must give the answer AND the deciding fact or condition in that same sentence. A bare "Yes." with the reason in sentence 2 is B. "B" Deferral: answers only with a non-specific hedge ("It depends", "That's up to you", "Sometimes, but not always", a bare "Yes"). "C" Lead-in: sets the scene, gives background, restates or asks the question, talks about something else, or only introduces a list that follows (ends with a colon without naming the points).
> - "standalone": "Y" if the first sentence, lifted out without the heading, makes clear what it is about; "N" if it relies on the heading or earlier text (starts It/This/They/Again/These, or never names its subject).
> - "heading_form": "Q" if the heading ends with ?, else "S".
> - "note": max 12 words, only for B or C, saying where the answer actually is (e.g. "answer in sentence 2", "items listed after sentence 2", "condition never stated").
> Write a JSON array of {id, opener, standalone, heading_form, note} for every id to `<folder>/coded_a.json`. Reply with the counts.

**Coder B (random sample).** Run `python3 geo_recce.py sample 40`, then a separate subagent with the same definitions for opener and standalone only, reading `sample_b.json` and writing `coded_b.json`. Tell it not to open any other file in the folder.

**Agreement.** `python3 geo_recce.py kappa`. Target Cohen's kappa of 0.7 or above on both fields. If lower, read the disagreements, tighten the rubric wording, and recode.

## Step 6. Score and find patterns
`python3 geo_recce.py score` writes `scores.json`, `patterns.json`, `coded_patterns.json` and `geo-scores.csv`, and prints the headline numbers. It computes:
- GEO score per article (0-100, 10 weighted criteria) and corpus mean with a 10,000-sample bootstrap 95% CI.
- Per criterion: average attainment, articles scoring below half, average points lost; share of lost points from the top three criteria.
- Robustness: Spearman correlation of rankings under equal weights, dropping each criterion, and 1,000 random weight changes of up to 50%.
- Question vs statement headings: direct-answer rates with Fisher's exact test and odds ratio.
- Opening-habit taxonomy for every indirect section (deferral, list set-up, question restated, advice preamble, promotional teaser, reason before action, scene-setting, adjacent fact), with counts, articles affected and examples.
- Where the answer sits (sentence 1, sentence 2, later, not in the first two).
- Recurring phrasing (hedges, "check your policy", "it depends", "it's important").
- Differences by content type (Kruskal-Wallis) and criteria that move together (Spearman, |rho| >= 0.4, p < 0.05).

Spot-check the taxonomy: print a few examples per habit from `coded_patterns.json` and make sure the biggest buckets aren't mis-sorted. Adjust the regex rules in `TAXONOMY` if the client's sector has its own stock phrases, and note any change in the method section.

The rubric weights and thresholds are in `RUBRIC`. Keep them unchanged across clients so scores are comparable. If you must change them for a client, say so in the report.

### The rubric and the evidence behind it (v2, October 2026)
v1 weights were practitioner judgement. v2 anchors each weight to published citation research: criteria with strong evidence gained weight, criteria without direct evidence lost some.

| Criterion | v1 | v2 | Evidence behind the weight |
|---|---|---|---|
| R1 Answer-first sections | 20 | 20 | Zyppy meta-analysis: answer near the top 8.8/10, query-answer match 9.2/10 |
| R2 Question-led headings | 10 | 6 | No direct study; inferred from query-answer match |
| R3 Summary at the top | 7 | 7 | Zyppy: answer near the top 8.8/10 |
| R4 Self-contained passages | 8 | 12 | Zyppy: self-contained passages 8.0/10 |
| R5 Short sentences | 10 | 7 | Not among the Zyppy factors; kept for extractability |
| R6 Lists and tables | 12 | 14 | Zyppy: AI-ready structure (headings and tables) 8.6/10 |
| R7 Statistics and specifics | 10 | 10 | Princeton GEO paper (Aggarwal et al., KDD 2024): statistics lifted visibility by up to 40% |
| R8 Named sources | 7 | 10 | Zyppy: cites sources 8.0/10; Princeton: citing sources up to 30% |
| R9 Named expertise | 6 | 6 | Princeton: quotations up to 37% |
| R10 Recently updated | 10 | 8 | Zyppy: freshness 7.0/10; Ahrefs: AI-cited content 25.7% fresher than organic, but AI Overviews cite slightly older pages |

Sources to cite in the report:
- Zyppy AI Citation Ranking Factors (Cyrus Shepard, May 2026), summarised at https://www.digitalapplied.com/blog/ai-search-citation-ranking-factors-2026-data-study
- Ahrefs, AI assistants prefer to cite fresher content (16.975M citations): https://ahrefs.com/blog/do-ai-assistants-prefer-to-cite-fresh-content/
- Yext, 86% of AI citations come from brand-managed sources (6.8M citations): https://investors.yext.com/news-events/press-releases/detail/376/yext-research-86-of-ai-citations-come-from-brand-managed (use to justify why on-page content matters)
- GEO: Generative Engine Optimization, Aggarwal et al., KDD 2024

Before each new client run, check whether these studies have been updated. If the evidence has moved enough to justify new weights, bump the version, rescore the previous studies from their CSVs so they stay comparable, and note the change in the report.

To rescore a past study after a weight change, the CSV alone is enough: it holds every criterion's 0-1 score, so the new score is the weighted sum. Check first that the old weights reproduce the published scores (within about 0.1, since criteria are stored to two decimals).

## Step 7. Charts
`python3 geo_recce.py charts` writes five self-contained SVGs to `charts/` (each under 30 KB, vector, no external fonts). To check them, screenshot each in Playwright (wrap the SVG in an HTML body and screenshot the `svg` element) and look for clipped or overlapping labels before publishing.

## Step 8. Before and after examples
- Pick 6-8 sections that show the main habits: deferrals, buried answers, scene-setting, list set-up, and one prose section that should be a table.
- Print each with `python3 geo_recce.py section <slug> "<heading text>"` and rewrite it answer-first using **only facts already on that page**. Never add a figure, claim or condition the page doesn't contain.
- Verify with a fresh subagent that hasn't seen your reasoning: give it each before, after and source section, and ask it to flag any fact in the after that isn't in the source. Fix anything flagged.

## Step 9. Write and publish the report
Structure:
1. **In brief** callout: corpus size, mean score with CI, articles below 50, share of indirect sections and articles affected, question vs statement direct-answer rates with significance, top-three share of lost points, whether content type matters.
2. **How the study works:** corpus and capture date, two layers of measurement, reliability (agreement and kappa), scoring method, statistical tests, robustness results, pattern classification.
3. **The GEO rubric:** table of dimension, criterion, weight, how it's scored, and the evidence behind each weight, with the sources listed under it.
4. **Results:** points-lost chart plus criterion table; heatmap with highest and lowest scorers; score-by-type chart with interpretation.
5. **Consistent patterns writers need to change:** heading chart with odds ratio and correlation; answer-position table; opening-habits chart; patterns table with pattern, how often, a real example from the site, what to do instead. Include sentence length, hedging, evidence, tables and freshness rows.
6. **Before and after examples:** table with page and pattern, before, after. Note any rewrites needing compliance review.
7. **Limitations:** coverage, capture method, duplicated sections, rule-based taxonomy not double-coded, weights anchored to evidence but still a judgement, structure is not the same as actual AI citations, and the rubric only scores on-page factors (crawlability, classic rankings and brand mentions, the strongest factors in the research, sit outside it).
8. **Next steps:** run on another section for comparison, rescore after rewrites or training, link scores to AI visibility data.
9. The CSV attached at the end.

Writing rules: plain English, sentence case, numbers stated precisely ("38 of 100", "p < 0.0001"), no internal references, no claims beyond what the data shows.

**If publishing to Notion:** direct file uploads from the workspace are usually blocked by the network allowlist. Instead upload each chart with `notion-create-attachment` passing the SVG text as `content` and a `.svg` filename, then place it with the returned `<image src="file-upload://..."></image>`. Upload the CSV the same way and place it with `<file src="..."></file>`. Create the report as a child page of the client's working page, then add a short callout linking to it from the parent. Fetch the page afterwards to confirm every image and the file rendered.

## Step 10. Check
- Every number in the report matches `patterns.json`.
- Every before/after passed the independent check.
- Charts render without clipping.
- Nothing internal or tool-related appears in the client-facing text.

## Gotchas
- Very short first sentences ("Yes.", "It depends.") are kept as openers on purpose, because they are the deferrals R1 is built to catch. The sentence-length metric (R5) still ignores fragments under three words.
- WebFetch passes pages through a small model; occasionally text is lightly altered. Spot-check one article against the live page.
- Strip " - Brand" suffixes from page titles before charting, and disambiguate any duplicate titles.
- Never run regex rounding over SVG text content; only round coordinate attributes (the script's SVG builder avoids this).
- Articles with no visible date score 0 on freshness. Report how many had no date.
- Two articles sharing identical sections are each scored as published; note it in limitations.
- Notion's find-and-replace can't match image or file blocks. To swap a chart on an existing report, fetch the page and rewrite it whole with `replace_content`, keeping every unchanged `notion-file-block://` reference exactly as fetched and using the new `file-upload://` sources for the replacements. Fetch again to confirm.
- Notion's signed file links can't be downloaded from the workspace. To get a past study's CSV back, open the report page in the browser and read the file with a `fetch()` from that tab.

## Changelog
- October 2026 (script fixes, scores comparable with v2 weights): short openers such as "Yes." were being dropped so sentence 2 was coded instead; dates written with "September" failed to parse and scored 0 for freshness; articles updated in the last few days were counted as over 12 months old in the headline counts; sections opening with a table had the footnote under the table coded as the opener. Thanks to an external tester who ran the skill on 32 German guides. Rescore any study run before this date from its raw files, since the first two fixes can change opener codes and freshness scores.

## Appendix: geo_recce.py
Write this to the working folder verbatim before Step 4. It needs Python 3 with numpy and scipy.

```python
#!/usr/bin/env python3
"""GEO content recce: score a set of web articles against a GEO rubric and find writing patterns.

Usage (run from the project folder that contains raw/):
  python3 geo_recce.py metrics            # computed metrics + extracts section openers -> metrics.json, openers.json
  python3 geo_recce.py sample 40          # random sample of openers for the second coder -> sample_b.json
  python3 geo_recce.py kappa              # agreement between coded_a.json and coded_b.json
  python3 geo_recce.py score              # rubric scores, statistics, patterns -> scores.json, patterns.json, geo-scores.csv
  python3 geo_recce.py charts             # compact SVG charts -> charts/*.svg
  python3 geo_recce.py section <slug> "<heading text>"   # print one section, for before/after drafting

Inputs:
  raw/<slug>.md   first three lines: 'TITLE: ...', 'UPDATED: <date or none>', 'AUTHOR: <name or none>', then the body as Markdown
  types.json      {"<slug>": "<content type>", ...}   (3-5 content types, assigned by reading the articles)
  coded_a.json    [{id, opener: A|B|C, standalone: Y|N, heading_form: Q|S, note}]  (all openers, coder A)
  coded_b.json    [{id, opener, standalone}]  (random sample, coder B)
"""
import sys, os, re, json, glob, csv, random, collections, statistics as st
from datetime import date, datetime

TODAY = date.today()
RAW = "raw"

# ---------------------------------------------------------------- parsing
MDLINK = re.compile(r"\[([^\]]*)\]\([^)]*\)")
SKIP_HEADINGS = re.compile(r"in this article|key points|video transcript|transcript|table of contents", re.I)
STAT = re.compile(r"(\d[\d,.]*\s?%|[£$€]\s?\d[\d,.]*(k|m|bn)?|\b\d+\s?(in|out of)\s?\d+\b|\b\d[\d,.]*\s?(million|billion|thousand)\b)", re.I)
SOURCE = re.compile(r"(according to|source[:d]|data from|figures from|research (by|from)|survey|study|report by|"
                    r"\bONS\b|\bABI\b|Met Office|Environment Agency|Office for National Statistics|Gov\.uk|\bNHS\b|"
                    r"\bFCA\b|\bRICS\b|\bBCIS\b|Which\?|Ofcom|Ofgem|university)", re.I)
QUOTE = re.compile(r"[“\"][^”\"]{20,}[”\"]\s*,?\s*(says|said|explains|adds|according)", re.I)
HEDGE = re.compile(r"\b(may|might|could)\b", re.I)


def sentences(text):
    text = re.sub(r"\s+", " ", text).strip()
    parts = re.split(r"(?<=[.!?])\s+(?=[A-Z“\"'(£$€0-9])", text)
    # Keep every non-empty sentence. Short ones ("Yes.", "It depends.") are often the deferral R1 must catch.
    return [p for p in parts if p.split()]


def parse(path):
    lines = open(path, encoding="utf-8").read().splitlines()
    meta = {"title": "", "updated": "", "author": ""}
    for l in lines[:3]:
        k, _, v = l.partition(": ")
        meta[k.strip().lower()] = v.strip()
    body = [MDLINK.sub(r"\1", l) for l in lines[3:]]
    out, skip = [], False
    for l in body:
        if re.match(r"^#{1,6}\s", l):
            h = l.lstrip("#").strip()
            skip = bool(re.search(r"in this article|transcript|table of contents", h, re.I))
            if skip:
                continue
        elif skip:
            continue
        if re.match(r"^\s*(published|last updated|updated|reviewed)\b", l, re.I) or re.search(r"mins? read", l, re.I):
            continue
        out.append(l)
    return meta, out


def blocks(lines):
    res, para, heading = [], [], None
    def flush():
        if para:
            res.append(("p", " ".join(para), heading)); para.clear()
    for l in lines:
        s = l.strip()
        if not s:
            flush(); continue
        m = re.match(r"^(#{1,6})\s+(.*)", s)
        if m:
            flush(); heading = (len(m.group(1)), m.group(2).strip("* ").strip()); res.append(("h", heading, heading)); continue
        if re.match(r"^([-*•]|\d+[.)])\s+", s):
            flush(); res.append(("li", re.sub(r"^([-*•]|\d+[.)])\s+", "", s), heading)); continue
        if s.startswith("|"):
            flush(); res.append(("tr", s, heading)); continue
        para.append(s)
    flush()
    return res


def months_since(d):
    if not d or d.lower() == "none":
        return None
    d = re.sub(r"\bSept\b", "Sep", d).strip()
    for fmt in ("%d %b %Y", "%d %B %Y", "%Y-%m-%d", "%B %d, %Y", "%b %d, %Y", "%d/%m/%Y"):
        try:
            x = datetime.strptime(d, fmt).date()
            return round((TODAY - x).days / 30.44, 1)
        except ValueError:
            pass
    return None


# ---------------------------------------------------------------- 1. metrics
def cmd_metrics():
    types = json.load(open("types.json")) if os.path.exists("types.json") else {}
    rows, openers, oid = [], [], 0
    led_by = collections.Counter()
    for path in sorted(glob.glob(f"{RAW}/*.md")):
        slug = os.path.basename(path)[:-3]
        meta, lines = parse(path)
        if lines and lines[0].startswith("FETCH_FAILED"):
            continue
        bl = blocks(lines)
        heads = [b[1] for b in bl if b[0] == "h" and b[1][0] in (2, 3) and not SKIP_HEADINGS.search(b[1][1])]
        prose = [b[1] for b in bl if b[0] == "p"]
        items = [b[1] for b in bl if b[0] == "li"]
        sents = [s for p in prose for s in sentences(p)]
        slen = [len(s.split()) for s in sents if len(s.split()) >= 3]  # sentence-length metric ignores fragments
        list_blocks, prev = 0, None
        for b in bl:
            if b[0] == "li" and prev != "li":
                list_blocks += 1
            prev = b[0]
        tables = sum(1 for i, b in enumerate(bl) if b[0] == "tr" and (i == 0 or bl[i - 1][0] != "tr"))
        text = " ".join(prose + items)
        words = len(text.split())
        for i, b in enumerate(bl):
            if b[0] == "h" and b[1][0] in (2, 3) and not SKIP_HEADINGS.search(b[1][1]):
                nxt = next((x for x in bl[i + 1:] if x[0] in ("p", "li", "tr", "h")), None)
                if nxt and nxt[0] in ("li", "tr"):
                    led_by["list" if nxt[0] == "li" else "table"] += 1  # not coded; reported separately
                if nxt and nxt[0] == "p":
                    ss = sentences(nxt[1]) or [nxt[1]]
                    oid += 1
                    openers.append({"id": oid, "slug": slug, "heading": b[1][1], "first_sentence": ss[0],
                                    "second_sentence": ss[1] if len(ss) > 1 else ""})
        rows.append({
            "slug": slug, "title": meta["title"], "type": types.get(slug, "Unassigned"),
            "words": words, "h2_h3": len(heads),
            "question_heading_pct": round(100 * sum(h.rstrip().endswith("?") for _, h in heads) / len(heads), 1) if heads else 0.0,
            "summary_block": any(b[0] == "h" and re.search(r"key points|summary|tl;?dr|at a glance|key takeaways", b[1][1], re.I) for b in bl),
            "list_blocks": list_blocks, "list_items": len(items), "tables": tables,
            "sentences": len(slen), "sentences_over_20": sum(x > 20 for x in slen),
            "avg_sentence_words": round(st.mean(slen), 1) if slen else None,
            "stats": len(STAT.findall(text)), "source_mentions": len(SOURCE.findall(text)),
            "expert_quotes": len(QUOTE.findall(text)), "hedges": len(HEDGE.findall(text)),
            "author_shown": meta["author"].lower() not in ("", "none"),
            "updated": meta["updated"], "months_since_update": months_since(meta["updated"]),
        })
    json.dump(rows, open("metrics.json", "w"), indent=1, ensure_ascii=False)
    json.dump(openers, open("openers.json", "w"), indent=1, ensure_ascii=False)
    json.dump(dict(led_by), open("openers_skipped.json", "w"))
    print(f"{len(rows)} articles, {len(openers)} section openers "
          f"({led_by['table']} sections open with a table and {led_by['list']} with a list; not coded)")
    unassigned = [r["slug"] for r in rows if r["type"] == "Unassigned"]
    if unassigned:
        print("WARNING: no content type for", unassigned)


# ---------------------------------------------------------------- 2. second-coder sample + agreement
def cmd_sample(n=40):
    o = json.load(open("openers.json"))
    random.seed(7)
    s = sorted(random.sample(o, min(int(n), len(o))), key=lambda x: x["id"])
    json.dump(s, open("sample_b.json", "w"), indent=1, ensure_ascii=False)
    print(f"{len(s)} openers written to sample_b.json")


def kappa(a, b):
    n = len(a); po = sum(x == y for x, y in zip(a, b)) / n
    ca, cb = collections.Counter(a), collections.Counter(b)
    pe = sum(ca[k] * cb[k] for k in set(ca) | set(cb)) / n / n
    return po, ((po - pe) / (1 - pe) if pe < 1 else 1.0)


def cmd_kappa():
    A = {x["id"]: x for x in json.load(open("coded_a.json"))}
    B = {x["id"]: x for x in json.load(open("coded_b.json"))}
    ids = [i for i in B if i in A]
    out = {}
    for f in ("opener", "standalone"):
        po, k = kappa([A[i][f] for i in ids], [B[i][f] for i in ids])
        out[f] = {"agreement": round(po, 3), "kappa": round(k, 2)}
        print(f"{f}: agreement {po:.0%}, Cohen's kappa {k:.2f}")
    out["n"] = len(ids)
    json.dump(out, open("agreement.json", "w"), indent=1)
    for i in ids:
        if A[i]["opener"] != B[i]["opener"] or A[i]["standalone"] != B[i]["standalone"]:
            print("  disagree", i, A[i]["opener"], B[i]["opener"], A[i]["standalone"], B[i]["standalone"])
    if min(v["kappa"] for k, v in out.items() if k != "n") < 0.7:
        print("Kappa below 0.7: tighten the rubric wording and recode before scoring.")


# ---------------------------------------------------------------- 3. rubric scoring, statistics, patterns
def lin(x, zero, full):
    if x is None:
        return 0.0
    return float(min(1.0, max(0.0, (x - zero) / (full - zero))))


# v2 weights (October 2026), anchored to published citation research. See the evidence table in SKILL.md Step 6.
RUBRIC = [
    ("R1", "Answerability", "Answer-first sections", 20, "% of sections whose first sentence answers the heading. 0 at 30%, full marks at 80%",
     lambda r: lin(r["answer_first_pct"], 30, 80)),
    ("R2", "Answerability", "Question-led headings", 6, "% of H2 and H3 headings written as questions. Full marks at 60%",
     lambda r: lin(r["question_heading_pct"], 0, 60)),
    ("R3", "Answerability", "Summary at the top", 7, "A key points or summary block is present",
     lambda r: 1.0 if r["summary_block"] else 0.0),
    ("R4", "Extractability", "Self-contained passages", 12, "% of section openings that make sense lifted out on their own. 0 at 70%, full marks at 95%",
     lambda r: lin(r["standalone_pct"], 70, 95)),
    ("R5", "Extractability", "Short sentences", 7, "Average sentence length. Full marks at 14 words or fewer, 0 at 22 or more",
     lambda r: lin(r["avg_sentence_words"], 22, 14)),
    ("R6", "Extractability", "Lists and tables", 14, "List blocks per 1,000 words (full marks at 5) for 70% of the score, plus at least one table for 30%",
     lambda r: 0.7 * lin(1000 * r["list_blocks"] / max(1, r["words"]), 0, 5) + 0.3 * (1.0 if r["tables"] else 0.0)),
    ("R7", "Evidence", "Statistics and specifics", 10, "Numbers, percentages or currency figures in the text. Full marks at 5",
     lambda r: lin(r["stats"], 0, 5)),
    ("R8", "Evidence", "Named sources", 10, "Mentions of a named source or data provider. Full marks at 2",
     lambda r: lin(r["source_mentions"], 0, 2)),
    ("R9", "Evidence", "Named expertise", 6, "An author or reviewer is shown, or a named expert is quoted",
     lambda r: 1.0 if (r["author_shown"] or r["expert_quotes"] > 0) else 0.0),
    ("R10", "Freshness", "Recently updated", 8, "Months since last update. Full marks at 3 or fewer, 0 at 12 or more",
     lambda r: lin(r["months_since_update"], 12, 3)),
]

TAXONOMY = [
    ("Deferral", "Opens with 'It depends', 'That's up to you', 'This can vary' or a bare 'Yes'",
     "Give the most common answer and the condition together: 'Usually, if you have…'",
     lambda a: a["opener"] == "B"),
    ("List set-up", "Announces a list instead of summarising it",
     "Summarise the list in one sentence, then give the list",
     lambda a: bool(re.search(r":\s*$|\bhere(?:'s| are| is)\b|\bbelow\b|the following|"
                              r"^there(?:'s| are| is) (?:a few|a couple|two|three|some|other|plenty|few|more|things)|"
                              r"different ways|\ba few (?:reasons|things|different factors|ways)", a["first_sentence"], re.I))),
    ("Question restated", "Opens by asking or repeating the question",
     "Answer it instead",
     lambda a: a["first_sentence"].rstrip().endswith("?") or bool(re.match(r"^you might be (?:thinking|wondering)", a["first_sentence"], re.I))),
    ("Advice preamble", "Says something is important or to check the policy, without the substance",
     "State the thing that's important",
     lambda a: bool(re.search(r"\bit'?s (?:important|worth|a good idea|essential|vital)|\bbe sure to check|\balways check|"
                              r"\bcheck your (?:policy|documents)|^understanding\b|\byou must\b|\bmake sure\b", a["first_sentence"], re.I))),
    ("Promotional teaser", "A sales or 'discover' line in place of the answer",
     "Keep the sales line for the end of the section",
     lambda a: bool(re.search(r"^(?:find out|discover|learn|watch this|ready|with (?:our|us)|we've got you covered|"
                              r"if you want to feel)", a["first_sentence"], re.I))),
    ("Reason before action", "An instruction heading that opens with why it matters, not what to do",
     "Instruction first, reason second",
     lambda a: bool(re.match(r"^(?:\d+\.\s*)?(?:move|turn|avoid|repair|look|lock|insure|check|help|how to|grab|keep|clear|"
                             r"fit|get|consider|protect|bleed|lag|leave|use|set|make|book|plan|switch|install|test|store)\b",
                             a["heading"].strip("* "), re.I))),
    ("Scene-setting", "A general or emotional framing line",
     "Delete the warm-up line; the second sentence is usually the right first sentence",
     lambda a: bool(re.search(r"isn'?t (?:just|only) about|does more than|make all the difference|can be (?:exciting|a sudden|the difference|tricky|daunting|stressful)|"
                              r"bit like|^while\b|^whether\b|don'?t panic|^fortunately|when it comes to|^in the future|^you might (?:think|find)|"
                              r"^thinking about|^in simple terms|^for some|^we all|^we know|^no one|^nobody|^many of us|^most of us|"
                              r"^let'?s face it|^life can|common sense|^having the right|^keeping your", a["first_sentence"], re.I))),
    ("Adjacent fact", "States something true and related, but not the answer to the heading",
     "Answer the heading in sentence 1; move supporting facts after it",
     lambda a: True),
]

PHRASES = {
    "'check your policy' and variants": r"check (?:your|the) (?:policy|policy documents|policy wording|policy details|documents|terms)",
    "'it depends' and variants": r"\bit depends\b|\bdepends on\b|\bcan vary\b|\bvaries\b|\bup to you\b",
    "'may', 'might' or 'could'": r"\b(?:may|might|could)\b",
    "'it's important' / 'it's worth' / 'a good idea'": r"\bit'?s (?:important|worth|a good idea)\b",
}


def answer_position(a):
    if a["opener"] == "A":
        return "Sentence 1"
    n = (a.get("note") or "").lower()
    if re.search(r"never|not given|no .*given|teaser|only in heading|rationale only|none in", n):
        return "Not in first two sentences"
    if re.search(r"after sentence 2|after colon|follow|listed after", n):
        return "After sentence 2"
    if "sentence 2" in n:
        return "Sentence 2"
    return "Not in first two sentences"


def cmd_score():
    import numpy as np
    from scipy import stats
    R = json.load(open("metrics.json"))
    O = {x["id"]: x for x in json.load(open("openers.json"))}
    A = json.load(open("coded_a.json"))
    for a in A:
        a.update({k: O[a["id"]][k] for k in ("slug", "heading", "first_sentence", "second_sentence")})
    per = collections.defaultdict(list)
    for a in A:
        per[a["slug"]].append(a)
    for r in R:
        L = per.get(r["slug"], [])
        r["answer_first_pct"] = round(100 * sum(x["opener"] == "A" for x in L) / len(L), 1) if L else None
        r["standalone_pct"] = round(100 * sum(x["standalone"] == "Y" for x in L) / len(L), 1) if L else None
    CR = [x[0] for x in RUBRIC]
    W = np.array([x[3] for x in RUBRIC], float)
    assert W.sum() == 100, "rubric weights must total 100"
    for r in R:
        r["crit"] = {c: round(fn(r), 3) for c, _, _, _, _, fn in RUBRIC}
        r["geo_score"] = round(float(np.dot([r["crit"][c] for c in CR], W)), 1)
    M = np.array([[r["crit"][c] for c in CR] for r in R]); S = M @ W
    rng = np.random.default_rng(42)
    out = {"n_articles": len(R), "n_sections": len(A), "total_words": sum(r["words"] for r in R), "date": str(TODAY)}
    boot = [np.mean(rng.choice(S, len(S), replace=True)) for _ in range(10000)]
    out["corpus"] = {"mean": round(float(S.mean()), 1), "median": round(float(np.median(S)), 1),
                     "ci95": [round(float(np.percentile(boot, 2.5)), 1), round(float(np.percentile(boot, 97.5)), 1)],
                     "n_below_50": int((S < 50).sum())}
    crit = []
    for i, (c, dim, name, w, rule, _) in enumerate(RUBRIC):
        col = M[:, i]
        crit.append({"id": c, "dimension": dim, "criterion": name, "weight": w, "rule": rule,
                     "mean_attainment_pct": round(100 * float(col.mean()), 1),
                     "articles_below_half": int((col < 0.5).sum()),
                     "avg_points_lost": round(float(w * (1 - col.mean())), 1)})
    out["criteria"] = crit
    lost = sorted(crit, key=lambda c: -c["avg_points_lost"])
    out["top3_share_of_lost_points"] = round(100 * sum(c["avg_points_lost"] for c in lost[:3]) / sum(c["avg_points_lost"] for c in crit), 1)
    # sensitivity
    loo = []
    for i in range(len(CR)):
        w2 = W.copy(); w2[i] = 0; loo.append(stats.spearmanr(S, M @ w2).statistic)
    pert = [stats.spearmanr(S, M @ (W * rng.uniform(0.5, 1.5, len(W)))).statistic for _ in range(1000)]
    out["sensitivity"] = {"equal_weights_rho": round(float(stats.spearmanr(S, M.mean(1)).statistic), 3),
                          "drop_one_min_rho": round(float(min(loo)), 3),
                          "random_weights_p5_rho": round(float(np.percentile(pert, 5)), 3)}
    # heading form vs direct answer
    q = [a for a in A if a["heading_form"] == "Q"]; s = [a for a in A if a["heading_form"] == "S"]
    hv = {"question": {"n": len(q), **{k: sum(a["opener"] == k for a in q) for k in "ABC"}},
          "statement": {"n": len(s), **{k: sum(a["opener"] == k for a in s) for k in "ABC"}}}
    if q and s:
        qa, sa = hv["question"]["A"], hv["statement"]["A"]
        odds, p = stats.fisher_exact([[qa, len(q) - qa], [sa, len(s) - sa]])
        hv.update(odds_ratio=round(float(odds), 2), fisher_p=float(f"{p:.2g}"))
    out["heading_vs_answer"] = hv
    # opener taxonomy
    counts = collections.Counter(); ex = collections.defaultdict(list)
    for a in A:
        if a["opener"] == "A":
            a["pattern"] = "Direct answer"; continue
        for name, _, _, rule in TAXONOMY:
            if rule(a):
                a["pattern"] = name; counts[name] += 1
                if len(ex[name]) < 4:
                    ex[name].append({"heading": a["heading"], "first": a["first_sentence"]})
                break
    out["opener_patterns"] = [{"pattern": n, "definition": d, "fix": f, "count": counts[n],
                               "articles": len({a["slug"] for a in A if a.get("pattern") == n}), "examples": ex[n]}
                              for n, d, f, _ in TAXONOMY if counts[n]]
    out["indirect_sections"] = sum(counts.values())
    out["articles_with_indirect"] = len({a["slug"] for a in A if a["opener"] != "A"})
    pc = collections.Counter(answer_position(a) for a in A)
    out["answer_position"] = {k: pc[k] for k in ["Sentence 1", "Sentence 2", "After sentence 2", "Not in first two sentences"]}
    # phrases
    texts = {r["slug"]: open(f"{RAW}/{r['slug']}.md", encoding="utf-8").read() for r in R}
    words = sum(r["words"] for r in R)
    out["phrases"] = []
    for label, pat in PHRASES.items():
        perart = {k: len(re.findall(pat, t, re.I)) for k, t in texts.items()}
        out["phrases"].append({"phrase": label, "total": sum(perart.values()),
                               "articles_using": sum(v > 0 for v in perart.values()),
                               "per_1000_words": round(1000 * sum(perart.values()) / words, 2)})
    # sentences, tables, evidence, freshness headlines
    tot = sum(r["sentences"] for r in R); over = sum(r["sentences_over_20"] for r in R)
    out["headlines"] = {"sentences": tot, "pct_sentences_over_20": round(100 * over / max(1, tot), 1),
                        "median_avg_sentence": st.median([r["avg_sentence_words"] for r in R if r["avg_sentence_words"]]),
                        "articles_no_table": sum(r["tables"] == 0 for r in R),
                        "articles_no_stats": sum(r["stats"] == 0 for r in R),
                        "articles_no_source": sum(r["source_mentions"] == 0 for r in R),
                        "articles_with_author": sum(r["author_shown"] for r in R),
                        "articles_with_expert_quote": sum(r["expert_quotes"] > 0 for r in R),
                        "articles_over_12_months": sum(r["months_since_update"] is not None and r["months_since_update"] > 12 for r in R),
                        "articles_within_3_months": sum(r["months_since_update"] is not None and r["months_since_update"] <= 3 for r in R),
                        "articles_no_date": sum(r["months_since_update"] is None for r in R)}
    # content types
    types = sorted({r["type"] for r in R})
    groups = [[r["geo_score"] for r in R if r["type"] == t] for t in types]
    bt = {"medians": {t: round(float(np.median(g)), 1) for t, g in zip(types, groups)}, "n": {t: len(g) for t, g in zip(types, groups)}}
    if len(types) > 1 and all(len(g) >= 2 for g in groups):
        H, p = stats.kruskal(*groups); bt.update(kruskal_H=round(float(H), 2), p=round(float(p), 3))
    out["by_type"] = bt
    # correlations
    pairs = []
    for i in range(len(CR)):
        for j in range(i + 1, len(CR)):
            if M[:, i].std() and M[:, j].std():
                rho, p = stats.spearmanr(M[:, i], M[:, j])
                if abs(rho) >= 0.4 and p < 0.05:
                    pairs.append({"a": RUBRIC[i][2], "b": RUBRIC[j][2], "rho": round(float(rho), 2), "p": round(float(p), 3)})
    lr = stats.spearmanr([r["words"] for r in R], [r["answer_first_pct"] or 0 for r in R])
    out["correlations"] = {"pairs": pairs, "length_vs_answer_first_rho": round(float(lr.statistic), 2)}
    if os.path.exists("agreement.json"):
        out["agreement"] = json.load(open("agreement.json"))
    json.dump(out, open("patterns.json", "w"), indent=1, ensure_ascii=False)
    json.dump(R, open("scores.json", "w"), indent=1, ensure_ascii=False)
    json.dump(A, open("coded_patterns.json", "w"), indent=1, ensure_ascii=False)
    with open("geo-scores.csv", "w", newline="") as f:
        w = csv.writer(f)
        w.writerow(["title", "slug", "content_type", "geo_score"] + [f"{x[0]} {x[2]} (0-1)" for x in RUBRIC] +
                   ["words", "answer_first_pct", "question_heading_pct", "avg_sentence_words", "stats", "months_since_update"])
        for r in sorted(R, key=lambda r: -r["geo_score"]):
            w.writerow([r["title"], r["slug"], r["type"], r["geo_score"]] + [f'{r["crit"][c]:.2f}' for c in CR] +
                       [r["words"], r["answer_first_pct"], r["question_heading_pct"], r["avg_sentence_words"], r["stats"], r["months_since_update"]])
    print(json.dumps({k: out[k] for k in ("corpus", "sensitivity", "heading_vs_answer", "answer_position", "by_type",
                                           "top3_share_of_lost_points", "headlines")}, indent=1))
    for c in crit:
        print(f'{c["criterion"]:26} met {c["mean_attainment_pct"]:5}%  below-half {c["articles_below_half"]:3}  lost {c["avg_points_lost"]}')
    for p in out["opener_patterns"]:
        print(f'{p["pattern"]:22} {p["count"]:4} sections in {p["articles"]} articles')
    for p in out["phrases"]:
        print(p)


# ---------------------------------------------------------------- 4. charts (compact, self-contained SVG)
FONT = 'font-family:Inter,-apple-system,"Segoe UI",Helvetica,Arial,sans-serif'
INK, INK2, GRID, S1, S2, S3, PALE = "#0b0b0b", "#52514e", "#ecebe7", "#2a78d6", "#eb6834", "#1baf7a", "#eef3fb"
RAMP = ["#f4f8fd", "#cde2fb", "#9ec5f4", "#6da7ec", "#3987e5", "#256abf", "#184f95", "#0d366b"]


def esc(s):
    return str(s).replace("&", "&amp;").replace("<", "&lt;").replace(">", "&gt;")


def svg_open(w, h, title, sub):
    return [f'<svg xmlns="http://www.w3.org/2000/svg" width="{w}" height="{h}" viewBox="0 0 {w} {h}">'
            f'<style>text{{{FONT};fill:{INK}}}.s{{fill:{INK2};font-size:12px}}.l{{font-size:12.5px}}.v{{fill:{INK2};font-size:11.5px}}</style>'
            f'<rect width="100%" height="100%" fill="#fff"/>'
            f'<text x="16" y="30" style="font-size:19px;font-weight:700">{esc(title)}</text>'
            f'<text x="16" y="52" class="s">{esc(sub)}</text>']


def ramp(v):
    t = max(0, min(1, v)) * (len(RAMP) - 1); i = int(min(len(RAMP) - 2, t)); f = t - i
    a = [int(RAMP[i][k:k + 2], 16) for k in (1, 3, 5)]; b = [int(RAMP[i + 1][k:k + 2], 16) for k in (1, 3, 5)]
    return "#%02x%02x%02x" % tuple(round(a[j] + (b[j] - a[j]) * f) for j in range(3))


def hbar(path, title, sub, rows, xmax, xlabel, back=None):
    """rows: [(label, value, value_label, back_value)] top to bottom."""
    L, W, top, rh = 210, 900, 78, 30
    plot = W - L - 140; H = top + rh * len(rows) + 50
    o = svg_open(W, H, title, sub)
    for k in range(0, int(xmax) + 1, max(1, int(xmax) // 5)):
        x = L + plot * k / xmax
        o.append(f'<line x1="{x:.1f}" y1="{top-6}" x2="{x:.1f}" y2="{top+rh*len(rows)}" stroke="{GRID}"/>'
                 f'<text x="{x:.1f}" y="{top+rh*len(rows)+16}" class="v" text-anchor="middle">{k}</text>')
    for i, (lab, v, vl, bk) in enumerate(rows):
        y = top + i * rh
        o.append(f'<text x="{L-10}" y="{y+18}" class="l" text-anchor="end">{esc(lab)}</text>')
        if bk is not None:
            o.append(f'<rect x="{L}" y="{y+5}" width="{plot*bk/xmax:.1f}" height="19" rx="2" fill="{PALE}"/>')
        o.append(f'<rect x="{L}" y="{y+5}" width="{max(1, plot*v/xmax):.1f}" height="19" rx="2" fill="{S1}"/>')
        o.append(f'<text x="{L+plot*max(v, bk or 0)/xmax+8:.1f}" y="{y+18}" class="v">{esc(vl)}</text>')
    o.append(f'<text x="{L+plot/2}" y="{H-10}" class="v" text-anchor="middle">{esc(xlabel)}</text></svg>')
    open(path, "w").write("".join(o))


def cmd_charts():
    os.makedirs("charts", exist_ok=True)
    P = json.load(open("patterns.json")); R = json.load(open("scores.json"))
    n = P["n_articles"]
    # 1. points lost
    cr = sorted(P["criteria"], key=lambda c: -c["avg_points_lost"])
    hbar("charts/1-points-lost.svg", "Where the GEO score is lost",
         f'Average points lost per article on each rubric criterion, {n} articles. Mean score {P["corpus"]["mean"]} out of 100.',
         [(c["criterion"], c["avg_points_lost"], f'{c["avg_points_lost"]} of {c["weight"]}', c["weight"]) for c in cr],
         max(c["weight"] for c in cr), "Average points lost per article (pale bar = points available)")
    # 2. heatmap
    CR = [c["id"] for c in P["criteria"]]; names = {c["id"]: c["criterion"] for c in P["criteria"]}
    rows = sorted(R, key=lambda r: -r["geo_score"])
    LW, CW, CH, TOP = 330, 52, 22, 170
    W = LW + len(CR) * CW + 70; H = TOP + len(rows) * CH + 52
    o = svg_open(W, H, "Every article, every criterion", "Share of each rubric criterion met, articles sorted by GEO score. Pale columns are consistent patterns.")
    for j, c in enumerate(CR):
        o.append(f'<text class="v" transform="translate({LW+j*CW+CW/2+4} {TOP-8}) rotate(-50)">{esc(names[c])}</text>')
    o.append(f'<text x="{LW+len(CR)*CW+10}" y="{TOP-8}" class="v" style="font-weight:700">Score</text>')
    for i, r in enumerate(rows):
        y = TOP + i * CH; t = r["title"] if len(r["title"]) < 48 else r["title"][:46] + "…"
        o.append(f'<text x="{LW-10}" y="{y+CH/2+4}" class="l" text-anchor="end">{esc(t)}</text>')
        o += [f'<rect x="{LW+j*CW}" y="{y}" width="{CW-2}" height="{CH-2}" fill="{ramp(r["crit"][c])}"/>' for j, c in enumerate(CR)]
        o.append(f'<text x="{LW+len(CR)*CW+12}" y="{y+CH/2+4}" class="l" style="font-weight:700">{r["geo_score"]:.0f}</text>')
    ly = H - 34
    o.append(f'<text x="{LW-10}" y="{ly+10}" class="s" text-anchor="end">Criterion met</text>')
    o += [f'<rect x="{LW+k*24}" y="{ly}" width="22" height="12" fill="{ramp(k/10)}"/>' for k in range(11)]
    o.append(f'<text x="{LW}" y="{ly+26}" class="v">0%</text><text x="{LW+120}" y="{ly+26}" class="v">50%</text><text x="{LW+238}" y="{ly+26}" class="v">100%</text></svg>')
    open("charts/2-heatmap.svg", "w").write("".join(o))
    # 3. heading form vs answer (100% stacked)
    hv = P["heading_vs_answer"]; W, L, plot = 900, 170, 680
    p_txt = f' Fisher\'s exact test, p = {hv["fisher_p"]}.' if "fisher_p" in hv else ""
    o = svg_open(W, 250, "Are question headings answered?", f'How the first sentence responds to its heading, {P["n_sections"]} sections.{p_txt}')
    segs = [("A", "Answers in sentence 1", S1, "#fff"), ("B", "Defers ('it depends')", S2, "#fff"), ("C", "Leads in with something else", S3, INK)]
    for i, (key, lab) in enumerate([("question", "Question headings"), ("statement", "Statement headings")]):
        d = hv[key]; y = 80 + i * 62; x = L
        o.append(f'<text x="{L-10}" y="{y+18}" class="l" text-anchor="end">{lab}</text><text x="{L-10}" y="{y+34}" class="v" text-anchor="end">({d["n"]} sections)</text>')
        for k, _, col, tc in segs:
            if not d["n"] or not d[k]:
                continue
            w = plot * d[k] / d["n"]
            o.append(f'<rect x="{x:.1f}" y="{y}" width="{w-2:.1f}" height="40" fill="{col}"/>')
            if w > 34:
                o.append(f'<text x="{x+w/2:.1f}" y="{y+25}" text-anchor="middle" style="font-weight:700;font-size:13px;fill:{tc}">{100*d[k]/d["n"]:.0f}%</text>')
            x += w
    lx = L
    for _, lab, col, _ in segs:
        o.append(f'<rect x="{lx}" y="222" width="14" height="10" fill="{col}"/><text x="{lx+20}" y="231" class="v">{lab}</text>'); lx += 220
    open("charts/3-heading-vs-answer.svg", "w").write("".join(o) + "</svg>")
    # 4. opener patterns
    op = sorted(P["opener_patterns"], key=lambda p: -p["count"])
    hbar("charts/4-opener-patterns.svg", f"{len(op)} habits behind the indirect openings",
         f'{P["indirect_sections"]} of {P["n_sections"]} sections don\'t answer in the first sentence. Each is classified by how it opens instead.',
         [(p["pattern"], p["count"], f'{p["count"]} in {p["articles"]} articles', None) for p in op],
         max(5, max(p["count"] for p in op) * 1.25), "Number of sections")
    # 5. score by content type (strip plot)
    types = sorted(P["by_type"]["medians"], key=lambda t: -P["by_type"]["medians"][t])
    W, L, plot, top, rh = 900, 220, 560, 78, 46; H = top + rh * len(types) + 50
    ptxt = f' Kruskal-Wallis p = {P["by_type"]["p"]}.' if "p" in P["by_type"] else ""
    o = svg_open(W, H, "GEO score by content type", f"GEO score per article by content type.{ptxt}")
    for k in range(0, 101, 25):
        x = L + plot * k / 100
        o.append(f'<line x1="{x}" y1="{top-6}" x2="{x}" y2="{top+rh*len(types)}" stroke="{GRID}"/><text x="{x}" y="{top+rh*len(types)+16}" class="v" text-anchor="middle">{k}</text>')
    for i, t in enumerate(types):
        y = top + i * rh + rh / 2; v = [r["geo_score"] for r in R if r["type"] == t]
        o.append(f'<text x="{L-10}" y="{y+4}" class="l" text-anchor="end">{esc(t)} ({len(v)})</text>')
        for k, s in enumerate(v):
            jy = y - 9 + 18 * k / max(1, len(v) - 1)
            o.append(f'<circle cx="{L+plot*s/100:.1f}" cy="{jy:.1f}" r="5" fill="{S1}" stroke="#fff" stroke-width="1.5"/>')
        m = P["by_type"]["medians"][t]
        o.append(f'<line x1="{L+plot*m/100:.1f}" y1="{y-15}" x2="{L+plot*m/100:.1f}" y2="{y+15}" stroke="{INK}" stroke-width="2.5"/>'
                 f'<text x="{L+plot+14}" y="{y+4}" class="v">median {m:.0f}</text>')
    o.append(f'<text x="{L+plot/2}" y="{H-10}" class="v" text-anchor="middle">GEO score (0-100)</text></svg>')
    open("charts/5-score-by-type.svg", "w").write("".join(o))
    for f in sorted(glob.glob("charts/*.svg")):
        print(f, os.path.getsize(f), "bytes")


def cmd_section(slug, head):
    on, lvl = False, 9
    for l in open(f"{RAW}/{slug}.md", encoding="utf-8").read().splitlines():
        m = re.match(r"^(#{1,6})\s+(.*)", l)
        if m:
            if on and len(m.group(1)) <= lvl:
                break
            if head.lower() in m.group(2).lower():
                on, lvl = True, len(m.group(1))
        if on:
            print(l)


if __name__ == "__main__":
    c = sys.argv[1] if len(sys.argv) > 1 else ""
    if c in ("metrics", "kappa", "score", "charts"):
        {"metrics": cmd_metrics, "kappa": cmd_kappa, "score": cmd_score, "charts": cmd_charts}[c]()
    elif c == "sample":
        cmd_sample(sys.argv[2] if len(sys.argv) > 2 else 40)
    elif c == "section":
        cmd_section(sys.argv[2], sys.argv[3])
    else:
        print(__doc__)
```