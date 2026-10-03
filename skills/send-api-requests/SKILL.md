---
name: send-api-requests
description: Send one HTTP request, or one request per row of a CSV, Excel file, Google Sheet, JSON array or Apify dataset, to any REST API, and get every status code and response back. Use when the user asks to push rows into a CRM or app (HubSpot, Airtable, Notion, GoHighLevel, Stripe, their own API), POST each lead to a webhook, call an API once per row with a templated URL or body, or test an endpoint from the cloud.
---

# HTTP Request Sender: Single or Bulk API Calls

Runs the Nero Labs Apify Actor `nerolabs/dataset-to-rest-api` (https://apify.com/nerolabs/dataset-to-rest-api) through the connector tool `nerolabs--dataset-to-rest-api`.

## Use it for requests like

- "Send every row of this sheet to my webhook"
- "Create a HubSpot contact for each lead in this CSV"
- "Call this API once per ID and give me the responses"
- "PATCH each record with its new status"

## What to pass

- `url` (required): the endpoint. Placeholders work: `https://api.example.com/contacts/{{id}}`.
- Source: `datasetId`, `fileUrl` or `data`. Give none of them, plus `body`, to send ONE request.
- `method`: POST, PUT, PATCH, GET or DELETE. `bodyMode`: `row` (send the row as is), `template` (use `bodyTemplate` with `{{field}}` placeholders) or `none`.
- `headers`, `queryParams` (placeholders work).
- `authType` (`bearer`, `basic`, `apiKeyHeader`, `apiKeyQuery`) with `authSecret`. Never paste a user's key into chat output.
- `rateLimitPerSecond`, `retries`, `batchSize`, `stopOnFirstError`.
- **Start with `dryRun: true`** on any live API: it shows the first three requests exactly as they would go out and charges nothing. Run for real only after the user confirms, because the requests change data in their system.

## How to run it

1. Call the `nerolabs--dataset-to-rest-api` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/dataset-to-rest-api` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.005 per request that got a reply from the API (any status code). Requests that never got a reply are free. 1,000 rows = about $5.
- $0.02 per webhook delivery of the summary.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

One result row per request: status code, success flag and the parsed response (trimmed by `responseSnippetChars`). Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
