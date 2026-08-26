---
name: moz-local-location-onboarding
description: Create and manage business locations in Moz Local — accounts, groups, location creation, listing-network status, and the reversal paths for the two write operations that have them.
api: Moz API
endpoint: https://api.moz.com/jsonrpc
generated: '2026-08-26'
method: generated
source: json-schema/moz-api-schema.json + https://moz.com/api/docs
operations:
  - local.account.list
  - local.account.lookup
  - local.account.create
  - local.group.list
  - local.group.create
  - local.group.location.add
  - local.group.location.remove
  - local.location.category.list
  - local.location.level.list
  - local.location.create
  - local.location.lookup
  - local.location.list
  - local.location.update
  - local.location.listing.list
  - local.location.network.list
  - local.location.network.status.list
  - local.location.network.field.status.list
  - local.location.network.field.issue.list
  - local.location.cancel
  - local.location.close
---

# Moz Local location onboarding

This is the **only write surface** in the Moz API. Everything in the `data` namespace is read-only;
everything below changes state, and some of it changes state in places Moz does not control — the
downstream listing networks.

POST to `https://api.moz.com/jsonrpc`, `x-moz-token` header, `id` at least 24 characters.

## Read this before you write anything

**There is no idempotency key.** Moz publishes no idempotency header, no request-replay window and no
deduplication semantics. The JSON-RPC `id` is a correlation identifier only — nothing says a repeated
`id` is de-duplicated. A retried `local.location.create` after a timeout can create a second location.

**There is no dry-run mode.** No validate-only or preview flag is documented on any method.

So: never retry a write blindly. On a timeout or an ambiguous response, call `local.location.list` or
`local.location.lookup` to find out what actually happened, then decide.

## 1. Establish the account and group context

```
local.account.list      -> accounts available to this token
local.account.lookup    -> one account
local.group.list        -> groups (each carries account_id)
local.group.create      -> a new group inside an account
```

Groups organise locations; membership is managed by method (`local.group.location.add` /
`.remove` / `.list`), not by a field on either object.

## 2. Resolve the controlled vocabularies first

```
local.location.category.list  -> valid primary and additional categories
local.location.level.list     -> the tiers of service available
```

Do this before building the create payload. Category and level are not free text.

## 3. Create the location

`local.location.create` takes a `LocalLocationCreateObject`. The resulting `LocalLocationObject`
carries 38 fields — `id`, `level`, `status`, `account_id`, `auto_sync_enabled`, `remote_id`,
`listings_name`, `location_name`, `website_url`, `address`, `lat`/`long`, `primary_category`,
`additional_categories`, `attributes`, `hours` / `more_hours` / `special_hours`, `opening_status`,
`reopen_date`, `phone`, `service_areas` and the rest.

Confirm with `local.location.lookup` before doing anything else.

## 4. Watch it propagate

```
local.location.listing.list            -> {name, status, url} per listing
local.location.network.list            -> the networks this location publishes to
local.location.network.status.list     -> per-network status
local.location.network.field.status.list
local.location.network.field.issue.list -> what a network rejected and why
```

Propagation is asynchronous and lives outside Moz. Poll the network status methods rather than
assuming a successful create means a live listing.

## 5. Reversibility — what you can take back, and what you cannot

| Action | Reversal | Window |
|---|---|---|
| `local.location.create` | `local.location.cancel` | **not published** |
| `local.location.create` | `local.location.close` | **not published** |
| `local.group.location.add` | `local.group.location.remove` | none needed — membership toggles |
| `local.location.level.update` | re-invoke with the previous level | **not published** |
| `local.location.update` | **none** — no revision history, no restore | n/a |
| `local.account.create`, `local.group.create` | **none published** | n/a |

Two things follow from that table and both matter before you act:

- Moz documents *that* a location can be cancelled but never *for how long*. Do not assume a grace
  period, and do not tell a user there is one.
- `local.location.close` sets the location's opening status to **permanently closed**. Moz does not
  document whether a closed location can be reopened through the API. Treat it as one-way.
- Reverting a `local.location.update` means re-sending the previous values, so **capture the current
  `LocalLocationObject` with `local.location.lookup` before every update** and keep it. Nothing in the
  API will give it back to you.

## Errors

- `-32651` / 401 — bad or missing `x-moz-token`.
- `-32652` / 400 — `error.data.key` names the missing field; `error.data.issue` is `param-is-missing`.
- `-32601` / 400 — method name not found.
- Do not hot-retry 4xx responses: 50+ in five minutes trips the penalty limiter (50 requests per five
  minutes, cleared by 30 seconds without a 4xx). On a write, retrying is doubly wrong — see the
  idempotency warning above.

## Access

Moz Local methods require a Moz Local subscription. The MCP equivalent is the separate
`https://api.moz.com/mcp/v1/local` endpoint, which is in beta and limited to account owners.
