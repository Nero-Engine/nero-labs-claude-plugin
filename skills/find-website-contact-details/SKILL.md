---
name: find-website-contact-details
description: Find the contact email, phone number and social profiles published on many business websites at once. Use when the user has a list, sheet or scraper dataset of company websites (for example Google Maps results) and wants each one's info@ style inbox, phone in international format, LinkedIn or Instagram links and contact page, kept next to their own columns.
---

# Website Contact Finder: Emails, Phones & Socials

Runs the Nero Labs Apify Actor `nerolabs/website-contact-finder` (https://apify.com/nerolabs/website-contact-finder) through the connector tool `nerolabs--website-contact-finder`.

## Use it for requests like

- "Find the contact email for every website in this sheet"
- "Get phone numbers and socials for these 300 businesses"
- "Add a contact email column to my Google Maps leads"
- "Which of these sites have no contact details at all?"

## What to pass

- `websiteField`: the website column, detected automatically if empty.
- Returns role inboxes (info@, sales@, enquiries@) at the site's own domain by default. `includePersonalEmails` (named people) and `includeNoReply` are OFF by default for data-protection reasons; only turn them on if the user asks and has a lawful reason.
- `extractPhones`, `extractSocials`, `followContactPages` (on by default), `phoneRegion` (e.g. `GB`).
- `respectRobotsTxt` stays on. `useProxy` retries sites that block ordinary requests.
- `keep`: `all`, `with_contacts`, `with_email` or `problems`. `exportFormats` for CSV or Excel.

## How to run it

1. Call the `nerolabs--website-contact-finder` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/website-contact-finder` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.02 per website that returned a usable contact. 500 sites with contacts = about $10.
- $0.002 per website reached that publishes no contact details. Unreachable sites are free.
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

Each row with emails, phones (E.164), social profile links and the contact page URL. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
