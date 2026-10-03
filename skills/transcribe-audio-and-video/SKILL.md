---
name: transcribe-audio-and-video
description: Transcribe many audio or video files to text at once, in about 60 languages, with optional speaker labels, timestamps and SRT subtitles. Use when the user has a list, sheet or dataset of recording links (calls, voicemails, podcasts, interviews, meetings, YouTube-style video files, Google Drive or Dropbox links) and wants a transcript for each kept next to their own columns.
---

# Bulk Transcription: Audio & Video to Text

Runs the Nero Labs Apify Actor `nerolabs/bulk-transcription` (https://apify.com/nerolabs/bulk-transcription) through the connector tool `nerolabs--bulk-transcription`.

## Use it for requests like

- "Transcribe every recording in this sheet"
- "Turn these podcast episodes into text"
- "Transcripts with speaker labels and SRT subtitles for these videos"
- "Transcribe these Spanish voicemails"

## What to pass

- Source: `datasetId`, `fileUrl`, `mediaUrls` (a plain list of direct links; Google Drive and Dropbox share links work) or `data`.
- `mode`: `text` (cheapest, one block of text) or `detailed` (Speaker A and B, timestamps, SRT with `includeSrt`, turns with `includeSegments`).
- `language`: two-letter code, or empty to detect.
- `vocabulary`: names and jargon to spell right (text mode).
- `maxMinutesPerFile`: cost ceiling per file.
To grade sales or receptionist calls, use the score-call-recordings skill instead.

## How to run it

1. Call the `nerolabs--bulk-transcription` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/bulk-transcription` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `mediaUrls`: a plain list of audio or video links.
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.01 per started minute of audio in text mode, $0.02 per minute with speakers and timestamps. 10 hours of audio in text mode = about $6. Silent files and failed links are free.
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems and maxMinutesPerFile`) so the bill cannot run away.

## What comes back

Each row with its transcript, detected language and duration, plus segments or SRT in detailed mode. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
