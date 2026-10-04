---
name: analyze-customer-reviews
description: Analyse customer reviews in bulk: sentiment, complaint and praise categories and themes, a verbatim key quote, urgent flags, one written report per business (top complaints and praise with real counts, what is getting better or worse, suggested fixes) and optional ready-to-post owner reply drafts that never promise refunds or admit fault. Use when the user has Google Maps, Trustpilot, Yelp, Amazon, TripAdvisor or app store reviews in a dataset, CSV or Google Sheet and wants to know what customers complain about, compare locations, report to a client, or reply to unanswered reviews.
---

# Review Analyzer: Sentiment, Complaints, Themes & Reply Drafts

Runs the Nero Labs Apify Actor `nerolabs/review-analyzer` (https://apify.com/nerolabs/review-analyzer) through the connector tool `nerolabs--review-analyzer`.

## Use it for requests like

- "What are customers complaining about in these Google reviews?"
- "Make a review report for each of our 12 locations"
- "Draft replies to every unanswered review"
- "Are our reviews getting better or worse?"

## What to pass

- Source: `datasetId` (any reviews scraper's dataset), `fileUrl` (CSV, Excel, JSON or Google Sheet), `reviewTexts` (plain list, with `businessName`) or `data`.
- Review text, stars, date, business and owner reply columns are detected automatically; override with `textField`, `ratingField`, `dateField`, `businessField` only if needed.
- `output`: `reviews_and_reports` (default), `reviews_only` or `reports_only`. `minReviewsForReport` (default 5). `themes`: the user's own theme list, optional.
- `replyDrafts: true` adds reply drafts (only for reviews the owner has not answered, unless `replyOnlyUnanswered: false`); set `replySignOff` and `replyContact` from the user's details, never invent them. Tell the user to read each reply before posting.

## How to run it

1. Call the `nerolabs--review-analyzer` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/review-analyzer` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `reviewTexts`: reviews pasted as plain text, with `businessName`.
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.002 per review analysed. 1,000 reviews = about $2. Star-only reviews and failed AI calls are never charged.
- $0.03 per written business report, $0.005 per reply draft (only if switched on).
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

Each review with sentiment, complaintThemes, praiseThemes, keyQuote, needsAttention and optional replyDraft, plus one business_report row per business with avgStars, topComplaints, topPraise, trend and suggestedFixes, and a readable report.html. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
