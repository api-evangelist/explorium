---
name: Run a bulk enrichment job with Explorium
description: >-
  Enrich up to 10,000 records asynchronously — upload an entity-ID dataset,
  submit a job, poll it, and collect the results before they expire. Use when
  the record count is above the 50-record synchronous ceiling.
api: openapi/explorium-asyncjobs-api-openapi.yml
operations:
  - v2_entity_id_lists_upload
  - v2_businesses_firmographics_job
  - v2_jobs
  - v2_job_status
  - v2_job_cancel
  - v2_businesses_research_enrich
generated: '2026-08-14'
method: generated
source: >-
  openapi/_original/explorium-agentsource-openapi.json +
  https://developers.explorium.ai/.well-known/agent-skills/explorium/skill.md
---

# Run a bulk enrichment job with Explorium

Use sync for 1–50 records. Above that, use this pipeline. This is a **v2 (beta)**
capability — Explorium recommends v2 for new work but states its paths, fields
and timing may change before GA.

## Step 1 — check the budget first

Every record in the job costs a credit, and a 10,000-record job is a 10,000-credit
commitment. Call `get_active_credits_summary` (`GET /v1/credits`) before
submitting. A job that outruns the balance fails with `403` insufficient
credits — and **there is no idempotency key**, so a resubmit after a partial
failure is a fresh charge.

## Step 2 — upload the dataset

`v2_entity_id_lists_upload` (`POST /v2/async/entity-id-datasets/upload`).

The dataset is a list of entity IDs — `business_id` or `prospect_id` values you
already obtained from a match or fetch call. Raw domains and emails are not
accepted; resolve them first.

Returns a **`list_id`**.

**Cap: 10,000 rows per job.**

## Step 3 — submit the job

Post to the `*_job` operation for the enrichment family you want, with the
`list_id` and any filters. There are 22 of them. Examples:

| Family | Operation | Path |
|---|---|---|
| Firmographics | `v2_businesses_firmographics_job` | `POST /v2/businesses/firmographics/job` |
| Technographics | `v2_businesses_technographics_job` | `POST /v2/businesses/technographics/job` |
| Contact information | `v2_prospects_contact_information_job` | `POST /v2/prospects/contact_information/job` |
| AI research | `v2_businesses_research_job` | `POST /v2/businesses/research/job` |

Returns a **`job_id`**.

## Step 4 — poll

`v2_job_status` (`GET /v2/jobs/status/{job_id}`) returns the state
(queued / running / completed) and, on completion, a results URL.

`v2_jobs` (`GET /v2/jobs`) lists jobs if you have lost a `job_id`.
`v2_job_cancel` (`POST /v2/jobs/cancel/{job_id}`) stops one.

Poll with backoff — polling counts against the 200 queries/minute limit like any
other call.

**Runtime cap: 24 hours.**

## Step 5 — collect the results before they expire

Follow the results URL from the completed status response. Two separate
deadlines apply:

- The **results URL expires in about an hour**.
- The **results themselves are retained 7 days**.

Download to your own storage immediately. If the URL has expired but you are
inside the retention window, re-read the job status to get a fresh one.

## AI research (beta)

`v2_businesses_research_enrich` (`POST /v2/businesses/research/enrich`) and
`v2_prospects_research_enrich` run a natural-language research task over a list
of entities, grounded in real-time web data. Available synchronously or as a job
(`v2_businesses_research_job`). This capability has **no MCP tool** — it is
REST-only.

## Errors

`403` insufficient credits — the whole job stops · `422` validation on the
submit body · `429` throttled while polling, honour `Retry-After` · `5xx` retry
with backoff. Check `response_context.request_status` on every response;
`correlation_id` is required by support.
