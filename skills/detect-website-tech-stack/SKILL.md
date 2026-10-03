---
name: detect-website-tech-stack
description: Find out what technology many websites run on: Shopify, WordPress, WooCommerce, Wix, HubSpot, Klaviyo, Google Analytics, Stripe, email hosting like Google Workspace or Microsoft 365, and 7,600 more. Use when the user wants to know a site's platform, find every Shopify store in a list, qualify leads by the tools they use, or enrich a sheet or dataset of websites with their tech stack.
---

# Tech Stack Detector (Wappalyzer and BuiltWith alternative)

Runs the Nero Labs Apify Actor `nerolabs/tech-stack-detector` (https://apify.com/nerolabs/tech-stack-detector) through the connector tool `nerolabs--tech-stack-detector`.

## Use it for requests like

- "Which of these websites run on Shopify?"
- "What CMS and analytics does each site in this sheet use?"
- "Find the companies using HubSpot in this lead list"
- "Do these businesses use Google Workspace or Microsoft 365 for email?"

## What to pass

- `websiteField`: the website column, detected automatically if empty.
- `technologies` (exact names like `Shopify`, `WordPress`) and or `categories` (like `Ecommerce`), with `matchMode` `any` or `all`.
- `keep`: `all`, `matching`, `detected` or `problems`.
- `checkDns` (on by default) reads MX and TXT records for email hosting.
- `retryBlockedWithProxy`: off by default; retries big brands that block ordinary requests ($0.005 extra each).

## How to run it

1. Call the `nerolabs--tech-stack-detector` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/tech-stack-detector` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.005 per website analysed. 1,000 sites = about $5. Unreachable sites are free.
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

Each row with the technologies found, their categories and confidence, plus summary columns (CMS, ecommerce platform, email host). Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
