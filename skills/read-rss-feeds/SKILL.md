---
name: read-rss-feeds
description: Read the newest posts, articles or podcast episodes from many RSS, Atom or JSON feeds, or from plain website addresses (the site's own feed is found automatically), with title, link, date, author, summary, tags, image and audio link. Use when the user wants to monitor blogs, news sites, competitors or podcasts, wants only what is new since the last check, wants news on a topic from many sources filtered by keywords or date, or has a list, sheet or dataset of websites and wants each one's latest posts kept next to their own columns.
---

# RSS Feed Reader: Bulk Feeds from a CSV, Google Sheet or Dataset

Runs the Nero Labs Apify Actor `nerolabs/rss-feed-reader` (https://apify.com/nerolabs/rss-feed-reader) through the connector tool `nerolabs--rss-feed-reader`.

## Use it for requests like

- "What have these 20 competitor blogs published this week?"
- "Every morning, give me only the new posts from these feeds"
- "Find stories mentioning 'acquisition' from these news sites in the last 24 hours"
- "Get the latest episodes and audio links for these podcasts"

## What to pass

- `feedUrls`: feed links or plain website addresses, for a quick run with no spreadsheet.
- `feedField`: the feed or website column, detected automatically if empty (`feedUrl`, `feed`, `rss`, `url`, `website`...).
- `maxItemsPerFeed` (default 20): newest items per feed.
- `onlyNewItems: true` with a `stateName` per monitor returns only items not returned before, free for repeats; made for schedules and agents. The first run returns the current items.
- `publishedAfter`: a date (`2026-10-01`) or period (`24 hours`, `7 days`). `keywords` / `excludeKeywords`: whole-word filters.
- `includeContent: true` adds the full post text the feed carries. For full articles from summary-only feeds, pass the `link` column to the extract-article-text skill.
- `keep`: `all` (items plus a free row per failed feed), `items` or `problems`. `exportFormats` for CSV or Excel.

## How to run it

1. Call the `nerolabs--rss-feed-reader` tool on the Nero Labs connector (Apify's MCP server, signed in with the user's own Apify account). If the tool is missing, ask the user to connect the plugin's Nero Labs connector, or to add `https://mcp.apify.com/?tools=nerolabs/rss-feed-reader` as a connector.
2. Give it ONE input source:
   - `datasetId`: an Apify dataset ID, for example the output of a scraper run.
   - `fileUrl`: a public CSV, TSV, Excel, JSON or JSON Lines link, or a normal Google Sheets link shared as "Anyone with the link can view".
   - `feedUrls`: a plain list of feed links or websites.
   - `data`: a JSON array of rows, for small data pasted into the chat or read from a local file. Use this when the user's file is on their own computer, because the Actor runs in the cloud and cannot read local paths.
3. If the run is still going when the tool returns, poll `get-actor-run` with the run ID, then read rows with `get-dataset-items` (`clean: true`). Download links for any exported files sit in the run's key-value store (`get-key-value-store-record`).
4. Reply with the result itself (a short table, the counts, the links), not the raw JSON.

## Cost

Billed per event to the user's Apify account at the Actor's listed price. The user pays nothing else: no compute or proxy charges on top. Prices below are the Free plan rate; paid Apify plans get tier discounts. New Apify accounts include free monthly credit.

- $0.001 per item delivered. 1,000 items = about $1.
- Failed feeds, filtered items, repeats and duplicates are free.
- $0.01 per exported file, $0.005 per webhook delivery (sent only when there are new items).

Before a run likely to cost more than about $1, tell the user the estimate in one line and set a cap (`maxItems`) so the bill cannot run away.

## What comes back

One row per item with `feedTitle`, `title`, `link`, `publishedAt`, `author`, `summary`, `categories`, `imageUrl`, `enclosureUrl` and `durationSeconds` (podcasts), plus the user's own columns. Rows with `feedStatus` other than `ok` are feeds that could not be read (`no_feed_found`, `blocked`...). Results land in an Apify dataset the user owns, so they can also open or export them in the Apify Console.
