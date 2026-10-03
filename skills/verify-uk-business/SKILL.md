---
name: verify-uk-business
description: Check whether a UK company, tradesperson or online seller looks legitimate before paying them. Use when the user asks "is this company legit", "is this builder a scam", "can I trust this website", wants due diligence on a UK supplier, or wants a whole list of UK businesses risk-checked. Checks Companies House, The Gazette (insolvency), the UK Sanctions List and, for food businesses, Food Standards Agency hygiene ratings.
---

# UK Business Trust Check: Verify Any UK Company or Trader

Runs the Nero Labs Apify Actor `nerolabs/uk-business-trust-check` (https://apify.com/nerolabs/uk-business-trust-check) through the connector tool `nerolabs--uk-business-trust-check`.

## Use it for requests like

- "Is this roofing company legit? Here's their website"
- "Check this UK supplier before I pay the deposit"
- "Risk-check every business in this Google Maps list"
- "Is company number 12345678 still trading?"

## What to pass

- One business: any of `businessName`, `website`, `companyNumber`, plus `postcode` to pick the right one among similar names.
- Many: `businesses` (a JSON list with the same keys), `datasetId` or `fileUrl`; use `fieldMapping` if the columns have unusual names.
- `includeOfficers` (director cross-checks), `includeGazette`, `checkFoodHygiene`.
Report the verdict with its cited reasons, and say it is an automated check of public records, not a guarantee.

## How to run it

1. Call the `nerolabs--uk-business-trust-check` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/uk-business-trust-check` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.05 per business checked. 100 businesses = $5.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

A verdict (looks_legitimate, caution, high_risk, cannot_verify), a 0 to 100 score and cited reasons per business. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
