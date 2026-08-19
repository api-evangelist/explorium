---
name: Explorium
description: Use when building AI agents, data pipelines, or integrations that need B2B business intelligence, prospect research, company enrichment, or real-time event monitoring. Reach for this skill when you need to match businesses/prospects, fetch filtered datasets, enrich records with firmographics/technographics/financial data, or integrate with Claude, n8n, Zapier, or custom applications.
metadata:
    mintlify-proj: explorium
    version: "1.0"
---

# Explorium AgentSource Skill

## Product Summary

Explorium AgentSource is a B2B data intelligence API that provides business matching, prospect research, enrichment, and event tracking across 80M+ companies and their employees. Agents use it to find and enrich company and contact data, monitor business events, and power AI-driven prospecting workflows. The API is available in two versions: v1 (stable) and v2 (beta, recommended for new work). Access via REST API at `https://api.explorium.ai` with your API key, or via Model Context Protocol (MCP) at `https://mcp.explorium.ai/mcp` for AI assistant integration. Primary docs: https://developers.explorium.ai

**Key endpoints:**
- Match: `/v2/businesses/match_businesses`, `/v2/prospects/match_prospects`
- Fetch: `/v2/businesses/fetch_businesses`, `/v2/prospects/fetch_prospects`
- Enrich: `/v2/businesses/enrichments/*`, `/v2/prospects/enrichments/*`
- Events: `/v2/endpoints/events`, `/v1/webhooks`
- Async jobs: `/v2/async/entity-id-datasets/upload`, `/v2/jobs/status/{job_id}`

**Authentication:** All requests require `api_key` header. Get your key from https://admin.explorium.ai under Access & Authentication.

## When to Use

Reach for this skill when:

- **Matching entities:** You need to resolve a business name/domain to a Business ID, or match a person to a Prospect ID
- **Searching datasets:** You need to find companies or people matching specific filters (industry, revenue, job title, location, technology stack, etc.)
- **Enriching records:** You have a list of Business or Prospect IDs and need to add firmographics, technographics, financial data, social media, or competitive intelligence
- **Monitoring events:** You need real-time notifications when companies get funding, hire, change offices, or when prospects change jobs
- **Building AI agents:** You're integrating with Claude, Cursor, n8n, or custom LLM applications via MCP
- **Bulk processing:** You need to enrich 10-10,000 records asynchronously
- **Prospecting workflows:** You're building lead generation, sales intelligence, or market research pipelines

Do not use this skill for: general web search, non-B2B data, personal consumer data, or operations that don't require verified business intelligence.

## Quick Reference

### API Versions

| Aspect | v1 (Stable) | v2 (Beta, Recommended) |
|--------|-----------|----------------------|
| Base path | `/v1/` | `/v2/` |
| Bulk routes | Separate `/bulk_enrich` endpoints | Single endpoint accepts 1 or list of IDs |
| Async | Limited | Every enrichment has sync + async pair |
| Research | Not available | AI-powered custom research endpoints |
| Webhooks | Available | Coming before GA |
| Status | Production | Beta, no hard cutover |

### Core Workflow: Match → Fetch/Enrich → Monitor

```
1. Match (get IDs)
   ├─ match-business: name + domain → business_id
   └─ match-prospects: email/phone/LinkedIn → prospect_id

2. Fetch or Enrich (get data)
   ├─ fetch-businesses: filter by industry, revenue, size, location
   ├─ fetch-prospects: filter by job title, department, company, location
   ├─ enrich-business: add firmographics, technographics, financials, etc.
   └─ enrich-prospects: add contact info, social media, professional profile

3. Monitor (optional)
   ├─ Enroll in events: subscribe to funding, hiring, job changes
   └─ Webhooks: receive real-time notifications
```

### Rate Limits & Queries

- **Limit:** 200 queries per minute per API key (sliding 60-second window)
- **Query counting:** Each entity in a batch counts as 1 query (not 1 per HTTP request)
  - Example: A single request with 50 businesses = 50 queries consumed
- **Response headers:** `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`, `Retry-After`
- **Retry strategy:** Implement exponential backoff; respect `Retry-After` header on 429 responses

### Enrichment Types (v2)

**Business enrichments (17 total):**
Firmographics, Technographics, LinkedIn Posts, Company Ratings, Website Keywords, Financial Indicators, Funding & Acquisition, Business Challenges, Competitive Landscape, Strategy, Website Changes, Workforce Trends, Lookalikes, Webstack, Company Hierarchies, Website Traffic, Bombora Intent

**Prospect enrichments (3 total):**
Contact Information, LinkedIn Posts, Professional Profiles

### Async Job Workflow

