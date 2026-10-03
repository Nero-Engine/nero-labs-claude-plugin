---
name: find-new-or-changed-rows
description: Compare two versions of a list (CSV, Excel, Google Sheet, JSON or Apify dataset) and return only what was added, removed or changed, or only rows never seen before. Use when the user asks what is new since the last scrape, which prices changed, which listings disappeared, to watch a Google Sheet or a scraper for new items on a schedule, or to stop getting duplicates from a repeated scraper run.
---

# Dataset Diff & Change Detector: Only New Items

Runs the Nero Labs Apify Actor `nerolabs/dataset-diff-detector` (https://apify.com/nerolabs/dataset-diff-detector) through the connector tool `nerolabs--dataset-diff-detector`.

## Use it for requests like

- "What's new in today's scrape compared with yesterday's?"
- "Which prices changed between these two files?"
- "Only give me listings I have never seen before"
- "Watch this Google Sheet and tell me what changed"

## What to pass

Two ways to use it:
- **Compare two snapshots:** `oldDatasetId` / `oldFileUrl` / `oldData` and `newDatasetId` / `newFileUrl` / `newData`.
- **Compare with its own last run:** set `snapshotName` (e.g. `"shop-prices"`) and give only the new data. The first run saves a baseline. Add `onlyNewItems: true` to get only rows never seen on ANY earlier run.
Always set `keyFields` (e.g. `["url"]` or `["sku"]`) so a changed row is not mistaken for a removed one plus an added one. Use `ignoreFields` for timestamps like `scrapedAt`, or `compareFields` to watch only some fields (e.g. `["price"]`).

## How to run it

1. Call the `nerolabs--dataset-diff-detector` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/dataset-diff-detector` as a connector.
2. Give each side ONE source, using the parameter names in "What to pass": an Apify dataset ID, a public CSV, Excel, JSON or Google Sheets link (shared as "Anyone with the link can view"), or inline JSON rows. Use inline rows when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.005 per difference found (added, removed or changed row). Unchanged rows are free unless `includeUnchanged` is on ($0.0005 each).
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

Rows tagged added, removed or changed, with the before and after values of each changed field. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
