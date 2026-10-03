# Nero Labs Data Tools

Bulk jobs on spreadsheets, Google Sheets and Apify datasets, done from a conversation with Claude. Give Claude a sheet, a CSV link, a list of links or an Apify dataset and ask for the job in plain words. Claude picks the right Nero Labs tool, runs it in the cloud and replies with the result.

## What it does

| Ask Claude to... | Skill | Apify Actor | Price (Free plan rate) |
|---|---|---|---|
| Keep only rows that match a rule, sort, take the top N | `filter-dataset-rows` | [Dataset Filter & Transform](https://apify.com/nerolabs/dataset-filter-transform) | $0.002 per row kept |
| Remove duplicates and clean a messy list | `clean-and-dedupe-data` | [Dataset Cleaner & Exporter](https://apify.com/nerolabs/dataset-cleaner-exporter) | $0.002 per record |
| VLOOKUP or join two lists on a key | `join-two-datasets` | [Dataset Join & Merge](https://apify.com/nerolabs/dataset-join-merge) | $0.002 per joined row |
| Count, sum, average per group, pivot tables | `group-by-and-pivot` | [Dataset Aggregate, Group By & Pivot](https://apify.com/nerolabs/dataset-aggregate-pivot) | $0.001 per input row |
| Show only new or changed rows since last time | `find-new-or-changed-rows` | [Dataset Diff & Change Detector](https://apify.com/nerolabs/dataset-diff-detector) | $0.005 per difference |
| Read text from images and scanned PDFs (OCR) | `ocr-images-and-scanned-pdfs` | [Bulk OCR](https://apify.com/nerolabs/bulk-ocr-image-pdf-to-text) | $0.005 per page |
| Pull text and tables out of PDFs | `extract-text-from-pdfs` | [PDF Extractor](https://apify.com/nerolabs/dataset-pdf-extract) | $0.01 per PDF |
| Screenshot every website in a list | `screenshot-websites` | [Bulk Website Screenshots](https://apify.com/nerolabs/bulk-website-screenshots) | $0.004 per screenshot |
| Send each row to an API or webhook | `send-api-requests` | [HTTP Request Sender](https://apify.com/nerolabs/dataset-to-rest-api) | $0.005 per request |
| Download every image or file in a list | `download-files-from-links` | [Bulk Image & File Downloader](https://apify.com/nerolabs/bulk-file-downloader) | $0.004 per file |
| Transcribe audio and video | `transcribe-audio-and-video` | [Bulk Transcription](https://apify.com/nerolabs/bulk-transcription) | $0.01 per minute |
| Check PageSpeed and Core Web Vitals of many sites | `check-website-speed` | [Bulk PageSpeed & Lighthouse Checker](https://apify.com/nerolabs/bulk-pagespeed-checker) | $0.005 per report |
| Upscale images 2x to 4x with AI | `upscale-images` | [Bulk AI Image Upscaler](https://apify.com/nerolabs/bulk-image-upscaler) | $0.015 per image |
| Score and review phone calls | `score-call-recordings` | [Call Score](https://apify.com/nerolabs/call-score) | $0.04 per call minute |

Each skill tells Claude when to use its tool, what to pass, what it costs and how to read the result. Full pricing, including small per-file export and webhook charges, is in each skill and on each Actor's Apify page.

## Use it

1. Add the plugin, then connect its **Nero Labs** connector and sign in with your Apify account (free to create at apify.com; new accounts include free monthly credit).
2. Ask in plain words, for example:
   - "Filter this Apify dataset to rows with no website"
   - "Screenshot every site in this Google Sheet"
   - "Transcribe the recordings in this CSV with speaker labels"
   - "Which of these 300 sites have a slow mobile PageSpeed score?"
3. Claude runs the tool and answers with the result: a table, counts and download links. Results also stay in your Apify account.

Before a run likely to cost more than about $1, Claude tells you the estimate and sets a cap.

## Pricing and billing

Runs are billed per result to **your own Apify account** at the Actor's pay-per-event price shown above. There is no subscription and no charge from this plugin itself. Failed links, blocked sites and skipped items are not charged. Paid Apify plans get tier discounts.

## Data

The plugin itself stores nothing and contains no code: it is instructions (skills) plus one connector.

- **Connector:** Apify's hosted MCP server at `mcp.apify.com`, limited to the 14 Nero Labs Actors listed above. You sign in to it with Apify OAuth. Claude sends the inputs you give (dataset IDs, file and sheet links, lists of URLs, inline rows) to Apify, which runs the Actor in your Apify account and stores the results in your account's datasets and key-value stores, under your Apify data retention settings.
- **Links you supply:** the Actors download the files, sheets, websites and media you point them at.
- **Outside services used by some Actors:**
  - Bulk Transcription and Call Score send the audio of your recordings to OpenAI's speech and language API (`api.openai.com`) to transcribe and review them.
  - Bulk PageSpeed & Lighthouse Checker sends each page URL to Google's PageSpeed Insights API (`googleapis.com`).
  - HTTP Request Sender sends your rows to the API endpoint you choose, with the credentials you give it.
  - Every other Actor (filter, clean, join, pivot, diff, OCR, PDF, screenshots, downloads, upscaling) does its work inside the Apify run with no other outside service.
- Nero Labs, the publisher, receives the usual Apify developer statistics (run counts and charges), not your data.

## Support

Open an issue on any Actor's **Issues** tab on Apify (for example https://apify.com/nerolabs/dataset-filter-transform/issues), or on this repository.

## About

Built by Nero Labs (Nero Engine Group Ltd, UK). All Actors: https://apify.com/nerolabs. Licence: MIT.
