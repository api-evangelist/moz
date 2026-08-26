---
name: moz-keyword-research
description: Research keywords with the Moz API — volume, difficulty, organic CTR opportunity, priority, search intent, related keywords, and the keywords a site already ranks for.
api: Moz API
endpoint: https://api.moz.com/jsonrpc
generated: '2026-08-26'
method: generated
source: json-schema/moz-api-schema.json + https://moz.com/api/docs
operations:
  - quota.lookup
  - data.keyword.metrics.fetch
  - data.keyword.metrics.volume.fetch
  - data.keyword.metrics.difficulty.fetch
  - data.keyword.metrics.opportunity.fetch
  - data.keyword.metrics.priority.fetch
  - data.keyword.search.intent.fetch
  - data.keyword.suggestions.list
  - data.site.ranking-keyword.list
  - data.site.ranking-keyword.count
  - data.global.top-domain.list
  - data.global.top-page.list
---

# Moz keyword research

POST to `https://api.moz.com/jsonrpc` with `x-moz-token`. JSON-RPC 2.0 body; `id` at least 24
characters; arguments under `params.data`.

## Use the combined call

`data.keyword.metrics.fetch` returns `{volume, difficulty, organic_ctr, priority}` in one response.
The four single-metric methods — `.volume.fetch`, `.difficulty.fetch`, `.opportunity.fetch`,
`.priority.fetch` — exist for callers who genuinely need only one. Calling all four separately costs
four times the rows for the same data.

A keyword is addressed by a **SERP query**, not by a bare string:

```json
{"jsonrpc":"2.0","id":"<uuid-v4>","method":"data.keyword.metrics.fetch",
 "params":{"data":{"serp_query":{"keyword":"domain authority","locale":"en-US",
   "device":"desktop","engine":"google"}}}}
```

`locale`, `device`, `engine` and `vicinity` are part of the key. The same keyword in `en-GB` on mobile
is a different record — do not compare across locales as if they were one series.

## Read nulls as nulls

A null metric means Moz **could not calculate it** when the keyword was last collected. It does not
mean zero. Never render a null volume as 0 in a report; say "not available".

## Intent before difficulty

`data.keyword.search.intent.fetch` classifies a keyword as informational, navigational, commercial or
transactional. Run it before difficulty: a high-volume keyword with the wrong intent for the page you
are planning is a bad target no matter how easy it is to rank for.

## Expand

`data.keyword.suggestions.list` returns related keywords. It takes a suggestions-options object in
`params.data` controlling how many and how they are ordered — set the limit deliberately, because this
is a per-row-billed list method.

## What the site already has

- `data.site.ranking-keyword.count` — how many keywords a target ranks for. Cheap; run it first.
- `data.site.ranking-keyword.list` — the actual keywords, each row `{keyword, ranking_page,
  rank_position, difficulty, volume}`. Use the count to size the pull before you make it.

Moz's coverage is keywords the site ranks in the **top 50** in Google. Absence from this list is not
proof the site does not rank at all.

## Market view

`data.global.top-domain.list` and `data.global.top-page.list` give the top-ranking domains and pages
globally. These are large result sets — page them tightly.

## Quota

Every list method bills one row per returned object. Check `quota.lookup` with path
`api.limits.data.rows` before a bulk expansion. Beta methods draw on a separate beta quota that does
not touch your main plan, but their data may only be used for prototyping or internal business
services, and they can be removed without notice.

## Error rules

`-32652` names the missing field in `error.data.key`. Do not hot-retry 4xx responses — the penalty
limiter caps you at 50 requests per five minutes after 50 4xx responses in five minutes, and clears
after 30 quiet seconds.
