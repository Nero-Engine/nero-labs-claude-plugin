---
name: upscale-images
description: Upscale many images at once 2x, 3x or 4x with AI (Real-ESRGAN) and get a public link to each sharper, larger image. Use when the user asks to upscale or enhance low-resolution product photos, catalogue images, property photos or scraped images in a sheet, CSV, list or dataset, or to make small images print or web ready.
---

# Bulk AI Image Upscaler

Runs the Nero Labs Apify Actor `nerolabs/bulk-image-upscaler` (https://apify.com/nerolabs/bulk-image-upscaler) through the connector tool `nerolabs--bulk-image-upscaler`.

## Use it for requests like

- "Upscale every product photo in this sheet 4x"
- "Make these small images sharper"
- "Enhance the low-res images from this scraper run"
- "Upscale only the images under 800 pixels"

## What to pass

- Source: `fileUrl`, `datasetId`, `imageUrls` (a plain list) or `data`. `urlField` detects the link column automatically.
- `scale`: `"4"`, `"3"` or `"2"` (same price).
- `outputFormat`: `auto` (keeps transparency), `jpg`, `png` or `webp`.
- `skipIfLongEdgeAtLeast`: skip, free, images already big enough.
- `maxInputMegapixels`: large inputs are shrunk first so the charge stays small.
- `fileNameField`, `storeName`, `maxImages` (cost cap).

## How to run it

1. Call the `nerolabs--bulk-image-upscaler` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/bulk-image-upscaler` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `imageUrls`: a plain list of image links.
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.015 per image delivered, plus $0.015 per started megapixel above the first 1 MP of input. A 500 x 500 photo costs $0.015; a 2000 x 2000 photo costs $0.06. Failed downloads, non-images and skipped images are free.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxImages`) so the bill cannot run away.

## What comes back

Each row with the upscaled image's public link and its new dimensions. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
