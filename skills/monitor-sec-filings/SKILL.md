---
name: monitor-sec-filings
description: Get only the new SEC EDGAR filings for watched US public companies since the last check: 8-K material events, Form 4 insider trades, 13D and 13G large stakeholder filings, or any other form type. Use when the user wants alerts on insider buying or selling, material news filings, or activist stakes for a list of tickers.
---

# SEC Filings Monitor: EDGAR 8-K, Form 4 Insider & 13D Alerts

Runs the Nero Labs Apify Actor `nerolabs/sec-edgar-filing-monitor` (https://apify.com/nerolabs/sec-edgar-filing-monitor) through the connector tool `nerolabs--sec-edgar-filing-monitor`.

## Use it for requests like

- "Alert me to new 8-K and Form 4 filings for AAPL, TSLA and NVDA"
- "Any new insider trades at these companies since yesterday?"
- "Watch these tickers for 13D filings"

## What to pass

- `companies` (required): tickers or CIK numbers.
- `formTypes`: defaults to 8-K, Form 4, SC 13D and SC 13G.
- `maxFilingsPerCompanyOnFirstRun`: how many recent filings the first run reports.
Every run remembers what it already reported, so running it again (or on an Apify schedule) returns only new filings.

## How to run it

1. Call the `nerolabs--sec-edgar-filing-monitor` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/sec-edgar-filing-monitor` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.10 per new filing returned. Runs with nothing new cost almost nothing.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxFilingsPerCompanyOnFirstRun`) so the bill cannot run away.

## What comes back

Each new filing with company, form type, date, document link and a note on why it may matter. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