```
1. Upload dataset: POST /v2/async/entity-id-datasets/upload
   → Returns list_id

2. Submit job: POST /v2/businesses/enrichments/{enrichment}/job
   Body: { "list_id": "...", "filters": {...} }
   → Returns job_id

3. Poll status: GET /v2/jobs/status/{job_id}
   → Returns state (queued/running/completed), results URL

4. Download results: Follow results URL (expires ~1 hour)
   → CSV or JSON with enriched data
```

**Limits:** 10,000 records per job, 24-hour runtime, 7-day result retention.

### MCP Tools (11 available)

| Tool | Category | Use Case |
|------|----------|----------|
| `match-business` | Business | Resolve company name/domain to Business ID |
| `fetch-businesses` | Business | Find companies by filters (industry, revenue, size, location, tech) |
| `fetch-businesses-statistics` | Business | Aggregate insights by industry, revenue, employee count |
| `fetch-businesses-events` | Business | Get funding rounds, office changes, partnerships, M&A |
| `enrich-business` | Business | Add firmographics, technographics, financials to Business ID |
| `match-prospects` | Prospects | Resolve email/phone/LinkedIn to Prospect ID |
| `fetch-prospects` | Prospects | Find people by job title, department, company, location |
| `fetch-prospects-statistics` | Prospects | Aggregate insights by department, location |
| `fetch-prospects-events` | Prospects | Get job changes, company changes, anniversaries |
| `enrich-prospects` | Prospects | Add contact info, social media, professional profile |
| `autocomplete` | Shared | Get valid values for filters (category, industry, tech stack, job title) |

## Decision Guidance

### When to Use Sync vs. Async

| Scenario | Use Sync | Use Async |
|----------|----------|----------|
| Single record enrichment | ✓ | |
| 1-50 records in one request | ✓ | |
| 51-10,000 records | | ✓ |
| Need results immediately | ✓ | |
| Can wait minutes/hours | | ✓ |
| Real-time agent response | ✓ | |
| Batch data pipeline | | ✓ |

### When to Use v1 vs. v2

| Aspect | Use v1 | Use v2 |
|--------|--------|--------|
| Webhooks/Events | ✓ | (coming soon) |
| New integrations | | ✓ |
| Async enrichments | Limited | ✓ |
| Custom research (GenAI) | | ✓ |
| Stable production | ✓ | |
| Consistent API surface | | ✓ |

### When to Use MCP vs. REST API

| Scenario | Use MCP | Use REST |
|----------|---------|----------|
| Claude Desktop/Cursor | ✓ | |
| n8n/Zapier integration | ✓ | |
| Custom Python/Node agent | ✓ | ✓ |
| Direct HTTP calls | | ✓ |
| Need full control over filters | ✓ | ✓ |
| Conversational AI | ✓ | |

## Workflow

### 1. Set Up Authentication

- Log in to https://admin.explorium.ai
- Navigate to Access & Authentication > Getting Your API Key
- Copy your API key
- Store securely (environment variable, secrets manager, vault — never hardcode)
- Test with a simple API call (see examples in docs)

### 2. Understand Your Data Need

Ask: Do I need to **match** entities, **fetch** a filtered list, **enrich** existing records, or **monitor** events?

- **Match:** You have a company name/domain or person's email and need their Explorium ID
- **Fetch:** You want to search for companies/people matching specific criteria
- **Enrich:** You have IDs and want to add data attributes
- **Monitor:** You want real-time notifications when something changes

### 3. Choose Sync or Async

- **Sync:** 1-50 records, need results in seconds, use for real-time agents
- **Async:** 51-10,000 records, can wait, use for batch pipelines

### 4. Build Your Request

**For sync (REST API):**
```
POST /v2/businesses/match_businesses
Headers: api_key: YOUR_KEY
Body: { "businesses_to_match": [{ "name": "...", "domain": "..." }] }
```

**For async:**
1. Upload CSV: `POST /v2/async/entity-id-datasets/upload`
2. Submit job: `POST /v2/businesses/enrichments/{type}/job`
3. Poll: `GET /v2/jobs/status/{job_id}`

**For MCP (AI agents):**
```python
# Tools are auto-discovered; call them by name
# Example: match-business, fetch-prospects, enrich-business
```

### 5. Handle Responses

- Check `response_context.request_status` (success/miss/failure)
- Use `correlation_id` for debugging
- For paginated results, use `page` and `page_size` or cursor-based pagination
- For async, poll until `state` is `completed`, then download results

### 6. Monitor Usage

- Check rate limit headers after each request
- Track credit consumption in Admin Portal > Consumption Report
- Implement preemptive throttling when `X-RateLimit-Remaining < 10`
- Monitor for 429 responses and implement exponential backoff

