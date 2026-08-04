---
name: Search the web and extract page content
description: Use Parallel Search to find relevant URLs and excerpts for a natural-language objective, then Extract clean markdown from the best URLs.
api: openapi/parallel-openapi-original.json
operations:
  - v1_search_v1_search_post
  - extract_v1_extract_post
---

# Search the web and extract page content

Authenticate every request with the `x-api-key` header (get a key at
https://platform.parallel.ai). Base URL is `https://api.parallel.ai`.

## Steps

1. **Search** — `POST /v1/search` (`v1_search_v1_search_post`). Send a natural-language
   `objective` (and optional `search_queries`). You get back ranked URLs with
   compressed, LLM-optimized excerpts. Prefer expressing intent as an objective
   ("Columbus-based corporate law firms specializing in disability care") over
   keyword boolean strings.
2. **Extract** — `POST /v1/extract` (`extract_v1_extract_post`). Pass the URLs you
   want full content for; receive clean markdown (handles JS-rendered pages and PDFs).

## Rules

- Search and Extract are each limited to 600 create-requests/minute; only POST
  creates count (see `rate-limits/parallel-rate-limits.yml`).
- There is no idempotency key — do not assume safe retries create no duplicate work.
- Handle `401` (bad/missing key), `402` (insufficient credit), `429` (quota) per
  `errors/parallel-problem-types.yml`.
