---
name: poland-company-lookup
description: Look up Polish companies in the official KRS (National Court Register) by KRS number, or watch them for new registry entries and suspension changes. Use for Polish KYB, supplier checks, or alerts on Polish companies.
---

# Poland KRS Company Registry Lookup & Monitor

Runs the Nero Labs Apify Actor `nerolabs/poland-krs-monitor` (https://apify.com/nerolabs/poland-krs-monitor) through the connector tool `nerolabs--poland-krs-monitor`.

## Use it for requests like

- "Look up KRS 0000033014"
- "Who is on the board of this Polish company?"
- "Watch these Polish suppliers for registry changes"

## What to pass

- `krsNumber` (one) or `krsNumbers` (many; needed for monitor mode), exact 10 digits.
- `registry`: `P` for companies (default), `S` for associations and foundations.
- `monitorMode`, `watchlistId`.

## How to run it

1. Call the `nerolabs--poland-krs-monitor` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/poland-krs-monitor` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.01 per record.
- Monitor mode: $0.05 per change, $0.002 per company unchanged.

Before a run likely to cost more than about $1, tell the user the estimate in one line and keep the request small (the number of companies sent) so the bill cannot run away.

## Monitor mode

With `monitorMode: true` the Actor remembers what it saw last time (per `watchlistId`) and returns only what changed. The first monitor run saves the baseline. For ongoing watching, run it again later from Claude, or tell the user they can save the input as an Apify task and schedule it in the Apify Console (for example daily).

## What comes back

Legal form, address, share capital, board and suspension status, and in monitor mode only new entries. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
