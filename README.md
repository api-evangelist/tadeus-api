# Tadeus API

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

Tadeus is an AI voice agent platform for workforce platforms: one agent holds real, adaptive voice conversations with every employee at scale, in their own language, and returns structured, timestamped records to the HCM, payroll, scheduling and compliance systems people already use. It works from the transcript rather than the audio and reports comprehension and engagement signals about responses rather than inferring emotion, which is the position its published EU AI Act documentation rests on. The public Integration API is a 47-operation REST surface for creating interview templates and campaigns, inviting participants individually or in bulk, creating anonymous sessions, and retrieving transcripts, structured results with per-response quality signals, and cross-session AI insights. Tadeus went generally available on 14 July 2026 and also ships a hosted, first-party MCP server so agents can commission interviews and query results as tool calls.

**Areas:** Voice AI · Research · Interviews · Workforce · Human Resources · Conversational AI · Employee Experience · AI Agents · MCP · EU AI Act · Compliance · Employee Engagement

## Tadeus API on APIs.io

Everything profiled for Tadeus API is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/tadeus-api/](https://apis.io/providers/tadeus-api/)** — or go straight to one of the APIs or properties below.

### APIs (9)

- [Tadeus API Attempts API](https://apis.io/apis/tadeus-api/tadeus-api-attempts-api/) — The attempts API from Tadeus API — 2 operation(s) for attempts.
- [Tadeus API Billing API](https://apis.io/apis/tadeus-api/tadeus-api-billing-api/) — The billing API from Tadeus API — 6 operation(s) for billing.
- [Tadeus API Campaigns API](https://apis.io/apis/tadeus-api/tadeus-api-campaigns-api/) — The campaigns API from Tadeus API — 5 operation(s) for campaigns.
- [Tadeus API Insights API](https://apis.io/apis/tadeus-api/tadeus-api-insights-api/) — The insights API from Tadeus API — 6 operation(s) for insights.
- [Tadeus API Organisation API](https://apis.io/apis/tadeus-api/tadeus-api-organisation-api/) — The organisation API from Tadeus API — 2 operation(s) for organisation.
- [Tadeus API Results API](https://apis.io/apis/tadeus-api/tadeus-api-results-api/) — The results API from Tadeus API — 4 operation(s) for results.
- [Tadeus API Sessions API](https://apis.io/apis/tadeus-api/tadeus-api-sessions-api/) — The sessions API from Tadeus API — 2 operation(s) for sessions.
- [Tadeus API Templates API](https://apis.io/apis/tadeus-api/tadeus-api-templates-api/) — The templates API from Tadeus API — 2 operation(s) for templates.
- [Tadeus API Transcripts API](https://apis.io/apis/tadeus-api/tadeus-api-transcripts-api/) — The transcripts API from Tadeus API — 2 operation(s) for transcripts.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [Tadeus MCP](https://apis.io/servers/tadeus-api/tadeus-mcp/)

#### Collections (1)

Postman and Open Collection request collections.

- [Integration API](https://apis.io/collections/tadeus-api/open-tadeus-api-integration/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Tadeus Api Integration Overlay](https://apis.io/overlays/tadeus-api/tadeus-api-integration-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Tadeus Api Authentication](https://apis.io/security/tadeus-api/tadeus-api-authentication/)
- [Tadeus Api Domain Security](https://apis.io/security/tadeus-api/tadeus-api-domain-security/)
- [Tadeus Api Trust Center](https://apis.io/security/tadeus-api/tadeus-api-trust-center/)
- [Tadeus Api Vulnerability Disclosure](https://apis.io/security/tadeus-api/tadeus-api-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Tadeus Api Plans Pricing](https://apis.io/plans/tadeus-api/tadeus-api-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Tadeus Api Rate Limits](https://apis.io/rate-limits/tadeus-api/tadeus-api-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://tadeus.net/)
- [Documentation](https://app.tadeus.net/api/integration/v1/swagger/)
- [Getting started](https://tadeus.net/api-examples)
- [API reference](https://app.tadeus.net/api/integration/v1/swagger/)
- [Pricing](https://tadeus.net/#pricing)
- [Sign up](https://app.tadeus.net/signup)
- [Support](https://tadeus.net/contact)
- [Blog](https://tadeus.net/blog)
- [Terms of service](https://tadeus.net/terms)
- [Privacy policy](https://tadeus.net/privacy)

## API Evangelist rating

**Kin Score 46.8** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 70.0 |
| Contract quality | 43.2 |
| Contract governance | 18.2 |
| Operational transparency | 0.0 |
| Developer ergonomics | 49.4 |
| Access clarity | 80.3 |

**Agent readiness 28.7** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | verified |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 29.4** — Employment & Payroll

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/tadeus-api/](https://apis.io/providers/tadeus-api/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Tadeus API: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/tadeus-api/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
