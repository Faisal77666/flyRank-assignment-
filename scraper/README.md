# Polite Scraper

## Target classification

- **Site:** Books to Scrape (https://books.toscrape.com)
- **Why:** This is an explicit sandbox site built by the maintainers of Scrapy specifically so people can
  practice web scraping without harming a real business or violating anyone's actual terms of service.
- **Scope:** Only the first 3 catalogue pages (60 books total) — not the entire site.
- **Data collected:** Book title, price, availability, star rating, description, and product URL — all
  publicly displayed information, no user data, no login-gated content.
- **robots.txt result:** [fill in exactly what you saw when you checked the URL above]
- **Why this is appropriate:** The site exists for this exact purpose, the data is public and already
  rendered in the HTML (no bypassing of any access control), and I'm only taking a small, fixed sample
  rather than scraping the entire site repeatedly.

I will not reuse this code on another site without checking its rules and terms first.