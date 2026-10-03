---
name: new-zealand-visa-processing-times
description: Get Immigration New Zealand's official visa processing times (50th and 80th percentile), estimate a decision window for an application date, or watch visas for changes. Use for advisers and applicants asking how long a New Zealand visa (Skilled Migrant, work, partner, visitor and others) takes.
---

# New Zealand Visa Processing Time Monitor

Runs the Nero Labs Apify Actor `nerolabs/nz-visa-processing-monitor` (https://apify.com/nerolabs/nz-visa-processing-monitor) through the connector tool `nerolabs--nz-visa-processing-monitor`.

## Use it for requests like

- "How long is the Skilled Migrant Category Resident Visa taking?"
- "I applied for my NZ partner visa on 2026-01-15, when should I hear?"
- "Watch these NZ visas for processing time changes"

## What to pass

- `visaName` or `visaId` for one; `targets` for many.
- `clients`: rows with visa and application date for decision-window estimates.
- `listAvailableVisas: true` lists every visa (free).
- `monitorMode`, `watchlistId`.

## How to run it

1. Call the `nerolabs--nz-visa-processing-monitor` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/nz-visa-processing-monitor` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.01 per lookup, $0.015 per client estimate.
- Monitor mode: $0.05 per change, $0.002 per visa unchanged.

Before a run likely to cost more than about $1, tell the user the estimate in one line and keep the request small (the number of targets sent) so the bill cannot run away.

## Monitor mode

With `monitorMode: true` the Actor remembers what it saw last time (per `watchlistId`) and returns only what changed. The first monitor run saves the baseline. For ongoing watching, run it again later from Claude, or tell the user they can save the input as an Apify task and schedule it in the Apify Console (for example daily).

## What comes back

Processing times per visa and estimated decision windows for clients. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
