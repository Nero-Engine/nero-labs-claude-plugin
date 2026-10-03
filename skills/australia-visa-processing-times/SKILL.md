---
name: australia-visa-processing-times
description: Get Australia's official visa processing times from Home Affairs per subclass and stream (25th, 50th, 75th and 90th percentiles), estimate where an application with a given lodgement date stands, or watch subclasses for changes. Use for migration agents and applicants asking how long an Australian visa (189, 190, 186, 482, 820 and others) takes.
---

# Australia Visa Processing Time Monitor

Runs the Nero Labs Apify Actor `nerolabs/au-visa-processing-monitor` (https://apify.com/nerolabs/au-visa-processing-monitor) through the connector tool `nerolabs--au-visa-processing-monitor`.

## Use it for requests like

- "How long is the 189 visa taking right now?"
- "I lodged my 820 on 2025-11-03, where does that put me?"
- "Alert me when 186 processing times change"

## What to pass

- `visaSubclassCode` plus optional `streamCode` for one; `targets` (`189`, `186:3`) for many.
- `clients`: rows with subclass, stream and application date for caseload estimates.
- `listAvailableVisas: true` lists every subclass and stream (free).
- `monitorMode`, `watchlistId`.

## How to run it

1. Call the `nerolabs--au-visa-processing-monitor` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/au-visa-processing-monitor` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.01 per lookup, $0.015 per client estimate.
- Monitor mode: $0.05 per change, $0.002 per subclass unchanged.

Before a run likely to cost more than about $1, tell the user the estimate in one line and keep the request small (the number of targets sent) so the bill cannot run away.

## Monitor mode

With `monitorMode: true` the Actor remembers what it saw last time (per `watchlistId`) and returns only what changed. The first monitor run saves the baseline. For ongoing watching, run it again later from Claude, or tell the user they can save the input as an Apify task and schedule it in the Apify Console (for example daily).

## What comes back

Percentile processing times per subclass and stream, and client position estimates. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
