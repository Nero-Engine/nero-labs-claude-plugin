---
name: monitor-uk-companies-house
description: Look up UK companies on Companies House or watch them for changes: new filings, director appointments and resignations, changes to people with significant control (PSC), and status changes such as strike-off or liquidation. Use when the user wants alerts when a client, competitor or supplier files something, or a quick status lookup by company number.
---

# UK Companies House Monitor: Filings, Directors & PSC Alerts

Runs the Nero Labs Apify Actor `nerolabs/uk-companies-house-monitor` (https://apify.com/nerolabs/uk-companies-house-monitor) through the connector tool `nerolabs--uk-companies-house-monitor`.

## Use it for requests like

- "Tell me when any of these 20 companies file something new"
- "Has this company changed directors?"
- "Watch my clients for strike-off notices"

## What to pass

- `companyNumber` (one) or `companyNumbers` (many; required for monitor mode). Exact 8-character numbers like `03977902`.
- `monitorMode`, `watchlistId`.

## How to run it

1. Call the `nerolabs--uk-companies-house-monitor` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/uk-companies-house-monitor` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.01 per company looked up.
- Monitor mode: $0.05 per company with a change, $0.002 per company confirmed unchanged. Watching 50 companies daily with no changes = about $3 a month.

Before a run likely to cost more than about $1, tell the user the estimate in one line and keep the request small (the number of companies sent) so the bill cannot run away.

## Monitor mode

With `monitorMode: true` the Actor remembers what it saw last time (per `watchlistId`) and returns only what changed. The first monitor run saves the baseline. For ongoing watching, run it again later from Claude, or tell the user they can save the input as an Apify task and schedule it in the Apify Console (for example daily).

## What comes back

Company name, status and link, and in monitor mode only the new filings, officer, PSC or status changes. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
