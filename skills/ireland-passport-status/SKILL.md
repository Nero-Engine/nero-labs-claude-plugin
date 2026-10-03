---
name: ireland-passport-status
description: Check the status of Irish passport applications on the official DFA passport tracker (stage and target issue date), or watch them and report only changes. Use when someone asks where their Irish passport application is, or a travel agent or family wants alerts as applications move stages.
---

# Ireland Passport Application Status Tracker

Runs the Nero Labs Apify Actor `nerolabs/ireland-passport-tracker` (https://apify.com/nerolabs/ireland-passport-tracker) through the connector tool `nerolabs--ireland-passport-tracker`.

## Use it for requests like

- "Check my Irish passport application 12345678901"
- "Tell me when these applications change stage"

## What to pass

- `applicationId` (one) or `applicationIds` (many; needed for monitor mode), the 11-digit number from the form or An Post receipt.
- `monitorMode`, `watchlistId`.

## How to run it

1. Call the `nerolabs--ireland-passport-tracker` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/ireland-passport-tracker` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.01 per application checked (not-found is charged too).
- Monitor mode: $0.05 per change, $0.002 per application unchanged.

Before a run likely to cost more than about $1, tell the user the estimate in one line and keep the request small (the number of applications sent) so the bill cannot run away.

## Monitor mode

With `monitorMode: true` the Actor remembers what it saw last time (per `watchlistId`) and returns only what changed. The first monitor run saves the baseline. For ongoing watching, run it again later from Claude, or tell the user they can save the input as an Apify task and schedule it in the Apify Console (for example daily).

## What comes back

Found or not, current stage and target issue date, and in monitor mode only changes. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
