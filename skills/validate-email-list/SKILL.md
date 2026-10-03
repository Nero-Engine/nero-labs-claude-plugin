---
name: validate-email-list
description: Clean and validate an email list before sending. Use when the user asks to check which emails in a CSV, sheet or dataset are valid, remove bounces before a campaign, flag throwaway or disposable addresses, fix typos like gmial.com, find shared inboxes like info@, or remove duplicate emails. Checks format and whether the domain accepts mail; it does not confirm that each individual mailbox exists.
---

# Bulk Email Validator & List Cleaner

Runs the Nero Labs Apify Actor `nerolabs/email-list-cleaner` (https://apify.com/nerolabs/email-list-cleaner) through the connector tool `nerolabs--email-list-cleaner`.

## Use it for requests like

- "Clean this email list before I send my campaign"
- "Which of these emails will bounce?"
- "Flag disposable and typo emails in this sheet"
- "Remove duplicate emails and give me a clean CSV"

## What to pass

- `emailField`: the email column, detected automatically if empty.
- `checkMailServer` (MX lookup), `detectDisposable`, `detectRole` (info@ and similar marked risky), `detectTypos` (adds a `didYouMean` column), `markDuplicates`: all on by default.
- `keep`: `all`, `valid`, `valid_and_risky` or `problems`. `exportFormats` for a clean CSV or Excel file.
Tell the user plainly that a "valid" verdict means the address is well formed and its domain accepts mail, not that the specific mailbox exists.

## How to run it

1. Call the `nerolabs--email-list-cleaner` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/email-list-cleaner` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.002 per address checked. 10,000 emails = about $20.
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

Each row with a verdict (valid, risky, invalid, duplicate), the reason and any typo suggestion. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
