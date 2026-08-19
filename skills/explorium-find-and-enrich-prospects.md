---
name: Find and enrich prospects with Explorium
description: >-
  Search the Explorium people dataset by role and employer, resolve people to
  prospect IDs, and attach contact details and professional profiles. Use when
  an agent needs contactable people at a set of companies.
api: openapi/explorium-prospects-api-openapi.yml
operations:
  - prospects_autocomplete
  - prospect_fetch_stats
  - fetch_prospects
  - match_prospects
  - prospects_contacts_information_enrich
  - prospects_contacts_information_bulk_enrich
  - prospects_profiles_enrich
generated: '2026-08-14'
method: generated
source: openapi/_original/explorium-agentsource-openapi.json
---

# Find and enrich prospects with Explorium

This skill handles personal data about identifiable people. Treat every output
as regulated: Explorium asserts GDPR and CCPA compliance and publishes a
"do not sell my personal info" path, but the obligations on the *caller* are the
caller's own. Do not enrich contact details you have no lawful basis to process.

## Two ways in

**You know who you want** → `match_prospects` (`POST /v1/prospects/match`), up
to 50 records, keyed on email, or full name plus company. Returns an ordered
list of `prospect_id` values with nulls for misses.

**You want to discover people** → `fetch_prospects`
(`POST /v1/prospects`) with filters on role, seniority, department, skills and
employer attributes.

## Step 1 — resolve filter values

`job_title` will not accept free text. Call `prospects_autocomplete`
(`GET /v1/prospects/autocomplete`) with the field name and query, and use the
returned values verbatim.

Note the two country filters are different fields: `country_code` is the
**prospect's** location; `company_country_code` is the **employer's** HQ.

## Step 2 — size before you spend

`prospect_fetch_stats` (`POST /v1/prospects/stats`) returns the distribution of
the matching audience without paging records out. Fix filters here.

## Step 3 — page results

`fetch_prospects` supports the same two pagination modes as the business
dataset: offset (`page`, `page_size` max 500, `size` max 60,000) or cursor
(`search_after` / `next_cursor`, uncapped). Use cursor for anything large.

## Step 4 — enrich

Three families exist:

| Need | Operation | Path |
|---|---|---|
| Email and phone | `prospects_contacts_information_enrich` | `POST /v1/prospects/contacts_information/enrich` |
| Role, workplace, career history | `prospects_profiles_enrich` | `POST /v1/prospects/profiles/enrich` |
| Recent LinkedIn activity | `prospects_linkedin_posts_enrich` | `POST /v1/prospects/linkedin_posts/enrich` |

For batches use the bulk twin, e.g.
`prospects_contacts_information_bulk_enrich`
(`POST /v1/prospects/contacts_information/bulk_enrich`), up to 50 prospect IDs.
Each record is one query and one credit regardless of batching.

## Step 5 — read the outcome

Check `response_context.request_status` per response — `miss` is a `200`.
Contact coverage is the sparsest data on the platform; expect misses and do not
retry them, because the retry is charged and the answer will not change.

## Errors

`401` missing/invalid key or partner ID · `403` out of credits or not entitled ·
`422` validation · `429` over 200 queries/min, honour `Retry-After` · `5xx`
retry with backoff. No idempotency key exists; a timeout retry is charged again.

## Next

To be notified when these people change jobs, see
`explorium-monitor-business-events.md` — prospect events cover role changes,
company moves and workplace anniversaries.
