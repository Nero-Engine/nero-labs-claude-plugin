---
name: filter-dataset-rows
description: Filter, sort, dedupe or reshape rows in a CSV, Excel file, Google Sheet, JSON array or Apify dataset. Use when the user asks to keep only rows that match a rule ("only businesses with no website", "rows where price is under 50", "leads from the last 30 days"), to fix a scraper's ignored date filter, to rename, cast or compute columns, to sort and take the top N, or to export the result as CSV or Excel.
---

# Dataset Filter & Transform

Runs the Nero Labs Apify Actor `nerolabs/dataset-filter-transform` (https://apify.com/nerolabs/dataset-filter-transform) through the connector tool `nerolabs--dataset-filter-transform`.

## Use it for requests like

- "Filter this Apify dataset to rows with no website"
- "Only keep listings under 200 pounds, newest first"
- "My scraper ignored the date range, give me only posts from September"
- "Rename these columns and give me an Excel file"

## What to pass

- `filters`: list of `{"field": "website", "operator": "isEmpty"}` style rules. 25 operators include equals, notEquals, contains, notContains, startsWith, endsWith, in, notIn, greaterThan, greaterOrEqual, lessThan, lessOrEqual, between, isEmpty, isNotEmpty, matchesRegex, before, after and arrayContains.
- `filterCombineMode`: `AND` (default, every rule) or `OR` (any rule).
- `transforms`: steps applied before filtering (rename, cast, compute, regex extract). Dotted paths like `address.city` work.
- `lenientNumbers: true` reads "$1,234.50" or "49 USD" as numbers. Turn it on for scraped prices.
- `sortBy` (e.g. `["-rating"]` for descending), `distinctBy` (keep one row per value), `limit`, `offset`.
- `exportFormats`: `["csv"]`, `["xlsx"]` or both for a downloadable file.
- `webhookUrl`: POST the result to Slack, Zapier, Make or n8n when the run ends.

## How to run it

1. Call the `nerolabs--dataset-filter-transform` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/dataset-filter-transform` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.002 per row kept (rows filtered out are free). 1,000 kept rows = about $2.
- $0.01 per exported file, $0.02 per confirmed webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems or limit`) so the bill cannot run away.

## What comes back

The kept, transformed rows, in order, plus file links if you asked for exports. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
