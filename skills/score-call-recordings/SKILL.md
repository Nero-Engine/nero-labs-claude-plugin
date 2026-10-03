---
name: score-call-recordings
description: Review and score phone call recordings with AI: outcome, booked or not, lead quality, questions the caller asked that were missed, a 0 to 10 score, the next step and a speaker transcript. Use when the user asks to QA receptionist, sales, support or AI voice agent calls, audit missed bookings, score a call log from GoHighLevel, Vapi, Retell, Twilio, CallRail or Aircall, or answer custom questions about every call.
---

# Call Score: AI Call Review & QA

Runs the Nero Labs Apify Actor `nerolabs/call-score` (https://apify.com/nerolabs/call-score) through the connector tool `nerolabs--call-score`.

## Use it for requests like

- "Score last week's receptionist calls"
- "Which of these calls should have been a booking?"
- "QA my AI voice agent's calls"
- "For every call: did they mention the free quote?"

## What to pass

- Source: `datasetId`, `fileUrl` (a call log with a recording link column), `recordingUrls` (a plain list) or `data`.
- `businessContext` (strongly recommended): a few lines on the business, what it sells, area, and what a good call looks like. Scores are much sharper with it. Ask the user for it if they have not said.
- `customQuestions`: up to 10 extra questions answered for every call.
- `language`, `includeTranscript` (default on), `maxMinutesPerFile` (cost ceiling).
For plain transcripts without scoring, use the transcribe-audio-and-video skill (cheaper).

## How to run it

1. Call the `nerolabs--call-score` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/call-score` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `recordingUrls`: a plain list of recording links.
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.04 per started minute of call. 100 calls of 3 minutes = about $12. Silent calls and failed links are free.
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems and maxMinutesPerFile`) so the bill cannot run away.

## What comes back

Each call with outcome, booked yes or no, lead quality, missed questions, score, next step and transcript. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
