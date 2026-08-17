# Crawl budget: what it is and how to protect it

**Crawl budget** is roughly how many URLs Googlebot will crawl on your site in a given period. Small sites rarely hit the limit; large sites (100k+ URLs) can starve important pages if they waste it.

## Where budget leaks
- **Faceted / filter URLs** — infinite parameter combinations (`?color=red&size=xl&sort=price`) that generate near-duplicate pages.
- **Session IDs / tracking params** creating unique URLs for the same content.
- **Soft 404s and redirect chains** that Google keeps re-crawling.
- **Low-value pages** (tag archives, thin pagination) crawled instead of money pages.

## How to protect it
- **robots.txt** disallow crawl traps (facets, internal search results) you don't want indexed.
- Consolidate duplicates with **canonicals**.
- Fix **redirect chains** (301 straight to the final URL).
- Return proper **404/410** for dead pages so Google stops re-crawling them.
- Strong **internal linking** to priority pages so they're found and re-crawled often.
- Keep the **sitemap** clean and `lastmod` honest so Google spends budget on what changed.

## Signals to watch
The **Crawl stats** report in Search Console shows requests over time and by response code — spikes of 404s or redirects are budget leaks.

For large multi-section sites, keeping sitemaps clean and confirming the important URLs actually get indexed is what [IndexFlow](https://indexflow.net) is built to help with.
