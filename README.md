# Ascend Elements

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
