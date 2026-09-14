## How to run it

\`\`\`bash
cd scraper
npm install
node src/index.js
\`\`\`

Produces `output/books.json` (60 validated records), `output/errors.json` (any invalid or failed
records with reasons), and `output/run-report.json` (a summary of the run).

## Record schema

| Field | Type | Notes |
|---|---|---|
| title | string | |
| product_url | string (URL) | canonical identity — deduped on this |
| price_text | string | original text, e.g. "£51.77" |
| price_gbp | number | normalized from price_text |
| availability_text | string | |
| rating_text | string \| null | e.g. "Three" |
| description | string \| null | null when the page has none — never invented |
| source_page | string (URL) | which catalogue page this book was discovered on |
| fetched_at | string (ISO timestamp) | when this record was fetched |

## Politeness rules followed

- Identifies itself with a clear User-Agent naming this project and linking to the repo.
- Every request has an 8-second timeout — never waits forever.
- Waits at least 500ms between real requests to the site (cached pages need no delay).
- Checks the status code before doing anything else — only 200 is treated as success.
- Retries once on timeouts and 5xx/429 errors; never retries 404 or 403 (asking again won't help, or the
  site has already said no).
- Caches every page locally after the first fetch, so repeated development runs never re-hit the site.

## Sample run report

\`\`\`json
[paste your actual output/run-report.json content here]
\`\`\`

## One honest limitation

[Write 1-2 sentences here — for example: character encoding needed a manual fix partway through
(response.text() was mis-decoding UTF-8 as something else); or: retry logic is simple, a single retry
with a fixed delay rather than proper exponential backoff, which next week's assignment addresses properly.]

## Why this needed no browser

The book data (title, price, description, etc.) is already present in the raw HTML the server sends —
there's no JavaScript rendering required to see it. A tool like Playwright would add real cost (a full
browser engine, much slower execution) for zero benefit here, since nothing is hidden behind
client-side rendering.

## Ethics note

This project only scrapes books.toscrape.com, a site explicitly built and maintained for people to
practice scraping on. I would not reuse this code against a real business's site without first checking
its robots.txt, terms of service, and whether an official API exists — scraping should always be a
last resort, not a first instinct, and should never bypass logins, paywalls, or explicit blocks.

## One honest limitation

The retry logic is intentionally simple — a single retry with a fixed 1-second delay on timeouts and
5xx/429 errors, not proper exponential backoff. On a flakier site than this practice sandbox, one retry
might not be enough, and a fixed delay doesn't respect a server's actual Retry-After header if it sends
one. I also hit a real encoding bug partway through building this: my first version used
response.text() to read each page, which silently mis-decoded the site's UTF-8 content — prices showed
as "Â£51.77" instead of "£51.77", and accented text in one book's French description came through as
garbled symbols. The fix was forcing explicit UTF-8 decoding via response.arrayBuffer() and
TextDecoder("utf-8") instead of trusting the default. It's a good reminder that "the request succeeded"
and "the data is actually correct" are two different checks — a 200 status code says nothing about
whether the bytes were decoded properly, which is exactly the kind of thing this assignment's "trust
nothing you scraped" rule is about.