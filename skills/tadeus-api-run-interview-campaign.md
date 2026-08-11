---
name: tadeus-api-run-interview-campaign
description: >-
  Run an end-to-end AI-moderated voice interview campaign against a named cohort with the
  Tadeus Integration API — author a template, create a campaign, invite participants
  individually or in bulk, poll for completion, and read back structured results with
  quality signals.
api: Tadeus Integration API
base_url: https://app.tadeus.net/api/integration/v1
generated: '2026-08-11'
method: generated
source:
  - openapi/tadeus-api-integration-openapi.json
  - https://tadeus.net/api-examples
operations:
  - templates_create
  - templates_read
  - campaigns_create
  - campaigns_read
  - campaigns_invite
  - campaigns_bulk_invite
  - sessions_list
  - results_list
  - results_read
  - organisation_read
---

# Run a Tadeus interview campaign

Every operationId below was verified against the Tadeus Swagger document in
`openapi/tadeus-api-integration-openapi.json`. Do not invent endpoints — the API has 47
operations and no others.

## Before you start

**Authentication.** Send both headers on every single request. They are not alternatives,
even though the spec declares them as two separate schemes:

```
X-API-KEY-ID: <key id>
X-API-SECRET: <key secret>
Content-Type: application/json
```

Both are generated in the Tadeus dashboard under **API access**. Load them from the
environment; never inline them.

**Base URL.** `https://app.tadeus.net/api/integration/v1`. Tadeus' own published examples
say `https://tadeus.net/api/integration/v1` — that host returns 404. Use `app.`.

**Trailing slashes are required.** Every path ends in `/`.

Sanity check with `organisation_read` (`GET /organisation/`) before doing anything that
costs money.

## Step 1 — author the template (`templates_create`)

`POST /templates/`

The template is the reusable part: it holds the interview prompt and the shape of the
structured data you want back.

```json
{
  "name": "Benefits enrolment check-in",
  "interview_prompt": "Interview an employee just after open enrolment. Confirm which plan they chose and whether anything was unclear. Ask one follow-up when an answer is vague.",
  "output_schema": {
    "plan_chosen": "string",
    "confusion_points": "string[]",
    "needs_followup": "boolean"
  },
  "status": "published"
}
```

Returns `201` with a `uuid`. `output_schema` is the contract for `Result.output_json`
later — get it right here and every downstream result is machine-readable without parsing
prose.

## Step 2 — create the campaign (`campaigns_create`)

`POST /campaigns/`

```json
{
  "name": "Q1 enrolment readiness",
  "template_uuid": "<template uuid>",
  "type": "survey",
  "access_type": "invite_only",
  "max_sessions": 5000
}
```

- `access_type`: `invite_only` for a named workforce, `open` for a public link.
- `max_sessions` is a hard safety cap. **Always set it.** There is no rate limiting on this
  API and no published quota error; `max_sessions` is your only backstop against a runaway
  campaign consuming the whole credit balance.

Returns `201` with a `uuid`.

## Step 3 — invite (`campaigns_invite` / `campaigns_bulk_invite`)

One person:

`POST /campaigns/{uuid}/invite/` with `{"email": "alex@company.com", "name": "Alex Doe"}`

A cohort:

`POST /campaigns/{uuid}/bulk-invite/` with `{"participants": [{"email": "..."}, ...]}`

**These are the highest-consequence calls in the API.** They send real invitations to real
people, and:

- there is **no idempotency key** anywhere in this API;
- there is **no documented rate limit** and no roster-size ceiling;
- there is **no documented 4xx response** on any operation.

So: on a timeout or a connection error, **do not blind-retry a bulk invite**. Call
`sessions_list` filtered by `campaign_uuid` first and reconcile against your roster before
sending anything again. A duplicate bulk invite is visible to the whole workforce.

## Step 4 — poll for completion (`sessions_list`)

`GET /sessions/?campaign_uuid=<uuid>&status=completed`

Response is page-number paginated:

```json
{ "count": 12, "next": null, "previous": null, "results": [ ... ] }
```

Follow `next` until it is `null`. Note that `campaign_uuid` and `status` work as filters —
Tadeus documents them — but they are **not declared in the Swagger document**, which only
declares `page`. Do not conclude from the spec that filtering is unavailable.

Poll on a sane interval. Voice conversations happen on the participant's own time, so a
campaign completes over hours or days, not seconds. There is no webhook and no completion
event, despite "Webhooks-ready" badging on the docs — nothing about a webhook surface is
published.

## Step 5 — read the structured results (`results_list`, `results_read`)

`GET /results/?campaign_uuid=<uuid>`

Each result carries:

- `short_summary` / `summary_text` — prose;
- `output_json` — your template's `output_schema`, instantiated. **This is the field to
  reason over**;
- `confidence`, `relevance`, `sentiment`, `engagement` — 0-1 signals about *the response*,
  derived from what was said;
- `validated` — boolean.

Weight low-`confidence` responses down rather than discarding them, and never treat these
signals as attributes of the *person*. Tadeus is explicit that they are properties of the
response and involve no voice-tone emotion inference — that distinction is what keeps a
workforce deployment clear of the EU AI Act Article 5(1)(f) prohibition on workplace emotion
recognition. Reporting them as an individual's score changes the compliance position of the
deployment.

## Error handling

The spec declares **no** error responses at all, so handle these empirically:

| Status | Body | Meaning |
|---|---|---|
| 403 | `{"detail":"Authentication credentials were not provided."}` | Missing key headers |
| 404 | `{"ok":false,"error":"not_found"}` | You hit `tadeus.net` instead of `app.tadeus.net` |

Anything else: read `detail` as prose. There is no machine-readable error code, no
`application/problem+json`, and no documented 429.

## Related

- `skills/tadeus-api-synthesize-campaign-insights.md` — roll a finished campaign into one
  synthesis.
- `skills/tadeus-api-run-anonymous-pulse.md` — the no-participant-data variant.
- `conventions/tadeus-api-conventions.yml`, `errors/tadeus-api-problem-types.yml`.
