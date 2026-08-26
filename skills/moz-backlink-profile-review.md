---
name: moz-backlink-profile-review
description: Review a site's backlink profile with the Moz Link Index — inbound links, linking root domains, anchor text, gained and lost domains, and competitor link gaps.
api: Moz API
endpoint: https://api.moz.com/jsonrpc
generated: '2026-08-26'
method: generated
source: json-schema/moz-api-schema.json + https://moz.com/api/docs
operations:
  - quota.lookup
  - data.site.link.list
  - data.site.link.filter.domain
  - data.site.link.filter.anchor.text
  - data.site.linking-domain.list
  - data.site.linking-domain.filter.recently-gained
  - data.site.linking-domain.filter.recently-lost
  - data.site.anchor-text.list
  - data.site.link.intersect.fetch
  - data.site.link.status.fetch
  - data.site.link.status.fetch.multiple
  - data.site.redirect.fetch
---

# Moz backlink profile review

POST to `https://api.moz.com/jsonrpc`, header `x-moz-token`, JSON-RPC 2.0 body, `id` at least 24
characters. Every method below takes its arguments under `params.data`.

## Watch the row cost first

List methods return **one row per returned object**, so a single `data.site.link.list` with a large
page limit can consume thousands of rows. Call `quota.lookup` (path `api.limits.data.rows`) before and
after any list-heavy run, and set the page limit deliberately rather than taking the default.

Pagination lives in the request body — there are no `Link` headers and no cursor tokens. List methods
take an options object carrying page/limit/order controls; read the option object for the specific
method out of the schema rather than assuming a shared shape.

## 1. The domain-level picture

`data.site.linking-domain.list` — linking root domains for the target. Each row nests a full
`site_metrics` object for the linking domain, so you get its Domain Authority and Spam Score without a
second call. Also on each row: `link_propensity` and the `targeted_*` counts (pages, nofollow,
redirect, deleted) telling you how that domain links.

Root-domain counts are the number worth reporting. Raw link counts are inflated by site-wide footer
and template links; `root_domains_to_root_domain` is not.

## 2. What changed

- `data.site.linking-domain.filter.recently-gained`
- `data.site.linking-domain.filter.recently-lost`

Run both. A profile that gained 40 and lost 60 is a declining profile even though the gained list
looks healthy on its own.

## 3. Individual links

`data.site.link.list` returns `DataSiteLinkObject` rows carrying `source_site_metrics`,
`target_site_metrics`, `anchor_text`, `date_first_seen`, `date_last_seen`, `date_disappeared`, and the
flags `nofollow`, `redirect`, `rel_canonical`, `via_redirect`, `via_rel_canonical`.

Filter instead of paging when you can: `data.site.link.filter.domain` and
`data.site.link.filter.anchor.text` push the filter server-side and cost fewer rows than pulling
everything and filtering locally.

## 4. Anchor text

`data.site.anchor-text.list` returns `{text, external_root_domains, external_pages}`. Judge anchor
concentration on `external_root_domains`, not `external_pages` — one domain linking 400 times with the
same anchor is one endorsement, not 400.

## 5. Competitor gaps

`data.site.link.intersect.fetch` is the Venn-diagram method: pages linking to one target but not
another. This is the highest-value method in the namespace for prospecting and also one of the more
expensive — scope the request tightly.

## 6. Verify before you act

- `data.site.link.status.fetch` / `.fetch.multiple` — is a specific link still live?
- `data.site.redirect.fetch` — the final redirect target for a URL, which is what you need before
  reporting a link as lost during a site migration.

## Error and retry rules

- `-32652` / HTTP 400 names the missing field in `error.data.key` — usually `site_query` or `target_query`.
- Never hot-retry a 4xx: 50+ 4xx responses in five minutes trips the penalty limiter.
- No `Retry-After` or `RateLimit-*` headers exist. Back off on your own clock.

## Guardrails

Read-only namespace. Link data is a crawl snapshot tied to an index build — record
`metadata.index.fetch` alongside any figure you are going to compare later.
