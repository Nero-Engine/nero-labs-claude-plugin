---
name: uk-company-risk-report
description: Produce a full plain-English risk report on a UK limited company: status, overdue accounts, strike-off warnings, director track record and disqualifications, people with significant control, charges, insolvency history and Gazette notices, as red, amber and green flags with a verdict. Use for credit checks, supplier due diligence, KYB onboarding, or before signing a contract with a UK company.
---

# UK Company Risk Report (Companies House and Gazette)

Runs the Nero Labs Apify Actor `nerolabs/uk-company-risk-report` (https://apify.com/nerolabs/uk-company-risk-report) through the connector tool `nerolabs--uk-company-risk-report`.

## Use it for requests like

- "Do a full risk report on company 00445790"
- "Due diligence on this UK supplier before we sign"
- "Have this company's directors run companies that went bust?"

## What to pass

- `companies` (required): Companies House numbers (exact) or names. A name that matches several companies returns a short candidate list instead (cheap); re-run with the right number.
- `includeGazette`, `checkDirectorHistory`, `checkDisqualifications` (all on by default), `maxOfficerChecks`.
- `includeMarkdown: true` adds the whole report as Markdown, ready to paste.
For a quick yes or no on many businesses, the verify-uk-business skill is cheaper.

## How to run it

1. Call the `nerolabs--uk-company-risk-report` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/uk-company-risk-report` as a connector.
2. Pass the inputs described in "What to pass" above.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.50 per full report. A name that needs choosing between candidates costs $0.01.

Before a run likely to cost more than about $1, tell the user the estimate in one line and keep the request small (the number of companies sent) so the bill cannot run away.

## What comes back

Per company: flags with evidence, a verdict and the underlying Companies House and Gazette facts. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
