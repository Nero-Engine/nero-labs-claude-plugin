---
name: check-pay-transparency-rules
description: Check whether a job post must show the pay range, and exactly what it must include, under US state and city laws, Canadian provinces and the EU Pay Transparency Directive country by country. Use before publishing or writing a job ad, or to audit a batch of postings for compliance.
---

# Pay Transparency Law Checker for Job Posts

Runs the Nero Labs Apify Actor `nerolabs/pay-transparency-rules` (https://apify.com/nerolabs/pay-transparency-rules) through the connector tool `nerolabs--pay-transparency-rules`.

## Use it for requests like

- "Does this job ad for Denver and remote need a salary range?"
- "What must our job posts in Germany say from 2026?"
- "Audit these 200 job posts for pay transparency"

## What to pass

- One post: `locations` (list like ["Denver, CO", "Germany"]), `remote`, `employer_employees`, `date`.
- Many: `lookups`, a JSON list with the same keys.
- `mode: "list"` (filter `country`, `status`) or `mode: "upcoming"` lists the laws.

## How to run it

1. Call the `nerolabs--pay-transparency-rules` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/pay-transparency-rules` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.003 per answered check. 1,000 posts = $3.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxTotalChargeUsd`) so the bill cannot run away.

## What comes back

Which laws apply, a merged checklist of what the post must include, salary history bans, penalties and the official source per law. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
