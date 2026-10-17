---
name: merge-datasets-remove-duplicates
description: Combine many lists into one and remove the duplicates between them: several Apify datasets (for example the same scraper run once per city, keyword or week), CSV, Excel or JSON files and Google Sheets. Use when the user asks to merge scraper runs, stack several sheets into one, dedupe across lists by phone, website, placeId or URL, keep the most complete copy of each record, fill blanks from the duplicates, or see which list each row came from.
---

# Merge Datasets & Remove Duplicates

Runs the Nero Labs Apify Actor `nerolabs/dataset-merge-dedupe` (https://apify.com/nerolabs/dataset-merge-dedupe) through the connector tool `nerolabs--dataset-merge-dedupe`.

## Use it for requests like

- "Merge my 10 Google Maps runs into one list without duplicate businesses"
- "Combine these three Google Sheets and remove duplicate emails"
- "Stack last week's and this week's scrape and keep one row per URL"
- "Which of these leads appear in more than one of my lists?"

## What to pass

Give it as many sources as needed, all merged together:
- `datasetIds`: a list of Apify dataset IDs.
- `fileUrls`: a list of public CSV, TSV, Excel, JSON or JSON Lines links, or normal Google Sheets links shared as "Anyone with the link can view".
- `data`: a JSON array of rows, merged in as one more list (use it for small data from the chat or a local file, since the Actor runs in the cloud).
- `dedupKeys`: fields that identify the same record, e.g. `["phone"]`, `["placeId"]`, `["website"]` or `["name", "city"]`. Empty compares whole rows. Names must match exactly.
- `dedupMode`: `normalized` (default; ignores case, spaces, punctuation, http and www), `exact`, `fuzzy` (near matches, up to 5,000 rows, `similarityThreshold` 0.9) or `none` (just stack the lists).
- `keepStrategy`: `most_complete` (default), `first` or `last`. `fillBlanksFromDuplicates` (default true) copies missing values from the removed copies.
- `addSourceInfo` (default true) adds `_source`, `_foundInSources` and `_duplicatesMerged` to each row.
- `exportFormats`: `["csv"]` and/or `["xlsx"]`. `webhookUrl` to POST the result.

## How to run it

1. Call the `nerolabs--dataset-merge-dedupe` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/dataset-merge-dedupe` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.001 per merged row written (rows read and duplicates removed are free). 10,000 unique rows = about $10.
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

One row per unique record with the source columns, plus how many duplicates were removed and blanks filled, and CSV and Excel links. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
