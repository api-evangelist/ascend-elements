---
name: Track Ascend Elements supplier sourcing events
description: Poll Ascend Elements' open supplier bid packages (procurement sourcing events) and retrieve the full scope of any one of them. The highest-utility flow on this API for a third party.
api: openapi/ascend-elements-wordpress-rest-openapi.yml
operations:
  - getWpV2SourcingEvents
  - getWpV2SourcingEventsId
  - getWpV2Media
---

# Track Ascend Elements supplier sourcing events

Ascend Elements runs procurement for its plants — currently the Apex 1 pCAM facility in
Hopkinsville, Kentucky — by publishing bid packages as a custom post type on its website. That
collection is anonymously readable over the REST API, so a supplier can watch it for new work
instead of checking the page by hand.

**Base URL:** `https://ascendelements.com/wp-json`

## Auth

None. This collection reads anonymously. Do **not** send credentials — you do not have any, and
none are needed. Writes are unavailable anonymously in any case (the `Allow` header on content
routes is `GET` for an unauthenticated caller).

## Steps

1. **List the open packages.** `getWpV2SourcingEvents` — `GET /wp/v2/sourcing-events`.

   Keep the payload small with sparse fields:

   ```
   GET /wp-json/wp/v2/sourcing-events?_fields=id,slug,link,title,date,modified&per_page=100&orderby=date&order=desc
   ```

   Read `X-WP-Total` from the response headers to know the collection size without paging
   (4 packages at the time this skill was written: the Apex 1 CSA, Mechanical, Piping and
   Electrical packages).

2. **Detect what is new since last run.** Persist the highest `modified` timestamp you have
   seen. On the next run, ask the server to filter for you rather than diffing client-side:

   ```
   GET /wp-json/wp/v2/sourcing-events?modified_after=2026-08-02T00:00:00&_fields=id,slug,title,modified
   ```

   `modified_after` and `after` are real query parameters on this operation — they come from the
   route index, so the server does the work.

3. **Pull the full scope for a package you care about.** `getWpV2SourcingEventsId` —
   `GET /wp/v2/sourcing-events/{id}` using an `id` from step 1. The `content.rendered` field
   carries the package scope as HTML; strip tags before handing it to a model.

4. **Collect the attached bid documents.** Package specs are usually attachments. Use
   `getWpV2Media` — `GET /wp/v2/media?parent={id}` — and read `source_url` and `mime_type` off
   each item to fetch the PDFs.

5. **Route the response.** Bid questions go to `apex2@ascendelements.com` where the package
   names it, otherwise `info@ascendelements.com`. There is no submission API — the API is
   read-only discovery; the actual bid goes over email.

## Conventions that apply

- **Pagination:** `page` / `per_page` (max 100) / `offset`; `X-WP-Total` and `X-WP-TotalPages`
  headers, plus an RFC 8288 `Link` header with `rel="next"`.
- **Sparse fields:** `?_fields=` a comma-separated property list.
- **Expansion:** `?_embed=1` inlines featured media and terms.
- **Errors:** `{code, message, data:{status}}` JSON — **not** RFC 9457. Branch on `code`.
  See `errors/ascend-elements-problem-types.yml`.
- **No idempotency contract** — irrelevant here, since this flow is entirely reads.
- **Caching:** responses carry `cache-control: max-age=600`. Polling more often than every
  10 minutes buys you nothing.

## Warnings

- **Do not fetch the HTML pages.** Every non-`/wp-json/` path on ascendelements.com returns
  HTTP 503 with `Retry-After: 600` to non-browser clients. The REST API is the supported path
  and is not blocked.
- This is a corporate website CMS, not a procurement product. Ascend Elements makes no
  availability or versioning commitment about it. Treat a schema change as expected, not
  exceptional.
