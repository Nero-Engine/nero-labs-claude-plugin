---
name: push-data-to-database
description: Load a CSV, Excel file, Google Sheet, JSON array or Apify dataset into a Postgres, Supabase, Neon or MySQL table. Use when the user wants to import data into their database, sync scraper results into Supabase, upsert rows by a key so repeat runs update instead of duplicate, or create a table automatically from a spreadsheet.
---

# Dataset to Postgres, Supabase & MySQL

Runs the Nero Labs Apify Actor `nerolabs/dataset-to-database` (https://apify.com/nerolabs/dataset-to-database) through the connector tool `nerolabs--dataset-to-database`.

## Use it for requests like

- "Import this CSV into my Supabase table"
- "Push this scraper dataset into Postgres, upsert on url"
- "Create a leads table in MySQL from this sheet"
- "Sync these rows into Neon every time"

## What to pass

- `databaseType`: `postgres` (Supabase, Neon, RDS, Railway, Render) or `mysql`.
- `connectionString`: the user's own, e.g. `postgres://user:password@host:5432/db`. It is a secret: ask the user to supply it, never repeat it back in the chat, and never store it anywhere else.
- `tableName`, `writeMode` (`append`, `upsert` with `keyFields`, or `replace`), `createTableIfMissing`, `addMissingColumns`, `columnNaming` (`snake_case`), `flattenNested`.
- **Start with `dryRun: true`**: it shows the inferred column types and the SQL and writes nothing. Run for real only after the user confirms, because it changes their database.

## How to run it

1. Call the `nerolabs--dataset-to-database` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/dataset-to-database` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.002 per row written. 10,000 rows = about $20. Dry runs and rolled-back batches are free.
- $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

Rows written, columns added, the table used and any warnings. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
