---
name: enrich-job-posts
description: Add clean AI fields to scraped job posts: yearly salary min and max in one currency (never invented, checked against the job text), seniority, remote, hybrid or onsite, visa sponsorship, contract type, job category, required skills, years of experience and a one-line summary. Use when the user has LinkedIn Jobs, Indeed, Glassdoor or career-site (Greenhouse, Lever, Ashby, Workday) results in a dataset, CSV or Google Sheet and wants to filter by pay, level, remote or visa, normalise salaries, build a job board, or research hiring and skills.
---

# Job Post Enricher: Salary, Seniority, Remote, Visa & Skills

Runs the Nero Labs Apify Actor `nerolabs/job-post-enricher` (https://apify.com/nerolabs/job-post-enricher) through the connector tool `nerolabs--job-post-enricher`.

## Use it for requests like

- "Which of these LinkedIn jobs pay over $120k and are remote?"
- "Normalise the salaries in this Indeed scrape to yearly GBP"
- "Tag every job in this sheet with seniority and skills"
- "Find the jobs that sponsor visas"

## What to pass

- Source: `datasetId` (any jobs scraper's dataset), `fileUrl` (CSV, Excel, JSON or Google Sheet), `jobTexts` (whole posts as plain text) or `data`.
- Title, description, company, location and salary columns are detected automatically, including nested ones like `description.text`; override with `descriptionField`, `titleField`, `salaryField` only if needed.
- `salaryCurrency`: currency for the yearly columns (default `USD`, or `original`). `hoursPerWeek` (default 40) for hourly pay when the job does not say.
- `includeExtras: true` adds benefits, education level and languages at the same price.

## How to run it

1. Call the `nerolabs--job-post-enricher` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/job-post-enricher` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `jobTexts`: whole job posts pasted as plain text.
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.003 per job enriched. 1,000 jobs = about $3. Rows with no text and failed AI calls are never charged.
- $0.01 per exported file, $0.02 per webhook delivery.

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

Each job with its original columns plus salaryMinYearly, salaryMaxYearly, salary as written with period and confidence, seniority, workMode, visaSponsorship, contractType, jobCategory, requiredSkills, years of experience and jobSummary. Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
