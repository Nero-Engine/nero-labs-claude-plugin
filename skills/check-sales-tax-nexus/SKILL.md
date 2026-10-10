---
name: check-sales-tax-nexus
description: Work out which US states an online or remote seller must register in to collect sales tax, from its sales and order counts per state, using each state's economic nexus threshold, measurement period, marketplace rules and what sales count. Use when the user asks "do I need to collect sales tax in X", sells on Shopify, Amazon or Etsy into several states, or wants a nexus review.
---

# US Sales Tax Economic Nexus Checker

Runs the Nero Labs Apify Actor `nerolabs/us-sales-tax-nexus` (https://apify.com/nerolabs/us-sales-tax-nexus) through the connector tool `nerolabs--us-sales-tax-nexus`.

## Use it for requests like

- "I sold $612k to California and $180k to Texas last year, where do I need to register?"
- "Which states' sales tax thresholds am I close to?"
- "What is the economic nexus threshold in New York?"

## What to pass

- `sales_by_state`: list of {"state", "sales", "transactions"}, optionally `marketplace_sales`, `retail_sales`, `taxable_sales` per state.
- `period` (last_12_months, previous_calendar_year, current_calendar_year), `includes_marketplace_sales`, `date`.
- `mode: "list"` (optional `state`) or `mode: "upcoming"` lists thresholds and changes.

## How to run it

1. Call the `nerolabs--us-sales-tax-nexus` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/us-sales-tax-nexus` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.003 per answered check (one seller, all states). Listing = $0.003.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxTotalChargeUsd`) so the bill cannot run away.

## What comes back

States where the seller must register, states close to the threshold, the rule and official source for each, and what the answer depends on. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
