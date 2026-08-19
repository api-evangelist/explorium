---
name: Monitor business and prospect events with Explorium
description: >-
  Register a signed webhook, enroll companies and people in event pipelines, and
  receive real-time signals — funding rounds, hiring, office moves, job changes.
  Use when an agent needs to react to change rather than poll for it.
api: openapi/explorium-webhooks-api-openapi.yml
operations:
  - add_webhook
  - check_webhook_connectivity
  - get_webhook
  - delete_webhook
  - add_businesses_enrollments
  - get_businesses_enrollments
  - delete_businesses_enrollments
  - add_prospects_enrollments
  - fetch_businesses_events
  - fetch_prospects_events
generated: '2026-08-14'
method: generated
source: >-
  openapi/_original/explorium-agentsource-openapi.json +
  https://developers.explorium.ai/reference/webhooks/webhooks
---

# Monitor business and prospect events with Explorium

Events are **v1 only**. Explorium's own published Agent Skill says v2 webhooks
are "coming before GA", so an integration built on v2 must still call v1 for
anything push-delivered.

## Decide: push or pull

- **Pull** — `fetch_businesses_events` (`POST /v1/businesses/events`) and
  `fetch_prospects_events` (`POST /v1/prospects/events`) return events
  synchronously. No webhook needed. Use this for backfill or a one-off sweep.
- **Push** — register a webhook and enroll entities. Use this for anything
  ongoing.

## Step 1 — register the webhook

`add_webhook` (`POST /v1/webhooks`) with:

```json
{"partner_id": "...", "webhook_url": "https://your-domain.com/webhook-handler",
 "headers": ["Authorization: Bearer your_token"], "payload_format": "json"}
```

`headers[]` is sent with **every** delivery, so your receiver can require its
own auth. `payload_format` is `json` (default) or `stringified_json` for
destinations that want a single text field.

The response returns a **`webhook_secret`**. Store it immediately — you cannot
retrieve it later.

**One webhook per partner ID.** Registering a second one silently overwrites the
first and rotates the secret. There is no fan-out and no way to add a
destination without losing the existing one.

## Step 2 — verify the signature on every delivery

Explorium signs with HMAC-SHA256 over a timestamped message:

1. base64url-**decode** `webhook_secret` to get the HMAC key.
2. Build the message `"{X-Timestamp}.{raw request body}"` — the *raw* bytes,
   before JSON parsing.
3. HMAC-SHA256 it, base64url-encode the digest, and compare to the `X-Signature`
   header using a **constant-time** comparison.
4. Reject anything where `|now - X-Timestamp| > 300` seconds.

Reject non-`application/json` content types with `415`. Reject a bad signature
with `400`. Never process an unverified body.

## Step 3 — test connectivity before enrolling

`check_webhook_connectivity` (`POST /v1/webhooks/check_connectivity`). Do this
before enrolling anything — a receiver that fails produces a `400`
"Client webhook failed on: [details]" per delivery, not a retry queue you can
inspect.

## Step 4 — enroll entities

`add_businesses_enrollments`
(`POST /v1/businesses/events/enrollments`) and
`add_prospects_enrollments` (`POST /v1/prospects/events/enrollments`).

Assign a distinct **`enrollment_key`** per use case. It is caller-assigned and
echoed back on every delivered event, and it is the only routing key you get —
all events for the account arrive at the same single URL.

Manage enrollments with `get_businesses_enrollments`,
`update_businesses_enrollments` and `delete_businesses_enrollments`.

## Step 5 — handle the event

Each delivery carries `enrollment_key`, `event_name` and `entity_id`. Route on
`enrollment_key`, branch on `event_name`, and resolve `entity_id` back to your
own record.

**Available event types**

*Companies (15):* new funding round · new investment · IPO announcement ·
merger and acquisitions · new partnership · new product launch · new office
opening · office closing · hiring by department · department workforce trends ·
new executive level hires · company award · lawsuits and legal proceedings ·
outages and security breaches · cost cutting.

*People (3):* employee changed workplace · employee changed role · employee's
workplace anniversary.

## Errors

`400` your receiver failed — check availability and response · `401` missing key
or partner ID · `403` out of credits · `429` over 200 queries/min ·
`5xx` retry with backoff. Event retrieval draws credits like any other call.
