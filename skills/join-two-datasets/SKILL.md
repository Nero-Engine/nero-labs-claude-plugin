---
name: join-two-datasets
description: Join or merge two tables (CSV, Excel, Google Sheets, JSON or Apify datasets) on a shared key, like VLOOKUP or a SQL join. Use when the user asks to enrich one list with columns from another, match two sheets on email, SKU, ID or domain, find rows in one list that are missing from the other (anti join), or stack two datasets into one (union).
---

# Dataset Join & Merge (VLOOKUP for Datasets)

Runs the Nero Labs Apify Actor `nerolabs/dataset-join-merge` (https://apify.com/nerolabs/dataset-join-merge) through the connector tool `nerolabs--dataset-join-merge`.

## Use it for requests like

- "VLOOKUP these two sheets on email"
- "Add the phone numbers from list B to list A"
- "Which customers in this CSV are not in my CRM export?"
- "Combine these two scraper runs into one table"

## What to pass

Each side takes its own source: `leftDatasetId` / `leftFileUrl` / `leftData` and `rightDatasetId` / `rightFileUrl` / `rightData`. The left side is the main table, the right side is the lookup table.
- `leftKeyFields` (required except for union) and `rightKeyFields` (only if the right side names the key differently), e.g. `["email"]`.
- `joinType`: `left` (VLOOKUP, keep every left row), `inner` (matches only), `right`, `full`, `leftAnti` (left rows with NO match), `rightAnti`, `union`.
- `keyMatching`: `normalized` (default, case and spacing ignored) or `exact`.
- `rightFields`: only bring in these columns. `onFieldConflict`: `prefixRight`, `keepLeft` or `keepRight`.
- `multipleMatches`: `all` (SQL style) or `first` (VLOOKUP style).
- `includeJoinInfo: true` adds `_joinStatus` and `_matchCount` to each row.

## How to run it

1. Call the `nerolabs--dataset-join-merge` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/dataset-join-merge` as a connector.
2. Give each side ONE source, using the parameter names in "What to pass": an Apify dataset ID, a public CSV, Excel, JSON or Google Sheets link (shared as "Anyone with the link can view"), or inline JSON rows. Use inline rows when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.002 per matched (joined) row, $0.001 per one-sided row (unmatched rows kept, anti joins, union rows).
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

The joined rows plus match rates for both sides. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
