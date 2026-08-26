---
name: moz-site-authority-audit
description: Pull Moz authority and link-count metrics for one or many sites, compare them over time, and report Domain Authority, Page Authority, Brand Authority and Spam Score without burning quota.
api: Moz API
endpoint: https://api.moz.com/jsonrpc
generated: '2026-08-26'
method: generated
source: json-schema/moz-api-schema.json + https://moz.com/api/docs
operations:
  - quota.lookup
  - data.site.metrics.fetch
  - data.site.metrics.fetch.multiple
  - data.site.metrics.brand.authority.fetch
  - data.site.metrics.histories.fetch
  - data.site.metrics.distributions.fetch
  - metadata.index.fetch
---

# Moz site authority audit

Every call is an HTTP POST to `https://api.moz.com/jsonrpc` with header `x-moz-token: <MOZ_API_TOKEN>`
and a JSON-RPC 2.0 body. The `id` is a client-generated string of **at least 24 characters** (use a v4
UUID) — it is not the API token. Sending a shorter one fails with error `-32654` before any work runs.

## 1. Check the budget before you spend it

Moz bills in **rows**, not requests, and there are no rate-limit headers — the only way to know your
remaining budget is to ask.

```json
{"jsonrpc":"2.0","id":"<uuid-v4>","method":"quota.lookup",
 "params":{"data":{"path":"api.limits.data.rows"}}}
```

Read `result` for `allotted`, `used`, `overage`, `period_start` and `period_reset`. Do this first on
any batch job. `data.site.metrics.fetch` costs one row per call.

## 2. Fetch metrics for a single target

```json
{"jsonrpc":"2.0","id":"<uuid-v4>","method":"data.site.metrics.fetch",
 "params":{"data":{"site_query":{"query":"https://example.com","scope":"domain"}}}}
```

`scope` is one of `page`, `subdomain`, `domain`. The response echoes a parsed `site_query` back —
**always read it**, because it tells you how Moz interpreted the input, and `site_query_suggestion`
tells you when Moz thinks you meant something else.

The `site_metrics` object carries 52 fields. The headline ones are `domain_authority`,
`page_authority`, `spam_score`, `link_propensity`, `title`, `last_crawled` and `http_code`. The link
counts follow a strict naming grammar: `<qualifier>_<unit>_to_<scope>` for inbound and
`<qualifier>_<unit>_from_<scope>` for outbound, where qualifier is one of nofollow / redirect /
external / indirect / deleted. Do not invent a field name — read it off the schema.

## 3. Batch instead of looping

For more than one target, use `data.site.metrics.fetch.multiple` rather than N single calls. Batching
is also what the JSON-RPC `id` field exists to support.

## 4. Add the two metrics that are not in the base call

- `data.site.metrics.brand.authority.fetch` — Brand Authority.
- `data.site.metrics.distributions.fetch` — where the target sits in the distribution.

## 5. Trend, do not snapshot

`data.site.metrics.histories.fetch` returns the metric over time. A single Domain Authority reading
is close to meaningless on its own — DA is a relative, log-scaled score, so report direction and
peer-relative position, never a bare number as if it were an absolute grade.

Pair it with `metadata.index.fetch` to record which Moz Link Index build produced the numbers. Two
readings from different index builds are not strictly comparable, and the index id is the only way to
tell.

## Error and retry rules

- `-32651` / HTTP 401 — missing or invalid `x-moz-token`.
- `-32652` / HTTP 400 — a required parameter is missing; `error.data.key` names it exactly.
- `-32601` / HTTP 400 — method name not found.
- **Do not hot-retry a 4xx.** More than 50 4xx responses in five minutes trips a penalty limiter that
  caps you at 50 requests per five minutes until you go 30 seconds without a 4xx. Fix the request,
  then retry.
- Quota exhaustion surfaces as a 4xx, not as a header. Poll `quota.lookup`, do not probe blind.

## Guardrails

- This namespace is entirely read-only; nothing here changes state.
- Moz's terms require attribution when Moz data is surfaced inside another product.
