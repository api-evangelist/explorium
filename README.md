# Explorium

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

Explorium is the B2B data layer for AI agents and go-to-market systems. Its AgentSource platform is one external-data and enrichment API plus a hosted MCP server, resolving, fetching, enriching and monitoring a company dataset and a people dataset aggregated from 100+ external sources. The REST API at https://api.explorium.ai publishes an anonymous OpenAPI 3.1 description covering 127 operations across two parallel surfaces - v1 (stable) and v2 (beta) - spanning entity matching, filtered fetch with cursor pagination, market-sizing statistics, autocomplete, seventeen company enrichment families and three people enrichment families (single, bulk to 50, and asynchronous to 10,000), AI-powered research, eighteen business and prospect event types delivered over HMAC-signed webhooks, and a shared prepaid credit pool that meters every call. Authentication is an api_key header on REST and OAuth 2.0 on MCP. Explorium is unusually agent-forward at the discovery layer, serving an RFC 9727 api-catalog, a conformant A2A agent card, an MCP server card, an ARD capability catalog, an agent-skills index and llms.txt from its own domain.

**Areas:** Data Enrichment · B2B Data · Company Data · Prospect Enrichment · Firmographics · Technographics · Web Intelligence · Reference Data · AI Agents · MCP · Agent Readiness · Sales Intelligence

## Explorium on APIs.io

