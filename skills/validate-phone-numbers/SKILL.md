---
name: validate-phone-numbers
description: Validate and format many phone numbers at once. Use when the user asks which numbers in a CSV, sheet or dataset are real, wants them in international E.164 format for a dialler or SMS tool, needs mobile versus landline, country, region, original carrier or timezone, or wants premium-rate, VoIP and duplicate numbers flagged.
---

# Bulk Phone Number Validator & Cleaner

Runs the Nero Labs Apify Actor `nerolabs/phone-number-validator` (https://apify.com/nerolabs/phone-number-validator) through the connector tool `nerolabs--phone-number-validator`.

## Use it for requests like

- "Check which of these phone numbers are valid"
- "Format these numbers as +44 for my dialler"
- "Which numbers are mobiles? I want to text them"
- "Add the timezone so we call in business hours"

## What to pass

- `phoneField`: the phone column, detected automatically if empty.
- `defaultRegion`: two-letter country for numbers without a country code (`GB`, `US`, `AU`).
- `lookupRegion`, `lookupCarrier` (the network the range was allocated to, not the current one), `lookupTimezone`.
- `flagPremiumRate`, `flagVoip`, `flagNonPersonal`, `markDuplicates`.
- `keep`: `all`, `valid`, `valid_and_risky`, `problems` or `mobile`.

## How to run it

1. Call the `nerolabs--phone-number-validator` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/phone-number-validator` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.002 per number checked. 10,000 numbers = about $20.
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

Each row with a verdict, E.164 format, country, line type (mobile or landline), region, carrier and timezone. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
