---
name: Browse Ascend Elements careers and facilities
description: Read the Ascend Elements careers board and its job function/location taxonomies — the location taxonomy doubles as the company's published facility list with capacity descriptions.
api: openapi/ascend-elements-wordpress-rest-openapi.yml
operations:
  - getWpV2Job
  - getWpV2JobId
  - getWpV2JobCategory
  - getWpV2JobLocation
  - getWpV2Pages
---

# Browse Ascend Elements careers and facilities

The careers board is a custom post type with two taxonomies. The `job_location` taxonomy is
worth knowing about independently: its term descriptions carry Ascend Elements' facility
profiles (Base 1 in Covington GA, Apex 1 in Hopkinsville KY, Novi MI, Westborough MA, Global),
including plant capacity language. It is the cleanest structured source for the company's
physical footprint.

**Base URL:** `https://ascendelements.com/wp-json`

## Auth

None. All three collections read anonymously.

## Steps

1. **List facilities.** `getWpV2JobLocation` —
   `GET /wp/v2/job_location?_fields=id,name,slug,link,description,count`.

   The `description` field is HTML and contains the facility profile — e.g. the Base 1 entry
   describes recycling capacity in metric tons of EV batteries per year. Strip tags before use.
   Five terms exist.

2. **List job functions.** `getWpV2JobCategory` —
   `GET /wp/v2/job_category?_fields=id,name,slug,link,description,count`. Observed terms:
   Corporate Positions, Engineering, Finance & Accounting, Plant Operations, Business
   Development & Marketing, Global Supply Chain, Science & Technology, All Positions.

3. **List open positions.** `getWpV2Job` — `GET /wp/v2/job?per_page=100&_fields=id,slug,link,title,date,job_category,job_location`.

   **Expect an empty array.** The collection is registered and readable but returned zero items
   and `X-WP-Total: 0` when this skill was written, and every taxonomy term shows `count: 0`.
   That is a real state, not an error — handle it rather than retrying.

4. **Filter when postings exist.** Pass taxonomy term ids from steps 1–2:
   `GET /wp/v2/job?job_location={locationId}&job_category={categoryId}`.

5. **Read one posting.** `getWpV2JobId` — `GET /wp/v2/job/{id}`; `content.rendered` holds the
   description.

6. **Fall back to the pages surface for context.** `getWpV2Pages` —
   `GET /wp/v2/pages?slug=employee-engagement` returns the careers culture page. The `about-us`
   page carries the leadership team.

## Conventions that apply

- **Taxonomy `count` is authoritative** — check it before paging a collection that may be empty.
- **Sparse fields** (`?_fields=`) and **expansion** (`?_embed=1`) both work.
- **Pagination:** `page` / `per_page` (max 100), with `X-WP-Total` / `X-WP-TotalPages`.
- **Errors:** `{code, message, data:{status}}` JSON, not RFC 9457.
  See `errors/ascend-elements-problem-types.yml`.

## Warnings

- **Do not fetch `https://ascendelements.com/careers/` directly** — like every other HTML path
  on the host it returns HTTP 503 with `Retry-After: 600` to non-browser clients. The REST API
  is the working path.
- An empty careers board is not evidence the company is not hiring; postings may be handled
  off-site. Do not infer hiring posture from `X-WP-Total: 0`.
