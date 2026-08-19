---
name: Match and enrich companies with Explorium
description: >-
  Resolve company names and domains to stable Explorium business IDs, then
  attach firmographic and technographic attributes — single record or in bulk.
  Use when an agent has a list of companies and needs data about them.
api: openapi/explorium-businesses-api-openapi.yml
operations:
  - match_businesses
  - businesses_firmographics_enrich
  - businesses_technographics_enrich
  - businesses_firmographics_bulk_enrich
  - get_active_credits_summary
generated: '2026-08-14'
method: generated
source: openapi/_original/explorium-agentsource-openapi.json
---

# Match and enrich companies with Explorium

Nothing on this API is addressable by a natural key. A domain name is **not** an
identifier here — it is an input to a match call that returns one. Budget for
two spends per company: the match, then the enrichment.

## Step 1 — check the budget

`get_active_credits_summary` (`GET /v1/credits`) returns the remaining balance.
Credits are consumed **per entity**, so a 50-company batch costs 50 on match and
50 again on each enrichment family you request. There is no test mode and no
sandbox — every call is live.

## Step 2 — match to `business_id`

Call `match_businesses` (`POST /v1/businesses/match`) with up to **50** records.

Send **name and domain together** wherever you have both — Explorium's own
guidance is that this is materially more reliable than either alone.

The response is a list the **same length and in the same order** as the input.
Unresolved entries come back with a null `business_id` — this is a normal
outcome, not an error, and the HTTP status is still `200`. Do not treat position
*n* of the output as anything other than input *n*.

## Step 3 — enrich

Pick the attribute families you actually need; each is a separate operation and
a separate credit spend. Seventeen exist. The common ones:

| Need | Operation | Path |
|---|---|---|
| Size, industry, location, revenue | `businesses_firmographics_enrich` | `POST /v1/businesses/firmographics/enrich` |
| Technology stack | `businesses_technographics_enrich` | `POST /v1/businesses/technographics/enrich` |

For more than one company, use the bulk twin —
`businesses_firmographics_bulk_enrich`
(`POST /v1/businesses/firmographics/bulk_enrich`) — which takes up to **50**
business IDs. Bulk saves round trips, **not** quota: 50 records still counts as
50 queries against the 200/minute limit and 50 credits against the balance.

Above 50 records, stop using sync entirely and switch to the async job pipeline
(see `explorium-run-a-bulk-enrichment-job.md`).

## Step 4 — read the result

Check `response_context.request_status` per response. `miss` means the entity
resolved but Explorium holds no data for that attribute family — coverage varies
by attribute and by region. Log `response_context.correlation_id`.

## Version note

The same flow exists on the v2 (beta) surface, where single and bulk collapse
into one endpoint: `v2_businesses_match` (`POST /v2/businesses/match`) and the
`v2_businesses_*_enrich` family. Explorium recommends v2 for new work but warns
that v2 paths, fields and timing may change before GA, and v2 has **no webhooks
yet**. v1 and v2 run in parallel with no announced cutover.

## Errors

`401` missing/invalid key or partner ID · `403` out of credits or not entitled ·
`422` validation (read `detail[].loc`) · `429` over 200 queries/min, honour
`Retry-After` · `5xx` retry with backoff.

**No idempotency key exists.** A retry after a timeout is charged again.
