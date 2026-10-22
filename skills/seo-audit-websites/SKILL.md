---
name: seo-audit-websites
description: Run an on-page SEO audit on many websites at once and get a 0-100 SEO score, an A to F grade and a plain-English list of what to fix for each (title, meta description, H1, noindex, HTTPS, mobile viewport, thin content, image alt text, Open Graph, structured data, sitemap, robots.txt, broken links). Use when the user has a list, sheet or scraper dataset of websites (for example Google Maps leads or client sites) and asks which have bad SEO, wants SEO problems as a sales opener, wants a monthly SEO check of client sites, or wants an SEO audit of one or more pages.
---

# SEO Audit Tool: Bulk Website Checker for CSV, Sheet or Dataset

Runs the Nero Labs Apify Actor `nerolabs/bulk-seo-audit` (https://apify.com/nerolabs/bulk-seo-audit) through the connector tool `nerolabs--bulk-seo-audit`.

## Use it for requests like

- "Audit the SEO of every website in this lead list"
- "Which of these Google Maps businesses have weak websites?"
- "Give me an SEO score and what to fix for these 50 client sites"
- "Check these pages for broken links and missing meta descriptions"

## What to pass

- `urls`: a plain list of websites or pages (bare domains work), for a quick run with no spreadsheet.
- `urlField`: the website column, detected automatically if empty (`website`, `domain`, `url`...).
- `checkLinks: true` also tests the links on each page (up to `maxLinksPerPage`, default 50) and lists broken ones; charged per link.
- `checkSite` (default true) checks each site once for robots.txt and an XML sitemap, free.
- `keep`: `all`, `audited`, `below_score` (with `scoreThreshold`, default 70: the quick way to find prospects with weak SEO) or `problems`.
- `respectRobotsTxt` (default true; turn off for the user's own sites). `exportFormats` for CSV or Excel.

## How to run it

1. Call the `nerolabs--bulk-seo-audit` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/bulk-seo-audit` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `urls`: a plain list of websites or pages.
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.005 per page audited. 1,000 sites = about $5. Blocked, unreachable, error and duplicate rows are free.
- $0.0005 per link checked, only when `checkLinks` is on.
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

Each row with `seoScore`, `grade`, `issues` and `issueSummary` (what to fix, worst first), plus `pageTitle`, `metaDescription`, `h1`, `isHttps`, `hasViewport`, `noindex`, `wordCount`, `structuredDataTypes`, `sitemapUrl` and broken links. The user's own columns are kept. Check `auditStatus` is `ok` before using a score. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
