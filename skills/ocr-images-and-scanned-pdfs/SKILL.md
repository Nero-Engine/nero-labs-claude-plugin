---
name: ocr-images-and-scanned-pdfs
description: Read the text out of images and scanned PDFs in bulk with OCR. Use when the user has a list, sheet or dataset of image or scanned PDF links (receipts, invoices, letters, photos of documents, screenshots, product labels) and wants the text from each, in English or other languages, kept next to their own columns.
---

# Bulk OCR: Image & Scanned PDF to Text

Runs the Nero Labs Apify Actor `nerolabs/bulk-ocr-image-pdf-to-text` (https://apify.com/nerolabs/bulk-ocr-image-pdf-to-text) through the connector tool `nerolabs--bulk-ocr-image-pdf-to-text`.

## Use it for requests like

- "OCR every receipt image in this sheet"
- "Get the text out of these scanned PDFs"
- "Read the text on these product photos"
- "Turn these screenshots into text"

## What to pass

- Source: `datasetId`, `fileUrl` (CSV, Excel or Google Sheet with a link column), `fileUrls` (a plain list of image or PDF links) or `data`.
- `urlField`: the link column, detected automatically if left empty.
- `languages`: Tesseract codes, e.g. `["eng"]`, `["eng","spa"]`, `["deu"]`.
- `usePdfTextLayer: true` (default) copies text from born-digital PDF pages at the cheap rate instead of OCRing them.
- `pageSegmentation`: `auto` for documents, `sparse` for screenshots and photos.
- `maxPagesPerFile`: cost ceiling per PDF. `includePageText: true` adds per-page text.
For PDFs that already contain text (not scans), use the extract-text-from-pdfs skill instead; it also rebuilds tables.

## How to run it

1. Call the `nerolabs--bulk-ocr-image-pdf-to-text` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/bulk-ocr-image-pdf-to-text` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `fileUrls`: a plain list of image or PDF links.
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.005 per page read by OCR (an image is one page). Blank pages are free. 1,000 pages = about $5.
- $0.001 per PDF page copied from its own text layer.
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems and maxPagesPerFile`) so the bill cannot run away.

## What comes back

Each row with its recognised text, confidence and page count. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
