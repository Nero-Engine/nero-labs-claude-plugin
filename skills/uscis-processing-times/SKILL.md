---
name: uscis-processing-times
description: Get official USCIS processing times by form, category and office, and USCIS's own answer on whether a case is outside normal processing time so the applicant can file a case inquiry; or watch combinations for changes. Use when someone asks how long an I-130, I-485, I-765, N-400 or other USCIS form takes, or whether they can file an inquiry for their receipt date.
---

# USCIS Processing Time Monitor

Runs the Nero Labs Apify Actor `nerolabs/uscis-processing-time-monitor` (https://apify.com/nerolabs/uscis-processing-time-monitor) through the connector tool `nerolabs--uscis-processing-time-monitor`.

## Use it for requests like

- "How long is the I-485 taking at the National Benefits Center?"
- "My N-400 receipt date is 2026-02-10, can I file an inquiry yet?"
- "Watch the I-130 processing time for changes"

## What to pass

- Run once with `listAvailableOptions: true` if you do not know the codes (lists forms, categories and offices).
- `formName` (e.g. `I-130`), `formCategory` (e.g. `134A-IR`), `officeCode` (e.g. `NBC`), optional `receiptDate` (`YYYY-MM-DD`) for the inquiry answer.
- Many at once: `targets` list. `monitorMode`, `watchlistId`.
This source is sometimes slow or under maintenance; if a run fails, say so and suggest retrying later.

## How to run it

1. Call the `nerolabs--uscis-processing-time-monitor` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/uscis-processing-time-monitor` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.02 per lookup, $0.03 with a receipt date, $0.01 per run start.
- Monitor mode: $0.05 per change, $0.006 per combination unchanged.

Before a run likely to cost more than about $1, tell the user the estimate in one line and keep the request small (the number of targets sent) so the bill cannot run away.

## Monitor mode

With `monitorMode: true` the Actor remembers what it saw last time (per `watchlistId`) and returns only what changed. The first monitor run saves the baseline. For ongoing watching, run it again later from Claude, or tell the user they can save the input as an Apify task and schedule it in the Apify Console (for example daily).

## What comes back

Processing-time range per form, category and office, and the inquiry-eligibility answer when a receipt date is given. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
