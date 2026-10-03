---
name: group-by-and-pivot
description: Summarise a CSV, Excel file, Google Sheet, JSON array or Apify dataset with GROUP BY and pivot tables: counts, sums, averages, min, max and median per group, by day, week, month, quarter or year. Use when the user asks how many rows per category, total sales by region, average rating per city, monthly counts, a pivot table, or the top 10 groups by a number.
---

# Dataset Aggregate, Group By & Pivot

Runs the Nero Labs Apify Actor `nerolabs/dataset-aggregate-pivot` (https://apify.com/nerolabs/dataset-aggregate-pivot) through the connector tool `nerolabs--dataset-aggregate-pivot`.

## Use it for requests like

- "How many businesses per city in this dataset?"
- "Total revenue by month and region as a pivot table"
- "Average rating per category, top 10"
- "Count of leads per source per week"

## What to pass

- `groupByFields`: e.g. `["city"]`. Empty aggregates the whole dataset into one row.
- `aggregations`: list of `{"field": "amount", "function": "sum", "alias": "total"}`. Functions: count, countDistinct, sum, avg, min, max, median, first, last, list, listDistinct.
- `dateBucketField` plus `dateBucketGranularity` (`day`, `week`, `month`, `quarter`, `year`) groups by time period.
- `pivotField`, `pivotValueField`, `pivotFunction` turn one field's values into columns.
- `lenientNumbers: true` for scraped prices. `sortBy`, `sortDirection`, `topN`, `includeTotalsRow`.

## How to run it

1. Call the `nerolabs--dataset-aggregate-pivot` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/dataset-aggregate-pivot` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.001 per input row read (not per group). A 5,000-row dataset costs about $5 whatever the grouping.
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

One row per group (or the pivot table), sorted, with an optional grand total row. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
