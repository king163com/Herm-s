---
name: api-integration
version: 1.0.0
category: devops
description: HTTP API integration patterns — REST calls, authentication, error handling, retries, and rate limiting.
tags: [api, http, rest, urllib, requests, integration]
---

# API Integration

Standard patterns for making HTTP API calls from Python and shell. Covers auth, retries, error handling, and common API patterns.

## When to Activate

- Making HTTP requests (GET/POST/PUT/DELETE)
- Working with REST APIs (GitHub, OpenAI, etc.)
- Handling API errors or rate limits
- API authentication (Bearer, API keys, OAuth)

## Python Patterns

### Basic GET with Bearer Token

```python
import urllib.request, json

token = open("~/.gh_token").read().strip()
url = "https://api.github.com/repos/owner/repo"
req = urllib.request.Request(url, headers={
    "Authorization": f"token {token}",
    "Accept": "application/vnd.github.v3+json"
})
res = urllib.request.urlopen(req, timeout=10)
data = json.loads(res.read())
```

### POST with JSON Body

```python
import urllib.request, json

data = {"title": "Hello", "body": "World"}
req = urllib.request.Request(
    url,
    data=json.dumps(data).encode(),
    headers={"Authorization": f"Bearer {token}", "Content-Type": "application/json"},
    method="POST"
)
res = urllib.request.urlopen(req, timeout=10)
```

### Retry Pattern

```python
import time
for attempt in range(3):
    try:
        res = urllib.request.urlopen(req, timeout=10)
        break
    except urllib.error.HTTPError as e:
        if e.code == 429:  # Rate limited
            retry_after = int(e.headers.get("Retry-After", 60))
            time.sleep(retry_after)
        else:
            raise
```

## Shell Patterns (curl)

```bash
# GET with Bearer token
curl -s -H "Authorization: Bearer $TOKEN" https://api.example.com/data

# POST with JSON
curl -s -X POST -H "Content-Type: application/json" \
  -d '{"key":"value"}' \
  -H "Authorization: Bearer $TOKEN" \
  https://api.example.com/endpoint

# With timeout
curl -s --max-time 10 -H "Authorization: Bearer $TOKEN" https://api.example.com/
```

## Error Handling

| Error Code | Meaning | Action |
|------------|---------|--------|
| 401 | Unauthorized | Check/refresh token |
| 403 | Forbidden | Check token scopes/permissions |
| 429 | Rate limited | Respect Retry-After header |
| 500+ | Server error | Retry with backoff |

## Anti-patterns

- ❌ Hardcoding tokens in scripts (use `~/.gh_token` or env vars)
- ❌ No timeout on requests
- ❌ Ignoring HTTP error codes
- ❌ Sending secrets in URL query params
