---
name: breach-notification-deadlines
description: After a data breach or security incident, list every notice the company owes and the deadline date for each: every US state (people affected, attorney general, credit bureaus), HIPAA, SEC Form 8-K, FTC rules, GDPR 72 hours, UK ICO, Canada, Australia and more. Use for incident response, breach checklists and privacy compliance questions.
---

# Data Breach Notification Deadline Calculator

Runs the Nero Labs Apify Actor `nerolabs/breach-notification-deadlines` (https://apify.com/nerolabs/breach-notification-deadlines) through the connector tool `nerolabs--breach-notification-deadlines`.

## Use it for requests like

- "We had a breach affecting 1,200 Californians and 40 Germans, who do we tell and by when?"
- "What's the attorney general notice threshold in Texas?"
- "We're listed on Nasdaq and found a ransomware incident on Monday, what are the deadlines?"

## What to pass

- `affected`: list of {"state", "count"} or {"country", "count"} (a bare two-letter code is a US state).
- `data_types` (ssn, drivers_license, payment_card, medical, username_password...), `encrypted`, `date_discovered`, `sec_registrant`, `materiality_date`, `sector` (hipaa_covered_entity, glba_financial...), `role`.
- `mode: "list"` (filter `state`, `country`, `scope`) lists the laws.

## How to run it

1. Call the `nerolabs--breach-notification-deadlines` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/breach-notification-deadlines` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.003 per answered check. Listing = $0.003.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxTotalChargeUsd`) so the bill cannot run away.

## What comes back

Every notice owed, earliest deadline first, with the date, who to notify, what to include, how to send it and the source per law; plus notices not required and why. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
