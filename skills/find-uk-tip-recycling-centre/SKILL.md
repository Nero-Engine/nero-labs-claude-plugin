---
name: find-uk-tip-recycling-centre
description: Find the nearest UK household waste recycling centres (tips) to a postcode, with distance, opening hours, accepted materials and trade-waste rules. Use when someone asks where to take rubbish, garden waste, electricals, batteries or a mattress, or which tip near them accepts trade waste from a van.
---

# UK Waste & Tip Finder

Runs the Nero Labs Apify Actor `nerolabs/uk-waste-tip-finder` (https://apify.com/nerolabs/uk-waste-tip-finder) through the connector tool `nerolabs--uk-waste-tip-finder`.

## Use it for requests like

- "Nearest tip to SW1A 1AA that takes garden waste"
- "Where can I dump an old fridge near Leeds LS1?"
- "Which recycling centres near me accept trade waste?"

## What to pass

- `postcode` (required, full or partial).
- `materials`: what they need to get rid of. `audience`: `resident` or `trade_commercial`.
- `radiusKm`, `maxResults`.

## How to run it

1. Call the `nerolabs--uk-waste-tip-finder` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/uk-waste-tip-finder` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.02 per site returned. 5 nearest tips = $0.10. Nothing found costs nothing.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxResults`) so the bill cannot run away.

## What comes back

Nearest sites first, with distance, opening hours, accepted materials and trade-waste warnings. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
