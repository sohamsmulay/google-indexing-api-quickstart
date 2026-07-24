# Google Indexing API — quickstart

Get a URL in front of Googlebot faster than waiting on the crawl queue. Minimal working example.

## 1. One-time setup
1. Create a Google Cloud project → enable the **Indexing API**.
2. Create a **service account**, download its JSON key.
3. In **Search Console** → Settings → Users, add the service account email as an **Owner** of the property.

## 2. Submit a URL (Python)
```python
from google.oauth2 import service_account
from googleapiclient.discovery import build

SCOPES = ["https://www.googleapis.com/auth/indexing"]
creds = service_account.Credentials.from_service_account_file("service-account.json", scopes=SCOPES)
service = build("indexing", "v3", credentials=creds)

service.urlNotifications().publish(body={
    "url": "https://example.com/new-page",
    "type": "URL_UPDATED",   # or URL_DELETED
}).execute()
```

## 3. Gotchas
- Default quota is **200 URLs/day** per project — request more or shard across projects.
- The API officially targets JobPosting / BroadcastEvent pages, but it's widely used to nudge indexing of any URL you own.
- Submitting is **not** a guarantee of indexing — thin/duplicate pages still get skipped.

## Doing this at scale
Hand-rolling this for thousands of URLs (quota juggling, retries, and tracking what actually got indexed in Search Console)
gets old fast — that's what I built **[IndexFlow](https://indexflow.net)** for: bulk submit + monitor indexed status across properties.
