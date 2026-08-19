---
name: Build a target account list with Explorium
description: >-
  Search the Explorium company dataset with firmographic and technographic
  filters, size the audience before spending on it, and page the results out.
  Use when an agent needs a list of companies matching an ICP.
api: openapi/explorium-businesses-api-openapi.yml
operations:
  - businesses_autocomplete
  - fetch_stats
  - fetch_businesses
generated: '2026-08-14'
method: generated
source: openapi/_original/explorium-agentsource-openapi.json + https://developers.explorium.ai/reference/pagination
---

# Build a target account list with Explorium

## Before you start

- Base URL is `https://api.explorium.ai`. Every request carries the `api_key`
  header (lowercase). The account's partner ID goes in `X-Context-Partner-ID`
  (or `partner-id` / `partner_id`) — a missing partner ID fails as **401**, not
  as a validation error.
- **Every call costs credits, and there is no test mode.** Check the balance
  first with `get_active_credits_summary` (`GET /v1/credits`).
- Rate limit is **200 queries per minute per key**, and a query is an *entity*,
  not a request. A response carrying 500 records is not one query.

## Step 1 — resolve your filter values

Do not guess filter values. `linkedin_category`, `google_category`,
`naics_category`, `company_tech_stack_tech` and `job_title` only accept
standardized values.

Call `businesses_autocomplete` (`GET /v1/businesses/autocomplete`) with the
field name and a query string, and use the returned values verbatim.

Never send more than **one** category type (`linkedin_category`,
`google_category`, `naics_category`) in a single request — Explorium documents
this as a conflict.

## Step 2 — size the audience before you buy it

Call `fetch_stats` (`POST /v1/businesses/stats`) with the filter set you intend
to use. This returns the distribution of the matching audience without paging
the records out.

If the count is far larger or smaller than expected, fix the filters here — this
is the cheap step. Broaden filters when results are sparse; coverage varies by
attribute.

## Step 3 — page the list out

Call `fetch_businesses` (`POST /v1/businesses`) with the same `filters` object.
Filters take the shape `{"field": {"values": [...]}}`.

Pick a pagination mode:

- **Offset** — `page` (1-based), `page_size` (max **500**), `size` (total cap,
  max **60,000**). The response carries `total_results`, `total_pages` and
  `page`. Use this when you need random page access.
- **Cursor** — pass `search_after` with the `next_cursor` from the previous
  response. No cap. Use this for anything large.

Watch `company_country_code` (the company's HQ) against `country_code` (a
prospect's location) — mixing them up is the most common filter bug on this API.

## Step 4 — read the outcome correctly

Every response wraps its payload in `response_context`:

```json
{"response_context": {"correlation_id": "...", "request_status": "success", "time_took_in_seconds": 0.45},
 "data": [ ... ], "total_results": 500, "page": 1, "total_pages": 5}
```

**Branch on `response_context.request_status`, not on the HTTP status.** A `200`
whose `request_status` is `miss` means nothing was found. Log `correlation_id`
on every call; it is the only identifier Explorium support will act on.

## Errors

| Status | Meaning | Do |
|---|---|---|
| 401 | Missing/invalid `api_key` **or** missing/invalid partner ID | Fix the credential; do not retry unchanged |
| 403 | Insufficient credits, or not entitled | Top up; retrying will never succeed |
| 422 | Validation failed | Read `detail[].loc` and `detail[].msg`; `page_size` ≤ 500, `size` ≤ 60000, `page` ≥ 1 |
| 429 | Over 200 queries/min | Honour `Retry-After`; back off exponentially |
| 5xx | Transient | Retry with exponential backoff |

**There is no idempotency key on this API.** A retried request after a timeout
is charged again. Prefer polling an async job over resubmitting a large call.

## Next

Feed the returned `business_id` values into
`explorium-enrich-companies-in-bulk.md`.
