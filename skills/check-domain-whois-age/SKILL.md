---
name: check-domain-whois-age
description: Look up registration date, expiry date, domain age, registrar and nameservers for many domains at once from the official registries (RDAP, the modern WHOIS). Use when the user asks how old a website or domain is, when domains expire, which domains in a list are unregistered or newly registered (a common scam or new-business signal), or to bulk-check WHOIS for a sheet or dataset.
---

# Bulk WHOIS Lookup & Domain Age Checker

Runs the Nero Labs Apify Actor `nerolabs/domain-whois-checker` (https://apify.com/nerolabs/domain-whois-checker) through the connector tool `nerolabs--domain-whois-checker`.

## Use it for requests like

- "How old are these domains?"
- "Which of my domains expire in the next 60 days?"
- "Flag websites registered in the last 6 months"
- "Are any of these domain names still available?"

## What to pass

- `domainField`: the domain or website column, detected automatically if empty; full URLs work.
- `keep`: `all`, `registered`, `unregistered`, `expiring`, `new` or `problems`.
- `expiringWithinDays` (default sets the expiringSoon flag), `newerThanDays` for the new-domain flag.
- `includeRegistrantOrganization`: off by default; most registries redact owner details anyway.

## How to run it

1. Call the `nerolabs--domain-whois-checker` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/domain-whois-checker` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.005 per domain the registry answered. 1,000 domains = about $5. Registries with no RDAP server are free.
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

Each row with created date, expiry date, age in days, registrar, nameservers and status flags. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
