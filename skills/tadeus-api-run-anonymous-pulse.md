---
name: tadeus-api-run-anonymous-pulse
description: >-
  Run a Tadeus voice pulse with no participant data attached — create anonymous sessions
  directly, distribute the links yourself, and read back aggregate-only results. The variant
  that keeps a workforce deployment aggregate-only and outside Annex III point 4.
api: Tadeus Integration API
base_url: https://app.tadeus.net/api/integration/v1
generated: '2026-08-11'
method: generated
source:
  - openapi/tadeus-api-integration-openapi.json
  - https://tadeus.net/api-examples
  - https://tadeus.net/trust/how-tadeus-classifies-itself
operations:
  - templates_create
  - campaigns_create
  - sessions_create
  - sessions_list
  - sessions_read
  - results_list
  - attempts_list
  - campaigns_generate_insights
---

# Run an anonymous Tadeus pulse

Use this instead of `tadeus-api-run-interview-campaign` when you must not hold participant
identity — a works-council-sensitive pulse, a whistleblowing-adjacent topic, an exit-adjacent
survey, or any deployment you need to keep demonstrably aggregate-only.

All operationIds verified against `openapi/tadeus-api-integration-openapi.json`.

## The mechanism

`Session.email_capture_mode` is the lever. Setting it to `"none"` creates a session with no
participant attached: no email is stored and no invitation is sent. Tadeus' own framing is
that people "feel safely unwatched, so they speak more freely."

## Step 1 — template and campaign

Same as the standard flow (`templates_create`, then `campaigns_create`), with one change:
set `access_type` to `open` rather than `invite_only`, because you are not inviting anyone
by email. Still set `max_sessions` — with an open campaign it is the only thing standing
between a shared link and your entire credit balance.

## Step 2 — create anonymous sessions (`sessions_create`)

`POST /sessions/`

```json
{
  "campaign_uuid": "<campaign uuid>",
  "email_capture_mode": "none",
  "attempts_allowed": 1
}
```

Returns `201`:

```json
{
  "uuid": "...",
  "campaign_uuid": "...",
  "status": "not_started",
  "email_capture_mode": "none",
  "attempts_allowed": 1
}
```

- `attempts_allowed` caps how many times one session can be re-entered. Keep it at 1 for a
  pulse; raise it only when a dropped call should be resumable.
- `success_attempts_allowed` caps *successful* completions on the session.

Create sessions one per intended respondent and distribute the session links through your
own channel (intranet, kiosk, shift-handover screen). Tadeus never learns who is who.

**No idempotency key exists.** A retried `sessions_create` after a timeout creates a second
session and consumes a second slot against `max_sessions`. Call `sessions_list` filtered by
`campaign_uuid` and reconcile the `count` before creating more.

## Step 3 — monitor (`sessions_list`, `attempts_list`)

`GET /sessions/?campaign_uuid=<uuid>` and read `count` against your target n. Per-session
detail via `sessions_read`; per-conversation detail via `attempts_list` (state, success,
finish_reason, start_ts/end_ts).

Because there is no participant identity, `count` and `status` are the only progress signal
you get — which is the point.

## Step 4 — read results in aggregate only (`results_list`)

`GET /results/?campaign_uuid=<uuid>`

Results still carry `session_uuid`, so they are *pseudonymous*, not anonymous, at the row
level. To keep the deployment genuinely aggregate-only:

- aggregate before storage — do not persist per-`session_uuid` rows into any system that
  can be joined back to a roster;
- never join a session uuid to the distribution list you used in step 2;
- prefer `campaigns_generate_insights` and read `output_json_summary` rather than
  hand-rolling analysis over individual rows.

## Why this variant exists

Tadeus' published self-classification argues it sits outside EU AI Act Annex III point 4
because it produces aggregate output — themes and distributions — and feeds no decision
about a named individual. Its own guidance names the failure mode explicitly: drift, where
individual-level exports find their way into a performance dashboard and change the
deployment's classification whether or not anyone re-reads the paperwork.

This skill is the technical expression of that position. If your integration ever surfaces a
single respondent's result to their manager, you are no longer running this pattern.

Separately, from **2026-08-02** the Article 50 transparency duty applies: people must be told
they are dealing with an AI system, and the practical bar is a timestamped per-person record
of the disclosure. An anonymous session removes the person from your records — so confirm
the disclosure evidence sits on the Tadeus side of the boundary before you rely on it.

## Related

- `skills/tadeus-api-run-interview-campaign.md` — the named-cohort flow.
- `conformance/tadeus-api-conformance.yml`, `security/tadeus-api-trust-center.yml`.
- https://tadeus.net/trust/how-tadeus-classifies-itself
