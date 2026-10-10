---
name: ai-enrich-rows
description: Run one AI instruction on every row of a spreadsheet, Google Sheet, CSV or Apify dataset and add the answers as new columns. Use when the user asks to classify, tag, categorise, score, summarise, translate, extract fields from or clean up text in hundreds or thousands of rows at once (for example "label each review positive or negative", "pull the city out of each address", "score each lead 1 to 10"). Use it when the data is too big to do in the chat itself.
---

# Dataset AI Enrichment: Bulk LLM Classify and Extract

Runs the Nero Labs Apify Actor `nerolabs/dataset-ai-enrich` (https://apify.com/nerolabs/dataset-ai-enrich) through the connector tool `nerolabs--dataset-ai-enrich`.

## Use it for requests like

- "Classify each of these 2,000 reviews as positive, neutral or negative"
- "Extract the job title and seniority from each LinkedIn headline"
- "Score every lead in this sheet for fit, 1 to 10"
- "Translate the description column into Spanish"

## What to pass

- `prompt` (required): the instruction for each row, with `{{field}}` placeholders, e.g. `Classify the sentiment of: {{review}}`.
- `outputFields`: the new columns as `[{"name": "sentiment", "type": "string", "description": "positive, neutral or negative"}]`. Types include string, number, boolean.
- `systemPrompt`: shared context (categories, language, edge cases).
- `model`: default `anthropic/claude-haiku-4.5`. No API key needed.
- **Run first with `previewRows: 5`**, show the user the result, then run the whole set with `previewRows: 0`.

## How to run it

1. Call the `nerolabs--dataset-ai-enrich` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/dataset-ai-enrich` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.002 per row enriched (10 to 30% less on bigger Apify plans), plus the AI tokens billed through Apify's OpenRouter Actor (about $0.001 per row on Haiku on paid Apify plans; free Apify plans pay about 10x the token rate). 1,000 rows = about $3 on a paid plan.
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxRows`) so the bill cannot run away.

## What comes back

Each original row plus the new AI columns, and an error note on any row the model could not answer. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
