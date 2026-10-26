# Explorium (explorium)

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

Explorium is a B2B data foundation for AI agents and go-to-market teams. Its **AgentSource API** is a single external-data and enrichment platform - one REST API plus a native MCP server - that resolves, fetches, enriches, and monitors a business dataset of 150M+ companies and a prospect dataset of 800M+ people aggregated from 100+ external sources. It answers "web intelligence" and "reference data" needs: firmographics, technographics, webstack and web-traffic signals, financials, funding, workforce trends, verified contact data, and real-time business/prospect events.

**Access model (honest):** The API is **gated but self-serve**. You create an Explorium account, obtain an API key at `https://admin.explorium.ai/api-key`, and send it in the `API_KEY` header on every call to `https://api.explorium.ai`. A **free developer tier of 100 credits (valid 90 days)** lets you evaluate before buying prepaid credit packages (reported to start around $200 for 5,000 credits) or an enterprise agreement. All API surfaces - match, fetch, stats, autocomplete, 30+ enrichment endpoints, and events - draw from a **single shared credit pool**. The developer documentation, the OpenAPI index (`/openapi.json`), and the MCP server source (MIT) are publicly readable.

The endpoint paths and methods in this catalog are **confirmed** from Explorium's published OpenAPI index and reference pages; request/response schemas in the OpenAPI file are **honestly modeled** at a representative level, and a representative subset of the 30+ enrichment endpoints is included.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/explorium/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/explorium/refs/heads/main/apis.yml)

## Tags

- Data Enrichment
- Web Intelligence
- Reference Data
- B2B Data
- Company Data
- AI Agents
- Prospect Enrichment
- Firmographics
- MCP

## Timestamps

- **Created:** 2026-07-11
- **Modified:** 2026-07-11

## APIs

### Explorium Business Enrichment API

Fetch, market-size (stats), and enrich company records across 150M+ businesses. Includes 17+ single and bulk enrichment endpoints - firmographics, technographics, webstack, website traffic and changes, company website keywords, financial indicators, funding and acquisition, company hierarchies, workforce trends, lookalikes, and Bombora intent - each returning structured reference data for a matched `business_id`.

- **Human URL:** [https://developers.explorium.ai/reference/businesses_api](https://developers.explorium.ai/reference/businesses_api)
- **Base URL:** `https://api.explorium.ai`

### Explorium Prospect Enrichment API

Fetch, market-size (stats), and enrich people records across 800M+ prospects filtered by job level, department, title, or employer. Single and bulk enrichment endpoints return verified contact information, professional profiles and work history, and LinkedIn posts for a matched `prospect_id`.

- **Human URL:** [https://developers.explorium.ai/reference/prospects_api](https://developers.explorium.ai/reference/prospects_api)
- **Base URL:** `https://api.explorium.ai`

### Explorium Matching API

High-accuracy entity resolution. Resolve up to 50 companies to `business_id` values from name, domain, URL, or LinkedIn URL, and resolve people to `prospect_id` values from name plus company, email, or LinkedIn URL. Matching is the first step before fetch and enrichment.

- **Human URL:** [https://developers.explorium.ai/reference/match_businesses](https://developers.explorium.ai/reference/match_businesses)
- **Base URL:** `https://api.explorium.ai`

### Explorium Autocomplete API

Fuzzy search over allowed filter values for businesses and prospects - industries, technologies, locations, job titles, departments - so clients can build valid filter payloads for fetch and stats calls.

- **Human URL:** [https://developers.explorium.ai/reference/businesses_api](https://developers.explorium.ai/reference/businesses_api)
- **Base URL:** `https://api.explorium.ai`

### Explorium Business and Prospect Events API

Real-time business and prospect signals - funding rounds, hiring surges, office moves, leadership changes, and job changes. Enroll businesses or prospects for monitoring, fetch events, and register webhooks that push notifications to an HTTPS endpoint.

- **Human URL:** [https://developers.explorium.ai/reference/businesses_events](https://developers.explorium.ai/reference/businesses_events)
- **Base URL:** `https://api.explorium.ai`

### Explorium AgentSource MCP Server

Native remote Model Context Protocol server exposing AgentSource data as 11 agent tools - match-business, fetch-businesses, fetch-businesses-statistics, fetch-businesses-events, enrich-business, match-prospects, fetch-prospects, fetch-prospects-statistics, fetch-prospects-events, enrich-prospects, and autocomplete. Available over streamable HTTP (`https://mcp.explorium.ai/mcp`) and SSE (`https://mcp.explorium.ai/sse`), and self-hostable via Docker (MIT). Works with Claude Desktop, Cursor, VS Code, n8n, and other MCP clients.

- **Human URL:** [https://developers.explorium.ai/mcp-docs/agentsource-mcp](https://developers.explorium.ai/mcp-docs/agentsource-mcp)
- **Base URL:** `https://mcp.explorium.ai/mcp`
- **Source:** [https://github.com/explorium-ai/mcp-explorium](https://github.com/explorium-ai/mcp-explorium)

## Authentication

API key in the `API_KEY` request header, issued at [https://admin.explorium.ai/api-key](https://admin.explorium.ai/api-key). See [authentication/explorium-authentication.yml](authentication/explorium-authentication.yml).

## Common Properties

- [GitHub Organization](https://github.com/explorium-ai)
- [LinkedIn](https://www.linkedin.com/company/explorium-ai)
- [Website](https://www.explorium.ai)
- [Documentation](https://developers.explorium.ai)
- [MCP](https://developers.explorium.ai/mcp-docs/agentsource-mcp)
- [Plans](plans/explorium-plans-pricing.yml)
- [Rate Limits](rate-limits/explorium-rate-limits.yml)
- [Fin Ops](finops/explorium-finops.yml)

## WebSocket Review

Explorium does **not** expose a documented public WebSocket API. Its real-time surfaces are poll-based event endpoints and outbound webhooks (plain HTTP), plus an MCP server over streamable-HTTP/SSE. See [review.yml](review.yml).

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
