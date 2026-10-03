---
name: uk-self-storage-prices
description: Get current weekly prices and offers for UK Big Yellow and Shurgard self-storage locations, or watch them for price and offer changes. Use for storage operators tracking competitors' prices or customers comparing UK storage prices.
---

# UK Self-Storage Price & Offer Monitor

Runs the Nero Labs Apify Actor `nerolabs/uk-storage-price-monitor` (https://apify.com/nerolabs/uk-storage-price-monitor) through the connector tool `nerolabs--uk-storage-price-monitor`.

## Use it for requests like

- "What does Big Yellow charge at this location?"
- "Watch these 5 competitor storage sites for price changes"
- "Any new offers at Shurgard near me?"

## What to pass

- `storeUrl` (one location page) or `storeUrls` (many; needed for monitor mode). Big Yellow and Shurgard pages only.
- `monitorMode`, `watchlistId`.

## How to run it

1. Call the `nerolabs--uk-storage-price-monitor` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/uk-storage-price-monitor` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.10 per location checked.
- Monitor mode: $0.35 per change, $0.10 per location unchanged.

Before a run likely to cost more than about $1, tell the user the estimate in one line and keep the request small (the number of locations sent) so the bill cannot run away.

## Monitor mode

With `monitorMode: true` the Actor remembers what it saw last time (per `watchlistId`) and returns only what changed. The first monitor run saves the baseline. For ongoing watching, run it again later from Claude, or tell the user they can save the input as an Apify task and schedule it in the Apify Console (for example daily).

## What comes back

Headline weekly price, unit sizes (Shurgard) and current offers per location. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
