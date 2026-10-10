---
name: check-website-speed
description: Run Google PageSpeed Insights (Lighthouse) on many websites at once and get performance, SEO, accessibility and best-practice scores, Core Web Vitals and the top speed fixes for each. Use when the user asks how fast a list of sites is, which leads have slow websites, to audit Core Web Vitals across a sheet or dataset, or to track site speed on a schedule.
---

# Bulk PageSpeed & Lighthouse Checker

Runs the Nero Labs Apify Actor `nerolabs/bulk-pagespeed-checker` (https://apify.com/nerolabs/bulk-pagespeed-checker) through the connector tool `nerolabs--bulk-pagespeed-checker`.

## Use it for requests like

- "Check the PageSpeed score of every site in this sheet"
- "Which of these leads have slow mobile websites?"
- "Core Web Vitals for all our client sites"
- "Lighthouse SEO and accessibility scores for these URLs"

## What to pass

- Source: `urls` (a plain list of web addresses), `datasetId`, `fileUrl` or `data`. `urlField` detects the website column automatically; bare domains work.
- `strategy`: `mobile` (default, what Google ranks on), `desktop` or `both` (two tests, two charges).
- `categories`: which Lighthouse scores to return (same price either way).
- `maxOpportunities`: how many speed fixes to list per page.
- `keep`: `all`, `slow`, `fast`, `cwv_fail` or `problems`, with `slowBelowScore` (default 50).
No Google API key needed; the Actor includes one.

## How to run it

1. Call the `nerolabs--bulk-pagespeed-checker` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/bulk-pagespeed-checker` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.005 per completed report (one page on one device). 200 sites on mobile = about $1. Pages Google could not load are free.
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

Each row with its scores, Core Web Vitals (LCP, CLS, INP, FCP, TBT) pass or fail, and the biggest fixes. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
