---
name: extract-text-from-pdfs
description: Extract text, tables and metadata from many PDFs at once. Use when the user has a list, sheet or dataset of PDF links (reports, filings, contracts, price lists, brochures, research papers) and wants the text or tables from each as plain text or Markdown for an LLM, kept next to their own columns. Scanned PDFs with no text layer are flagged, not guessed.
---

# PDF Extractor: Bulk PDF to Text, Tables & Markdown

Runs the Nero Labs Apify Actor `nerolabs/dataset-pdf-extract` (https://apify.com/nerolabs/dataset-pdf-extract) through the connector tool `nerolabs--dataset-pdf-extract`.

## Use it for requests like

- "Pull the text out of every PDF in this list"
- "Extract the tables from these PDF price lists"
- "Turn these PDFs into Markdown I can feed to an AI"
- "Which of these PDF links are broken?"

## What to pass

- Source: `datasetId`, `fileUrl`, `pdfUrls` (a plain list) or `data`. `urlField` detects the link column automatically.
- `textFormat`: `plain`, `markdown` (tables appended as Markdown, best for LLMs) or `none`.
- `extractTables: true` rebuilds tables as rows of cells.
- `firstPage`, `lastPage`, `maxCharsPerPdf` to keep rows small.
- `keep`: `all`, `extracted`, `needs_ocr` or `problems` (broken links).
PDFs flagged `needs_ocr` are scans: send those to the ocr-images-and-scanned-pdfs skill.

## How to run it

1. Call the `nerolabs--dataset-pdf-extract` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/dataset-pdf-extract` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `pdfUrls`: a plain list of PDF links.
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.01 per PDF that returned real text. 100 PDFs = about $1.
- $0.002 per PDF that has no text layer (flagged for OCR).
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

Each PDF's text (or Markdown), tables, page count and metadata. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
