---
name: extract-article-text
description: Get the full text of many news or blog articles at once, with headline, author, publish date, publisher, image, tags and word count. Use when the user has a list, sheet or scraper dataset of article links (Google News, RSS, search results, a sitemap) and wants the stories themselves, wants articles as clean Markdown for an LLM or RAG pipeline, wants to monitor news daily and read only new stories, or wants a coverage list of who wrote what and when.
---

# Bulk Article Extractor: Full Text from a CSV, Sheet or Dataset

Runs the Nero Labs Apify Actor `nerolabs/bulk-article-extractor` (https://apify.com/nerolabs/bulk-article-extractor) through the connector tool `nerolabs--bulk-article-extractor`.

## Use it for requests like

- "Get the full text of every article in this Google News scrape"
- "Turn these 200 blog links into Markdown I can feed to an LLM"
- "Who wrote each of these articles and when were they published?"
- "Every morning, give me only the new stories from this feed"

## What to pass

- `articleUrls`: a plain list of links, for a quick run with no spreadsheet.
- `urlField`: the link column, detected automatically if empty (`url`, `link`, `articleUrl`...).
- `textFormat`: `plain` (default), `markdown` (adds a markdown column, best for LLMs), `html` or `none` (metadata only).
- `onlyNewUrls: true` with a `stateName` per feed skips links already extracted in earlier runs, free; made for daily monitoring.
- `minWords` (default 50): shorter pages (videos, galleries, paywall stubs) are marked `too_short` and free.
- `respectRobotsTxt` stays on. `proxyConfiguration` is off by default and rarely helps: big publishers that block (NYT, Forbes) block by browser check, not address.
- `keep`: `all`, `extracted` or `problems`. `exportFormats` for CSV or Excel.

## How to run it

1. Call the `nerolabs--bulk-article-extractor` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/bulk-article-extractor` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.002 per article returned. 1,000 articles = about $2.
- Blocked, failed, duplicate and paywall-preview links are free.
- $0.01 per exported file (only when the run returned an article), $0.005 per webhook delivery. Residential proxy, if turned on, adds $0.01 per article.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

Each row with `extractStatus`, `title`, `author`, `publishedAt`, `siteName`, `imageUrl`, `wordCount`, `paywalled` and the full `text` (plus `markdown` if asked). Check `extractStatus` is `ok` before using a row. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
