# Reading the Search Console 'Page indexing' report

The **Page indexing** report tells you exactly why URLs are or aren't indexed. Decode the common statuses and you know what to fix.

## Statuses that matter
- **Crawled – currently not indexed** → Google saw it but judged it not worth indexing (thin, duplicate, low value). Improve the page; the Indexing API alone won't fix it.
- **Discovered – currently not indexed** → known but not yet crawled (crawl budget / queue). This is where the **Indexing API** helps most.
- **Duplicate without user-selected canonical** → add a canonical or consolidate near-identical pages.
- **Duplicate, Google chose different canonical** → Google picked another URL as canonical; align your signals.
- **Excluded by 'noindex'** → a stray meta or header tag; remove it if the page should rank.
- **Soft 404** → the page looks empty/error-like; add real content or return a proper status.
- **Blocked by robots.txt** → unblock if it should be crawled.

## A workflow that works
1. Group URLs by status and tackle the biggest bucket first.
2. Fix the **underlying cause** (content, canonical, internal links, noindex).
3. Submit the fixed URLs via the Indexing API to jump the queue.
4. Use **Validate Fix** and re-check after a couple of days.

## Don't chase ghosts
"Crawled – not indexed" on thin pages is Google telling you the page isn't worth it. Consolidate or improve rather than spamming resubmissions.

Tracking these statuses across thousands of URLs and multiple sites is what [IndexFlow](https://indexflow.net) is built to monitor.
