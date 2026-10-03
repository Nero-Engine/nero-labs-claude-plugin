---
name: ffxiv-market-prices
description: Check Final Fantasy XIV market board prices for items on a world, data centre or region (cheapest listing, sale velocity, recent sales), or watch items for price drops. Use for FFXIV players and gil traders asking what an item sells for, where it is cheapest, or to be alerted when it gets cheap.
---

# FFXIV Market Board Price Checker & Deal Monitor

Runs the Nero Labs Apify Actor `nerolabs/ffxiv-market-monitor` (https://apify.com/nerolabs/ffxiv-market-monitor) through the connector tool `nerolabs--ffxiv-market-monitor`.

## Use it for requests like

- "What's the cheapest price for item 5333 on Aether?"
- "Alert me when these crafting mats drop 20% on Chaos"
- "How fast does this item sell on Excalibur?"

## What to pass

- `itemIds` (required, from universalis.app) and `worldDcRegion` (required: a world, data centre or region).
- `listingsPerItem`, `includeHistory`, `historyEntriesPerItem`.
- Monitor mode: `monitorMode`, `watchlistId`, `priceDropAlertPercent`, `targetPriceGil`.

## How to run it

1. Call the `nerolabs--ffxiv-market-monitor` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/ffxiv-market-monitor` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.01 per item checked.
- Monitor mode: $0.05 per price alert, $0.002 per item with no alert.

Before a run likely to cost more than about $1, tell the user the estimate in one line and keep the request small (the number of items sent) so the bill cannot run away.

## Monitor mode

With `monitorMode: true` the Actor remembers what it saw last time (per `watchlistId`) and returns only what changed. The first monitor run saves the baseline. For ongoing watching, run it again later from Claude, or tell the user they can save the input as an Apify task and schedule it in the Apify Console (for example daily).

## What comes back

Cheapest listing, average price, sale velocity and recent sales per item, or price-drop alerts. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
