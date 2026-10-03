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
| Find contact emails, phones and socials on websites | `find-website-contact-details` | [Website Email Extractor, Phone & Contact Finder (CSV, Sheet)](https://apify.com/nerolabs/website-contact-finder) | $0.02 per website contact |
| Validate an email list before sending | `validate-email-list` | [Bulk Email Validator & List Cleaner: MX, Disposable & Typos](https://apify.com/nerolabs/email-list-cleaner) | $0.002 per address |
| Validate and format phone numbers | `validate-phone-numbers` | [Bulk Phone Number Validator & Cleaner: Carrier, CSV or Sheet](https://apify.com/nerolabs/phone-number-validator) | $0.002 per phone number |
| See what tech a website runs on (Shopify, WordPress...) | `detect-website-tech-stack` | [Tech Stack Detector: Wappalyzer & BuiltWith Alt, CSV or Sheet](https://apify.com/nerolabs/tech-stack-detector) | $0.005 per website |
| Check domain age, expiry and WHOIS | `check-domain-whois-age` | [WHOIS Lookup & Domain Age Checker: Bulk Expiry, RDAP from CSV](https://apify.com/nerolabs/domain-whois-checker) | $0.005 per domain |
| Classify, tag or extract from every row with AI | `ai-enrich-rows` | [Dataset AI Enrichment: Bulk LLM & GPT Classify and Extract](https://apify.com/nerolabs/dataset-ai-enrich) | $0.005 per row enriched |
| Make charts and a PDF report from data | `make-charts-and-pdf-report` | [Chart Generator & PDF Report from Any Dataset, CSV or JSON](https://apify.com/nerolabs/dataset-charts-report) | $0.03 per chart |
| Load data into Postgres, Supabase or MySQL | `push-data-to-database` | [Dataset to Postgres, Supabase & MySQL (Database Push)](https://apify.com/nerolabs/dataset-to-database) | $0.002 per row written |
| Chain several Apify Actors in one run | `chain-apify-actors` | [Actor Pipeline Runner (Chain Actors in One Run)](https://apify.com/nerolabs/actor-pipeline-runner) | $0.05 per pipeline run |
| Check if a UK business or trader is legit | `verify-uk-business` | [UK Business Trust Check: Verify Any UK Company or Trader](https://apify.com/nerolabs/uk-business-trust-check) | $0.05 per business checked |
| Full risk report on a UK company | `uk-company-risk-report` | [UK Company Risk Report (Companies House + Gazette)](https://apify.com/nerolabs/uk-company-risk-report) | $0.5 per risk report delivered |
| Watch UK companies for new filings and director changes | `monitor-uk-companies-house` | [UK Companies House Monitor: Filings, Directors & PSC Alerts](https://apify.com/nerolabs/uk-companies-house-monitor) | $0.01 per company lookup |
| Alerts on new SEC filings (8-K, Form 4, 13D) | `monitor-sec-filings` | [SEC Filings Monitor: EDGAR 8-K, Form 4 Insider & 13D Alerts](https://apify.com/nerolabs/sec-edgar-filing-monitor) | $0.1 per result |
| Look up or watch Irish companies | `ireland-company-lookup` | [Ireland CRO Company Registry Lookup & Monitor: Filing Alerts](https://apify.com/nerolabs/ireland-cro-monitor) | $0.01 per company lookup |
| Look up or watch Polish companies (KRS) | `poland-company-lookup` | [Poland KRS Company Registry Lookup & Monitor (KYB)](https://apify.com/nerolabs/poland-krs-monitor) | $0.01 per company record returned |
| Amazon Buy Box price, seller and stock | `amazon-price-buybox-check` | [Amazon Buy Box & Price Monitor: Seller, Stock & Price Tracker](https://apify.com/nerolabs/amazon-buybox-monitor) | $0.012 per product lookup |
| Depop listings and new-listing alerts | `depop-listings-monitor` | [Depop Scraper & Monitor: New Listings, Price & Sold Alerts](https://apify.com/nerolabs/depop-marketplace-monitor) | $0.015 per listing check |
| UK self-storage prices and offers | `uk-self-storage-prices` | [UK Self-Storage Price & Offer Monitor](https://apify.com/nerolabs/uk-storage-price-monitor) | $0.1 per store checked (one-time lookup) |
| Find the nearest UK tip for a postcode | `find-uk-tip-recycling-centre` | [UK Waste & Tip Finder](https://apify.com/nerolabs/uk-waste-tip-finder) | $0.02 per site found |
| FFXIV market board prices and deal alerts | `ffxiv-market-prices` | [FFXIV Market Board Price Checker + Deal Monitor](https://apify.com/nerolabs/ffxiv-market-monitor) | $0.01 per item price checked |
| Irish passport application status | `ireland-passport-status` | [Ireland Passport Application Status Tracker](https://apify.com/nerolabs/ireland-passport-tracker) | $0.01 per application lookup |
| USCIS processing times and inquiry dates | `uscis-processing-times` | [USCIS Processing Time Monitor](https://apify.com/nerolabs/uscis-processing-time-monitor) | $0.02 per processing-time lookup |
| Australia visa processing times | `australia-visa-processing-times` | [Australia Visa Processing Time Monitor](https://apify.com/nerolabs/au-visa-processing-monitor) | $0.01 per visa processing-time lookup |
| New Zealand visa processing times | `new-zealand-visa-processing-times` | [New Zealand Visa Processing Time Monitor](https://apify.com/nerolabs/nz-visa-processing-monitor) | $0.01 per visa processing-time lookup |

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

- **Connector:** Apify's hosted MCP server at `mcp.apify.com`, limited to the 38 Nero Labs Actors listed above. You sign in to it with Apify OAuth. Claude sends the inputs you give (dataset IDs, file and sheet links, lists of URLs, inline rows) to Apify, which runs the Actor in your Apify account and stores the results in your account's datasets and key-value stores, under your Apify data retention settings.
- **Links you supply:** the Actors download the files, sheets, websites and media you point them at.
- **Outside services used by some Actors:**
  - Bulk Transcription and Call Score send the audio of your recordings to OpenAI's speech and language API (`api.openai.com`) to transcribe and review them.
  - Bulk PageSpeed & Lighthouse Checker sends each page URL to Google's PageSpeed Insights API (`googleapis.com`).
  - HTTP Request Sender sends your rows to the API endpoint you choose, with the credentials you give it.
  - Dataset to Postgres, Supabase & MySQL writes your rows into the database you choose, with the connection string you give it.
  - Dataset AI Enrichment and the Chart Generator's optional summary send row text to AI models through Apify's OpenRouter Actor (openrouter.ai).
  - Website Contact Finder, Tech Stack Detector and UK Business Trust Check read the public websites you list; Trust Check and the company tools query official registers (Companies House, The Gazette, UK Sanctions List, Food Standards Agency, Ireland CRO, Poland KRS, SEC EDGAR).
  - The monitors read their public sources (Amazon, Depop, Big Yellow, Shurgard, Universalis, OpenStreetMap, Irish DFA passport tracker, USCIS, Australian Home Affairs, Immigration New Zealand), some through Apify's proxy.
  - The data tools (filter, clean, join, pivot, diff, OCR, PDF, screenshots, downloads, upscaling, email and phone validation, WHOIS, pipeline runner) work inside the Apify run, apart from reading the links, domains and DNS records you give them.
- Nero Labs, the publisher, receives the usual Apify developer statistics (run counts and charges), not your data.

Full privacy policy: [PRIVACY.md](PRIVACY.md).

## Support

Open an issue on any Actor's **Issues** tab on Apify (for example https://apify.com/nerolabs/dataset-filter-transform/issues), or on this repository.

## About

Built by Nero Labs (Nero Engine Group Ltd, UK). All Actors: https://apify.com/nerolabs. Licence: MIT.
