---
name: make-charts-and-pdf-report
description: Turn a spreadsheet, CSV, JSON or Apify dataset into chart images (bar, line, area, pie, donut, scatter as PNG and SVG) and an optional PDF or HTML report with KPIs and a short plain-English summary. Use when the user wants charts as files to share, a client-ready PDF report, or a chart of data too large to paste into the chat.
---

# Chart Generator & PDF Report from Any Dataset

Runs the Nero Labs Apify Actor `nerolabs/dataset-charts-report` (https://apify.com/nerolabs/dataset-charts-report) through the connector tool `nerolabs--dataset-charts-report`.

## Use it for requests like

- "Make a bar chart of sales by region from this CSV"
- "Turn this dataset into a PDF report for my client"
- "Line chart of daily signups as a PNG"
- "Build a monthly report with 3 charts from this sheet"

## What to pass

- `charts` (required): up to 20, e.g. `[{"type": "bar", "title": "Sales by region", "x": "region", "y": "amount", "aggregate": "sum"}]`. Types: bar, horizontalBar, stackedBar, line, area, pie, donut, scatter.
- `generateReport` (on by default), `reportTitle`, `reportSubtitle`, `reportFormats` (`pdf`, `html`).
- `theme` (`light` or `dark`), `accentColor`, `imageFormats`, `width`, `height`, `scale`.
- `aiSummary: true` adds 3 to 6 sentences written from the KPIs only.

## How to run it

1. Call the `nerolabs--dataset-charts-report` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/dataset-charts-report` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.03 per chart drawn. $0.40 per report built. A report with 3 charts = $0.49.
- $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxRows`) so the bill cannot run away.

## What comes back

Public links to every chart image and to the PDF or HTML report, plus the KPI values. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