### 7. Set Up Events (Optional)

- Enroll businesses/prospects: `POST /v1/businesses/events/add_businesses_enrollments`
- Register webhook: `POST /v1/webhooks` with your endpoint URL
- Receive real-time notifications when events trigger

## Common Gotchas

- **Rate limit confusion:** A single request with 50 entities = 50 queries, not 1. Each entity counts separately.
- **Null business_id:** If match returns null, the business wasn't found. Try with different identifiers (name + domain together is most reliable).
- **Filter coverage varies:** Some filters have low data coverage. If results are sparse, broaden your filters.
- **Category filter conflict:** Never use more than one category type (linkedin_category, google_category, naics_category) in a single request.
- **Location filter confusion:** `company_country_code` = company HQ location; `country_code` = prospect's location. Don't mix them.
- **Autocomplete required:** Before using filters like `linkedin_category`, `google_category`, `naics_category`, `company_tech_stack_tech`, or `job_title`, call `autocomplete` first to get valid values.
- **Async dataset limits:** Max 10,000 rows per job; 24-hour runtime; results expire after 7 days.
- **Webhook secret:** Store the `webhook_secret` returned when creating a webhook; use it to verify incoming event signatures.
- **v2 beta changes:** Paths, fields, and timing may change before GA. v1 runs in parallel; migrate at your own pace.
- **Hardcoded API keys:** Never commit API keys to repos. Use environment variables or secrets managers.
- **Pagination max:** Offset-based pagination supports up to 60,000 records; use cursor-based for larger datasets.
- **Bulk endpoint counting:** Bulk requests count each entity toward rate limit, not the HTTP request itself.

## Verification Checklist

Before submitting work with Explorium:

- [ ] API key is stored securely (not hardcoded, not in version control)
- [ ] Authentication test passed (simple API call returns 200)
- [ ] Rate limit headers are being monitored and logged
- [ ] Retry logic with exponential backoff is implemented for 429 responses
- [ ] For match requests: using both name and domain when possible for higher accuracy
- [ ] For fetch requests: filters are valid (use autocomplete for category/tech/job_title filters)
- [ ] For async jobs: dataset is ≤10,000 rows; polling logic handles all job states
- [ ] For enrichments: Business/Prospect IDs are obtained before enriching
- [ ] For events: webhook URL is registered and tested with `check_webhook_connectivity`
- [ ] Error handling covers 401 (invalid key), 429 (rate limit), 422 (validation), and network errors
- [ ] Correlation IDs are logged for debugging
- [ ] Credit consumption is tracked and within budget
- [ ] For MCP: session context is active before loading tools
- [ ] For MCP: using Claude Sonnet models (recommended) or tested alternatives

## Resources

**Comprehensive navigation:** https://developers.explorium.ai/llms.txt

**Critical documentation pages:**
1. [API Overview & Quick Start](https://developers.explorium.ai/reference/quick-starts/introduction) — Core concepts, best practices, workflow overview
2. [AgentSource MCP](https://developers.explorium.ai/mcp-docs/agentsource-mcp) — MCP tools, integration options, usage notes
3. [Rate Limits & Error Handling](https://developers.explorium.ai/reference/rate-limit) — Query counting, retry logic, monitoring

**Implementation guides:**
- [LangGraph Integration](https://developers.explorium.ai/mcp-docs/implement-with-langgraph) — Build AI agents with async MCP
- [OpenAI Integration](https://developers.explorium.ai/mcp-docs/implement-with-openai) — Use with OpenAI models
- [Python SDK](https://developers.explorium.ai/mcp-docs/python-sdk-implementation-guide) — Direct Python implementation

**Platform integrations:**
- [n8n](https://developers.explorium.ai/integrations/n8n/integrate-with-n8n) — Visual workflow automation
- [Zapier](https://developers.explorium.ai/integrations/zapier/integrate-with-zapier) — No-code automation
- [Claude Desktop](https://developers.explorium.ai/integrations/claude/claude-desktop-integration) — Native MCP support
- [Cursor IDE](https://developers.explorium.ai/integrations/cursor/cursor-integration) — AI-powered coding

**API reference:**
- [v2 Businesses](https://developers.explorium.ai/v2/businesses/match_businesses) — Match, fetch, enrich companies
- [v2 Prospects](https://developers.explorium.ai/v2/prospects/match_prospects) — Match, fetch, enrich people
- [v2 Async Jobs](https://developers.explorium.ai/v2/async-jobs) — Batch processing infrastructure
- [v1 Webhooks](https://developers.explorium.ai/reference/webhooks/webhooks) — Event delivery (v2 coming soon)

---

> For additional documentation and navigation, see: https://developers.explorium.ai/llms.txt