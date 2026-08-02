---
name: Monitor Ascend Elements news and press coverage
description: Pull Ascend Elements' own press releases and the third-party media coverage it curates, filtered by category and date, for competitive or market monitoring.
api: openapi/ascend-elements-wordpress-rest-openapi.yml
operations:
  - getWpV2Posts
  - getWpV2PostsId
  - getWpV2MediaCoverage
  - getWpV2MediaCoverageId
  - getWpV2Categories
  - getWpV2Search
---

# Monitor Ascend Elements news and press coverage

Ascend Elements maintains two separate streams: `posts` (its own announcements) and
`media_coverage` (a curated list of third-party articles about it). Both read anonymously.

**Base URL:** `https://ascendelements.com/wp-json`

## Auth

None required. Published content reads anonymously.

## Steps

1. **Resolve the category ids once and cache them.** `getWpV2Categories` —
   `GET /wp/v2/categories?_fields=id,slug,name,count`. The live taxonomy is `awards`, `company`,
   `coverage`, `customers`, `news`, `products`, `uncategorized`. You need the numeric `id`,
   because the filter on posts takes ids, not slugs.

2. **Pull announcements.** `getWpV2Posts` — `GET /wp/v2/posts`:

   ```
   GET /wp-json/wp/v2/posts?categories={newsId}&after=2026-01-01T00:00:00&per_page=100&orderby=date&order=desc&_fields=id,date,modified,slug,link,title,excerpt
   ```

   `categories`, `after`, `before`, `search`, `orderby` and `order` are all real query
   parameters on this operation. 66 posts existed when this skill was written — read
   `X-WP-Total` rather than assuming.

3. **Page properly.** `per_page` maxes at 100. Follow the `Link` header's `rel="next"` until it
   is absent; do not increment `page` past `X-WP-TotalPages` or you get a 400
   `rest_post_invalid_page_number`.

4. **Get the body of anything interesting.** `getWpV2PostsId` — `GET /wp/v2/posts/{id}`.
   `content.rendered` is HTML; strip tags before summarizing.

5. **Pull third-party coverage separately.** `getWpV2MediaCoverage` —
   `GET /wp/v2/media_coverage?per_page=100&orderby=date&order=desc`, and
   `getWpV2MediaCoverageId` for a single item. This is a distinct custom post type (61 items
   observed) — it will **not** appear in `/wp/v2/posts`. Querying only `posts` is the common
   mistake here and silently loses the coverage stream.

6. **Keyword search across everything.** `getWpV2Search` — `GET /wp/v2/search?search=lithium` —
   spans all published types at once and returns `id`, `title`, `url`, `type` and `subtype`.
   Use it when you do not know which collection a topic lives in.

## Conventions that apply

- **Incremental sync:** persist the max `modified` you have seen and use `modified_after` on the
  next run. Server-side filtering beats client-side diffing.
- **Sparse fields:** `?_fields=` — post bodies are large; ask only for what you need.
- **Errors:** `{code, message, data:{status}}` JSON, not RFC 9457.
  See `errors/ascend-elements-problem-types.yml`.
- **Caching:** `cache-control: max-age=600`. Poll at most every 10 minutes.
- **No rate-limit headers** on `/wp-json/`, but be a good citizen — this is a corporate website,
  not a metered API product.

## Warnings

- **Use the REST API, not the website.** Non-`/wp-json/` paths return HTTP 503 with
  `Retry-After: 600` to non-browser clients.
- Responses carry `x-robots-tag: noindex`. Ascend Elements does not intend this surface to be
  indexed; use it for analysis, and attribute and link back to the canonical `link` field rather
  than republishing `content.rendered`.
- Do not confuse this company with Ascend.io, Ascend Software, Ascend RMS or Ascend.sh.
