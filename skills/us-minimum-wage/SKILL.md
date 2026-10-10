---
name: us-minimum-wage
description: Find the legal hourly minimum wage and tipped cash wage at any US address, city, county or state on any date, including city rates, industry rates and the next scheduled change, with the government source. Use when the user asks "what is the minimum wage in X", is setting pay or checking payroll, or has a list of work locations to check.
---

# US Minimum Wage by Address and Date

Runs the Nero Labs Apify Actor `nerolabs/us-minimum-wage` (https://apify.com/nerolabs/us-minimum-wage) through the connector tool `nerolabs--us-minimum-wage`.

## Use it for requests like

- "What's the minimum wage in Seattle from January?"
- "Check these 40 store addresses against local minimum wage"
- "What can I pay a tipped server in Florida?"

## What to pass

- One place: `address` (best, resolves the exact city and county) or `state` plus `city` and/or `county`; `date` (default today); optional `employer_size`, `industry`, `annual_revenue`.
- Many: `lookups`, a JSON list of items with the same keys.
- `mode: "upcoming"` with `state`, `from_date`, `days` lists scheduled rate changes.

## How to run it

1. Call the `nerolabs--us-minimum-wage` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/us-minimum-wage` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.003 per answered lookup. 1,000 addresses = $3. Failed lookups are free.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxTotalChargeUsd`) so the bill cannot run away.

## What comes back

The hourly rate that applies, which jurisdiction sets it, the tipped cash wage, the federal, state, county and city breakdown, upcoming changes and the source per figure. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