Everything profiled for Explorium is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/explorium/](https://apis.io/providers/explorium/)** — or go straight to one of the APIs or properties below.

### APIs (22)

- [Explorium AgentSource MCP Server](https://apis.io/apis/explorium/explorium-agentsource-mcp-server/) — Native remote Model Context Protocol server exposing the AgentSource data as 11 agent tools - match-business, fetch-businesses, fetch-businesses-…
- [Explorium Businesses API](https://apis.io/apis/explorium/explorium-businesses-api/) — Match, fetch, stat, autocomplete, and event/enrollment operations over the Explorium business dataset (v1).
- [Explorium Credits API](https://apis.io/apis/explorium/explorium-credits-api/) — Inspect the shared credit pool that meters all Explorium API usage, including the credit menu and consumption aggregation (v1).
- [Explorium Prospects API](https://apis.io/apis/explorium/explorium-prospects-api/) — Match, fetch, stat, autocomplete, and event/enrollment operations over the Explorium prospect dataset (v1).
- [Explorium Webhooks API](https://apis.io/apis/explorium/explorium-webhooks-api/) — Register, retrieve, delete, and connectivity-test the webhook endpoints that receive Explorium business and prospect event notifications (v1).
- [Explorium Async Enrichment Jobs API](https://apis.io/apis/explorium/explorium-asyncenrichmentjobs-api/) — Submit any v2 company or people enrichment — plus AI research — as an asynchronous job over an uploaded entity-ID dataset.
- [Explorium Async Jobs API](https://apis.io/apis/explorium/explorium-asyncjobs-api/) — List, poll, and cancel the asynchronous enrichment jobs that back batch runs of up to 10,000 records (v2).
- [Explorium Bulk Business Enrichments API](https://apis.io/apis/explorium/explorium-bulkbusinessenrichments-api/) — Bulk (multi-record) versions of every v1 company enrichment, processing up to 50 business IDs per request.
- [Explorium Business Enrichments API](https://apis.io/apis/explorium/explorium-businessenrichments-api/) — Single-record company enrichments (v1): firmographics, technographics, webstack, website traffic and changes, financial indicators, funding and acquisition,…
- [Explorium Custom Enrichments API](https://apis.io/apis/explorium/explorium-customenrichments-api/) — Run a customer-specific custom enrichment, single-record or in bulk, by custom enrichment ID.
- [Explorium Entity ID Datasets API](https://apis.io/apis/explorium/explorium-entityiddatasets-api/) — Upload an entity-ID dataset that asynchronous v2 enrichment jobs run against.
- [Explorium Prospects Bulk Enrichments API](https://apis.io/apis/explorium/explorium-prospectsbulkenrichments-api/) — Bulk (multi-record) versions of the v1 people enrichments, processing up to 50 prospect IDs per request.
- [Explorium Prospects Enrichments API](https://apis.io/apis/explorium/explorium-prospectsenrichments-api/) — Single-record people enrichments (v1): contact information, professional profiles, and LinkedIn posts for a matched prospect.
- [Explorium V2 Autocomplete API](https://apis.io/apis/explorium/explorium-v2autocomplete-api/) — Resolve free-text input to the standardized filter values the v2 search API accepts.
- [Explorium V2 Business Enrichments API](https://apis.io/apis/explorium/explorium-v2businessenrichments-api/) — Synchronous v2 company enrichments — the same seventeen attribute families as v1, on the unified v2 request shape.
- [Explorium V2 Businesses API](https://apis.io/apis/explorium/explorium-v2businesses-api/) — Match, fetch, and stat over the company dataset on the v2 (beta) surface, where bulk and single input share one endpoint.
- [Explorium V2 Credits API](https://apis.io/apis/explorium/explorium-v2credits-api/) — Credit balance and consumption aggregation on the v2 (beta) surface.
- [Explorium V2 Prospect Enrichments API](https://apis.io/apis/explorium/explorium-v2prospectenrichments-api/) — Synchronous v2 people enrichments: contact information, professional profiles, and LinkedIn posts.
- [Explorium V2 Prospects API](https://apis.io/apis/explorium/explorium-v2prospects-api/) — Match, fetch, and stat over the people dataset on the v2 (beta) surface.
- [Explorium V2 Research API](https://apis.io/apis/explorium/explorium-v2research-api/) — Run a custom, AI-powered research task over a list of businesses or prospects using a natural-language query or prompt template, grounded in real-time web…
- [Explorium Version API](https://apis.io/apis/explorium/explorium-version-api/) — Report the deployed version of the Explorium AgentSource service.
- [Explorium Health Check API](https://apis.io/apis/explorium/explorium-health-check-api/) — The Health Check API from Explorium — 2 operation(s) for health check.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Explorium Webhooks](https://apis.io/asyncapis/explorium/explorium-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [mcp](https://apis.io/servers/explorium/mcp/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Explorium Agentic Access](https://apis.io/agentic-access/explorium/explorium-agentic-access/)

#### Collections (8)

Postman and Open Collection request collections.

- [Explorium AgentSource API](https://apis.io/collections/explorium/open-explorium/)
- [Explorium AgentSource Business Enrichments API](https://apis.io/collections/explorium/open-explorium-business-enrichments-api/)
- [Explorium AgentSource Business Enrichments Businesses API](https://apis.io/collections/explorium/open-explorium-businesses-api/)
- [Explorium AgentSource Business Enrichments Credits API](https://apis.io/collections/explorium/open-explorium-credits-api/)
- [Explorium AgentSource Business Enrichments Events API](https://apis.io/collections/explorium/open-explorium-events-api/)
- [Explorium AgentSource Business Enrichments Prospect Enrichments API](https://apis.io/collections/explorium/open-explorium-prospect-enrichments-api/)
- [Explorium AgentSource Business Enrichments Prospects API](https://apis.io/collections/explorium/open-explorium-prospects-api/)
- [Explorium AgentSource Business Enrichments Webhooks API](https://apis.io/collections/explorium/open-explorium-webhooks-api/)

#### Overlays (22)

OpenAPI overlays, including conversational phrasing for each operation.

- [Explorium Agentsource Overlay](https://apis.io/overlays/explorium/explorium-agentsource-overlay/)
- [Explorium Asyncenrichmentjobs Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-asyncenrichmentjobs-api-phrasing-overlay/)
- [Explorium Asyncjobs Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-asyncjobs-api-phrasing-overlay/)
- [Explorium Bulkbusinessenrichments Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-bulkbusinessenrichments-api-phrasing-overlay/)
- [Explorium Businessenrichments Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-businessenrichments-api-phrasing-overlay/)
- [Explorium Businesses Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-businesses-api-phrasing-overlay/)
- [Explorium Credits Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-credits-api-phrasing-overlay/)
- [Explorium Customenrichments Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-customenrichments-api-phrasing-overlay/)
- [Explorium Entityiddatasets Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-entityiddatasets-api-phrasing-overlay/)
- [Explorium Health Check Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-health-check-api-phrasing-overlay/)
- [Explorium Prospects Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-prospects-api-phrasing-overlay/)
- [Explorium Prospectsbulkenrichments Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-prospectsbulkenrichments-api-phrasing-overlay/)
- [Explorium Prospectsenrichments Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-prospectsenrichments-api-phrasing-overlay/)
- [Explorium V2Autocomplete Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-v2autocomplete-api-phrasing-overlay/)
- [Explorium V2Businessenrichments Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-v2businessenrichments-api-phrasing-overlay/)
- [Explorium V2Businesses Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-v2businesses-api-phrasing-overlay/)
- [Explorium V2Credits Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-v2credits-api-phrasing-overlay/)
- [Explorium V2Prospectenrichments Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-v2prospectenrichments-api-phrasing-overlay/)
- [Explorium V2Prospects Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-v2prospects-api-phrasing-overlay/)
- [Explorium V2Research Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-v2research-api-phrasing-overlay/)
- [Explorium Version Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-version-api-phrasing-overlay/)
- [Explorium Webhooks Api Phrasing Overlay](https://apis.io/overlays/explorium/explorium-webhooks-api-phrasing-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Explorium Authentication](https://apis.io/security/explorium/explorium-authentication/)
- [Explorium Domain Security](https://apis.io/security/explorium/explorium-domain-security/)
- [Explorium Trust Center](https://apis.io/security/explorium/explorium-trust-center/)
- [Explorium Vulnerability Disclosure](https://apis.io/security/explorium/explorium-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Explorium Plans Pricing](https://apis.io/plans/explorium/explorium-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Explorium Rate Limits](https://apis.io/rate-limits/explorium/explorium-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Explorium Finops](https://apis.io/finops/explorium/explorium-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.explorium.ai)
- [Documentation](https://developers.explorium.ai)
- [Getting started](https://developers.explorium.ai/reference/quick-starts/quick-starts)
- [API reference](https://developers.explorium.ai/reference/quick-starts/introduction)
- [Pricing](https://www.explorium.ai/pricing/)
- [Sign up](https://www.explorium.ai/sign-up/)
- [Support](https://developers.explorium.ai/reference/support-help-center)
- [Blog](https://www.explorium.ai/blog/news-and-updates/)
- [Compliance](https://www.explorium.ai/data-security/)
- [Terms of service](https://www.explorium.ai/explorium-website-terms-of-use/)
- [Privacy policy](https://www.explorium.ai/privacy-policy/)
- [GitHub](https://github.com/explorium-ai)

## API Evangelist rating

**Kin Score 65.0** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 54.7 |
| Contract governance | 18.2 |
| Operational transparency | 57.9 |
| Developer ergonomics | 71.4 |
| Access clarity | 92.1 |

**Agent readiness 58.6** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | documented (partial credit) |
| MCP Server | documented (partial credit) |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | yes |
| Well Known Catalog | yes |
| Consent Identity | — |
| Agent Card | conformant |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 29.4** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/explorium/](https://apis.io/providers/explorium/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Explorium: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/explorium/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
