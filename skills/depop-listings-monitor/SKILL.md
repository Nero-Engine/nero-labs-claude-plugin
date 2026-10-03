---
name: depop-listings-monitor
description: Search Depop listings by keyword or check specific listings (price, sold or available), or watch a search for new listings and items for price drops or sales. Use for resellers and vintage buyers who want alerts on new Depop listings for a search like "vintage carhartt jacket".
---

# Depop Scraper & Monitor: New Listings, Price & Sold Alerts

Runs the Nero Labs Apify Actor `nerolabs/depop-marketplace-monitor` (https://apify.com/nerolabs/depop-marketplace-monitor) through the connector tool `nerolabs--depop-marketplace-monitor`.

## Use it for requests like

- "Show me Depop listings for vintage Nike windbreakers"
- "Alert me when new listings appear for this search"
- "Has this Depop item sold or dropped in price?"

## What to pass

- `searchQueries` and or `productUrls`.
- `maxItemsPerSearch` (about 24 per page), `proxyCountryCode` (default `US`).
- `monitorMode`, `watchlistId`.

## How to run it

1. Call the `nerolabs--depop-marketplace-monitor` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/depop-marketplace-monitor` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.015 per listing checked. Blocked checks are free.
- Monitor mode: $0.06 per change found, $0.004 per check with no change.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItemsPerSearch`) so the bill cannot run away.

## Monitor mode

With `monitorMode: true` the Actor remembers what it saw last time (per `watchlistId`) and returns only what changed. The first monitor run saves the baseline. For ongoing watching, run it again later from Claude, or tell the user they can save the input as an Apify task and schedule it in the Apify Console (for example daily).

## What comes back

Listings with title, price, sold status and link, and in monitor mode only new listings or changes. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
