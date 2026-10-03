---
name: chain-apify-actors
description: Run several Apify Actors one after another in a single call, passing each step's output dataset to the next. Use when the user wants a multi-step data job done end to end, for example "scrape Google Maps, then find each business's email, then clean the list", or wants to run one fixed chain repeatedly or on a schedule.
---

# Actor Pipeline Runner: Chain Apify Actors in One Run

Runs the Nero Labs Apify Actor `nerolabs/actor-pipeline-runner` (https://apify.com/nerolabs/actor-pipeline-runner) through the connector tool `nerolabs--actor-pipeline-runner`.

## Use it for requests like

- "Scrape plumbers in Leeds from Google Maps, then find their emails, then dedupe"
- "Run these three Actors in a row on this dataset"
- "Chain my scraper into the cleaner and export a CSV"

## What to pass

- `steps` (required): in order, each `{"actor": "username/actor-name", "input": {...}}`. Each step receives the previous step's dataset; `datasetField` sets which input field gets it when the next Actor does not use `datasetId`.
- `initialDatasetId`: feed an existing dataset to the first step.
- `stopOnFailure` (on by default), `defaultWaitSecs`.
- **Run first with `dryRun: true`**: it validates the chain and shows each step's exact input, starting and charging nothing.
Each step is a normal Actor run billed at that Actor's own price, on top of the pipeline fee.

## How to run it

1. Call the `nerolabs--actor-pipeline-runner` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/actor-pipeline-runner` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.05 per pipeline run plus $0.01 per step started. A 3-step pipeline = $0.08 plus each Actor's own charges. Dry runs are free.

Before a run likely to cost more than about $1, tell the user the estimate in one line and keep the request small (the steps' own limits) so the bill cannot run away.

## What comes back

Every step's run ID, status, dataset and row count, plus the final dataset. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
