---
name: clean-and-dedupe-data
description: Remove duplicates and clean a messy CSV, Excel file, Google Sheet, JSON array or Apify dataset, then export it as a spreadsheet. Use when the user asks to dedupe leads by email or website, merge near-duplicate company names, flatten nested JSON into columns, normalise emails, phones and URLs, strip HTML, drop or rename columns, or turn scraper output into a clean CSV or Excel file.
---

# Dataset Cleaner & Exporter

Runs the Nero Labs Apify Actor `nerolabs/dataset-cleaner-exporter` (https://apify.com/nerolabs/dataset-cleaner-exporter) through the connector tool `nerolabs--dataset-cleaner-exporter`.

## Use it for requests like

- "Remove duplicate leads from this sheet, match on email"
- "Merge near-duplicate business names"
- "Flatten this scraper output into a spreadsheet"
- "Clean the phone numbers and give me an Excel file"

## What to pass

- `dedupMode`: `exact`, `normalized` (ignores case and spacing, the usual pick) or `fuzzy` (near-duplicates; set `similarityThreshold`, 0.9 is a good start).
- `dedupKeys`: fields that define a duplicate, e.g. `["email"]` or `["website"]`. Empty compares the whole row.
- `keepStrategy`: `most_complete` (default), `first` or `last`.
- `flatten: true` turns `address.city` into an `address_city` column. `expandArrayField` explodes one array into rows.
- `cleanFields`, `stripHtml`, `coerceTypes`, `emptyToNull`, `dropEmptyFields`: cleaning switches.
- `columnsToKeep`, `columnsToRemove`, `columnRenameMap` (`"old:new"` lines).
- `exportFormats`: `["csv"]` and/or `["xlsx"]`.

## How to run it

1. Call the `nerolabs--dataset-cleaner-exporter` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/dataset-cleaner-exporter` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.002 per cleaned record written. 1,000 records = about $2.
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

The deduplicated, cleaned records, how many duplicates were merged, and file links. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
