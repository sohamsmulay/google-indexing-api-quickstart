# Google Indexing API — a practical quickstart

Get your URLs in front of Googlebot faster than waiting on the organic crawl queue. This is a complete, working guide — setup, code, quotas and the gotchas nobody tells you.

## When this helps (and when it doesn't)

The Indexing API tells Google "this URL changed, please recrawl it." It's great for **new pages, updated content and removed pages**. It is **not** a magic ranking button — thin, duplicate or low-value pages still won't get indexed just because you pinged the API. Use it to *speed up* discovery of pages that deserve to be indexed.

## 1. One-time setup

1. In **Google Cloud Console**, create a project and **enable the Indexing API**.
2. Create a **service account** and download its JSON key file.
3. In **Google Search Console** → your property → **Settings → Users and permissions**, add the service account's email (`...@...iam.gserviceaccount.com`) as an **Owner** of the property. This is the step people miss — without it every call returns `403 Permission denied`.

## 2. Submit a URL (Python)

```python
from google.oauth2 import service_account
from googleapiclient.discovery import build

SCOPES = ["https://www.googleapis.com/auth/indexing"]
creds = service_account.Credentials.from_service_account_file(
    "service-account.json", scopes=SCOPES)
service = build("indexing", "v3", credentials=creds)

def notify(url, kind="URL_UPDATED"):   # or "URL_DELETED"
    return service.urlNotifications().publish(
        body={"url": url, "type": kind}).execute()

print(notify("https://example.com/new-page"))
```

## 3. Batch many URLs

```python
urls = ["https://example.com/a", "https://example.com/b", ...]
for u in urls:
    try:
        notify(u)
    except Exception as e:
        print("skip", u, e)   # log and continue; don't let one bad URL stop the run
```

## 4. Quotas & scaling

- Default quota is about **200 URL notifications/day per Cloud project**.
- To go bigger: request a quota increase for the project, **and/or shard across multiple Cloud projects** — each project's service account must be added as a Search Console owner of the property.
- Prioritise **new and changed** URLs; don't waste quota re-submitting unchanged pages every day.
- Check `urlNotifications().getMetadata()` to see the last notify time.

## 5. Common errors

| Error | Cause / fix |
|-------|-------------|
| 403 Permission denied | service account not an **Owner** in Search Console for that property |
| 429 Rate limit | daily quota hit — shard across projects or wait |
| 400 Invalid URL | URL not under the verified property, or malformed |
| Submitted but not indexed | page quality/duplication — the API can't fix thin content |

## Doing this at scale

Hand-rolling quota juggling, retries, multi-project sharding and — the hard part — **tracking which URLs actually got indexed** in Search Console gets old fast across even a few sites. That's exactly what I built **[IndexFlow](https://indexflow.net)** to handle: bulk submit, multi-property support, and monitoring of real indexed status.

---
*PRs welcome — corrections, other languages, or additional gotchas.*
