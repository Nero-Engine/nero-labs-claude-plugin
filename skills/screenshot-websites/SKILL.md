---
name: screenshot-websites
description: Take screenshots of many websites at once, desktop and or mobile, with cookie banners hidden, and get a public image link for each. Use when the user asks to screenshot every site in a sheet, CSV, list or Google Maps scraper dataset, for outreach videos, website audits, lead lists, competitor research or a visual check of many homepages.
---

# Bulk Website Screenshots

Runs the Nero Labs Apify Actor `nerolabs/bulk-website-screenshots` (https://apify.com/nerolabs/bulk-website-screenshots) through the connector tool `nerolabs--bulk-website-screenshots`.

## Use it for requests like

- "Screenshot every site in this sheet"
- "Get desktop and mobile screenshots of these 200 websites"
- "Add a homepage screenshot to each lead"
- "Full-page screenshots of these competitor sites"

## What to pass

- Source: `datasetId`, `fileUrl`, `urls` (a plain list; `example.com` works without https) or `data`.
- `websiteField`: the website column, detected automatically if empty.
- `devices`: `["desktop"]`, `["mobile"]` or both (each device is its own screenshot and charge).
- `fullPage: false` (default) for above the fold, `true` for the whole page.
- `imageFormat`: `jpeg` (default, small) or `png`. `hideCookieBanners: true` by default.
- `waitSecs` for slow animated sites.

## How to run it

1. Call the `nerolabs--bulk-website-screenshots` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/bulk-website-screenshots` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `urls`: a plain list of websites.
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.004 per screenshot stored. 500 sites on desktop only = about $2. Dead, blocked or repeated sites are never charged.
- $0.001 per run start, $0.01 per exported file.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

Each row with a public screenshot link per device, plus status for sites that failed. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
