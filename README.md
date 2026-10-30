# Moz

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

Moz is a Seattle-based search-marketing software company, founded in 2004 as SEOmoz and part of Ziff Davis since 2021, whose products include Moz Pro, Moz Local, STAT and Moz Data. Its developer surface is the Moz API, a JSON-RPC 2.0 API served from a single universal endpoint at https://api.moz.com/jsonrpc, exposing 62 public methods across five namespaces: site metrics (Domain Authority, Page Authority, Brand Authority, Spam Score), the Moz Link Index (links, linking root domains, anchor text, link intersect, link status, final redirect), keyword metrics (volume, difficulty, opportunity, priority, search intent, related keywords), ranking and top-page data, and Moz Local location, group, account, listing-network and Google Business Profile insight management. Requests are authenticated with a Moz API token in an x-moz-token header, billed against a row-based monthly quota, and Moz additionally runs two beta remote MCP servers - Moz Data and Moz Local - at https://api.moz.com/mcp/v1/data and https://api.moz.com/mcp/v1/local, protected by OAuth 2.1 with PKCE and RFC 9728 protected-resource metadata.

**Areas:** SEO · Search · Marketing · Analytics · Keywords · Backlinks · Local Marketing · Domain Authority · Link Index · JSON-RPC · MCP · Company

## Moz on APIs.io

Everything profiled for Moz is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/moz/](https://apis.io/providers/moz/)** — or go straight to one of the APIs or properties below.

### APIs (3)

- [Moz API](https://apis.io/apis/moz/moz-api/) — The Moz API is a JSON-RPC 2.0 API accessed through HTTP POST requests to a single universal endpoint, https://api.moz.com/jsonrpc. The method name in the…
- [Moz MCP Server - Data](https://apis.io/apis/moz/moz-mcp-server-data/) — Beta remote Model Context Protocol server exposing Moz Data API tools (site metrics, links, keywords, rankings) to LLM hosts such as Claude Desktop, ChatGPT…
- [Moz MCP Server - Local](https://apis.io/apis/moz/moz-mcp-server-local/) — Beta remote Model Context Protocol server exposing Moz Local tools for managing and reporting on business locations and listings. Streamable HTTP transport;…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [Moz MCP Servers](https://apis.io/servers/moz/moz-mcp-servers/)

#### JSON Schema (1)

The data models the API reads and writes.

- [Moz Api](https://apis.io/schemas/moz/moz-api/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Moz Authentication](https://apis.io/security/moz/moz-authentication/)
- [Moz Domain Security](https://apis.io/security/moz/moz-domain-security/)
- [Moz Trust Center](https://apis.io/security/moz/moz-trust-center/)
- [Moz Vulnerability Disclosure](https://apis.io/security/moz/moz-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Moz Scopes](https://apis.io/scopes/moz/moz-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Moz Plans Pricing](https://apis.io/plans/moz/moz-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Moz Rate Limits](https://apis.io/rate-limits/moz/moz-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://moz.com/)
- [Documentation](https://moz.com/api/docs)
- [Getting started](https://moz.com/api/docs/guides/getting-started)
- [API reference](https://moz.com/api/docs/welcome)
- [Pricing](https://moz.com/products/api/pricing)
- [Support](https://moz.com/help)
- [Blog](https://moz.com/blog)
- [Terms of service](https://moz.com/terms-of-use)
- [Privacy policy](https://moz.com/privacy-policy)
- [GitHub](https://github.com/seomoz)

## API Evangelist rating

**Kin Score 46.9** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.3 |
| Contract quality | 13.7 |
| Contract governance | 18.2 |
| Operational transparency | 34.2 |
| Developer ergonomics | 58.9 |
| Access clarity | 69.7 |

**Agent readiness 34.9** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | — |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | verified |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | documented (partial credit) |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | na |

**Regulatory alignment 40.2** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/moz/](https://apis.io/providers/moz/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Moz: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/moz/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
