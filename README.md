# Ascend Elements

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Ascend Elements is a Westborough, Massachusetts battery materials company that manufactures engineered lithium-ion battery materials from elements reclaimed from spent lithium-ion batteries and gigafactory manufacturing scrap. Founded in 2015 as Battery Resourcers out of MIT-affiliated research by Eric Gratz and Professor Yan Wang, it recovers up to 98% of the critical battery metals in end-of-life cells and uses its patented Hydro-to-Cathode direct cathode precursor (pCAM) synthesis process to produce cathode precursor and battery-grade lithium carbonate.

- https://ascendelements.com/

## Facilities

| Site | Role |
|---|---|
| Westborough, MA | Corporate HQ, R&D, demonstration-scale Hydro-to-Cathode line |
| Covington, GA | "Base 1" — North America's largest EV battery recycling facility |
| Hopkinsville, KY | "Apex 1" — commercial pCAM plant |
| Novi, MI | Commercial office |

## API surface

**Ascend Elements publishes no public developer API**, no developer portal, no SDK, no CLI, no sandbox, no changelog and no status page. It is a materials manufacturer, not a software vendor.

The only machine-readable surface on its own hosts is the **WordPress REST API** at `https://ascendelements.com/wp-json` — 362 routes across 21 namespaces, anonymously readable for published content. Alongside the WordPress core collections it carries three company-specific custom post types:

- `sourcing-events` — open supplier bid packages for the Apex 1 plant (the one collection with real third-party utility)
- `media_coverage` — curated third-party press coverage
- `job` — careers board, with `job_category` and `job_location` taxonomies (the location terms double as the published facility list)

The OpenAPI in this repo was **derived** by the API Evangelist enrichment pipeline from the live route index and from JSON Schemas returned by HTTP OPTIONS — Ascend Elements does not publish an OpenAPI itself.

> Note for crawlers: every non-`/wp-json/` path on ascendelements.com returns HTTP 503 with `Retry-After: 600` to non-browser clients. The REST API is exempt and is the supported path.

## Artifacts

| Artifact | Path |
|---|---|
| OpenAPI 3.1 (derived) | `openapi/ascend-elements-wordpress-rest-openapi.yml` |
| Overlay | `overlays/ascend-elements-wordpress-rest-overlay.yaml` |
| Authentication | `authentication/ascend-elements-authentication.yml` |
| Conventions | `conventions/ascend-elements-conventions.yml` |
| Error catalog | `errors/ascend-elements-problem-types.yml` |
| Data model | `data-model/ascend-elements-data-model.yml` |
| Lifecycle | `lifecycle/ascend-elements-lifecycle.yml` |
| Conformance | `conformance/ascend-elements-conformance.yml` |
| MCP (candidate) + tool crosswalk | `mcp/` |
| Agentic access | `agentic-access/ascend-elements-agentic-access.yml` |
| Agent skills | `skills/` |
| Well-known probes | `well-known/ascend-elements-well-known.yml` |
| Domain security | `security/ascend-elements-domain-security.yml` |
| Packages | `packages/ascend-elements-packages.yml` (empty — no SDKs exist) |
| llms.txt | `llms/ascend-elements-llms.txt` |

No A2A agent card, AsyncAPI, webhooks, OAuth scopes, security.txt, trust center or vulnerability disclosure program was found; per the pipeline's no-fabrication rule those artifacts were not authored.

## Disambiguation

Unrelated to Ascend.io, Ascend Software, Ascend RMS, Ascend.sh or Ascend (useascend.com) — several of which do publish developer APIs.
