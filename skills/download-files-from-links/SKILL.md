---
name: download-files-from-links
description: Download every image or file linked in a CSV, Excel file, Google Sheet, JSON array or Apify dataset, store each with a public link, and optionally zip them all. Use when the user asks to download all product images, property photos, PDFs or attachments from a list or scraper output, rename files by SKU or ID, or get one ZIP of every file.
---

# Bulk Image & File Downloader

Runs the Nero Labs Apify Actor `nerolabs/bulk-file-downloader` (https://apify.com/nerolabs/bulk-file-downloader) through the connector tool `nerolabs--bulk-file-downloader`.

## Use it for requests like

- "Download all the product images in this sheet"
- "Save every photo from this scraper run and zip them"
- "Name each file by its SKU"
- "Which of these image links are broken?"

## What to pass

- Source: `datasetId`, `fileUrl`, `fileUrls` (a plain list) or `data`.
- `urlField`: the link column, or several separated by commas (`"mainImage, gallery"`); detected automatically if empty.
- `fileTypes`: only keep certain types, judged from the bytes.
- `fileNameField`: name files by a column, e.g. `"sku"`.
- `createZip: true` for one ZIP (split into parts when large).
- `storeName`: keep files in a named store that does not expire with the run.
- `maxFiles`: cost cap on files downloaded.

## How to run it

1. Call the `nerolabs--bulk-file-downloader` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/bulk-file-downloader` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `fileUrls`: a plain list of links.
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.004 per file stored, plus $0.003 per full 10 MB. 1,000 small images = about $4. Broken links, web pages and repeats are free.
- ZIPs are charged by size like files. $0.01 per exported results file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxFiles`) so the bill cannot run away.

## What comes back

One row per link with the stored file's public link, size and type, plus ZIP links. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
