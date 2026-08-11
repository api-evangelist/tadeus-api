---
name: tadeus-api-synthesize-campaign-insights
description: >-
  Roll every result in a finished Tadeus campaign into a single cross-session synthesis and
  export it as Markdown, HTML, PDF or JSON for a steering committee — including how to know
  when the asynchronous job has actually finished.
api: Tadeus Integration API
base_url: https://app.tadeus.net/api/integration/v1
generated: '2026-08-11'
method: generated
source:
  - openapi/tadeus-api-integration-openapi.json
  - https://tadeus.net/api-examples
operations:
  - campaigns_generate_insights
  - insights_list
  - insights_read
  - insights_markdown
  - insights_pdf
  - insights_html
  - insights_lexical
  - results_export_export_csv
  - results_export_export_json
  - transcripts_list
  - transcripts_read
---

# Synthesise a Tadeus campaign into one insight

Runs after `tadeus-api-run-interview-campaign`. All operationIds verified against
`openapi/tadeus-api-integration-openapi.json`.

## Step 1 — confirm the campaign is actually done

Do not synthesise a half-finished campaign. Call `sessions_list` with
`?campaign_uuid=<uuid>&status=completed` and compare `count` against your invited roster,
walking `next` to the end. Synthesis consumes processing minutes, and re-running it after
more responses land costs again.

## Step 2 — kick off the synthesis (`campaigns_generate_insights`)

`POST /campaigns/{uuid}/generate-insights/`

Returns `201`. This is a **billed asynchronous job**, and the response gives you nothing to
track it with — no job id, no status field, no completion signal, no webhook. That is a real
gap in the contract, not something you are missing.

**Do not fire this twice.** There is no idempotency key. A duplicate POST is a second billed
synthesis.

## Step 3 — poll for the insight (`insights_list`)

`GET /insights/?campaign_uuid=<uuid>`

Poll with backoff until a new `uuid` appears whose `created_at` is later than the moment you
posted in step 2. That timestamp comparison is the only reliable completion test available.

Note: `insights_list` returns the `InsightList` projection, which carries only `uuid` and
`created_at`. Fetch the body separately.

## Step 4 — read it (`insights_read`)

`GET /insights/{uuid}/`

Returns:

- `executive_summary` — the top-line;
- `content` — the full synthesis;
- `output_json_summary` — structured rollup across the campaign's `output_json` values.
  Prefer this when an agent needs to reason rather than quote.

## Step 5 — export in the format the audience needs

| Operation | Path | Use |
|---|---|---|
| `insights_markdown` | `GET /insights/{uuid}/md/` | Agent consumption, docs, chat |
| `insights_html` | `GET /insights/{uuid}/html/` | Embed in a portal |
| `insights_pdf` | `GET /insights/{uuid}/pdf/` | Steering committee, works council pack |
| `insights_lexical` | `GET /insights/{uuid}/lexical/` | Editable rich-text (Lexical) |

These four return non-JSON bodies even though the spec's global `produces` says
`application/json`. Read `.text` / raw bytes, not `.json()`.

For the underlying rows rather than the synthesis:

- `results_export_export_csv` — `GET /results/export/csv/`
- `results_export_export_json` — `GET /results/export/json/`

## Step 6 — supporting quotes (`transcripts_list`, `transcripts_read`)

When the synthesis needs evidence, pull transcripts: `GET /transcripts/{uuid}/` returns
`text`. Tadeus stores the transcript, never the audio, so this is the deepest available
record.

A transcript belongs to an **attempt**, not directly to a session — `Transcript.attempt_uuid`
links to `Attempt`, and `Attempt.session_uuid` links to `Session`. There is no expansion
mechanism, so walk it: `attempts_list` → `transcripts_list`.

## Compliance note

An insight is aggregate output — themes and distributions across a campaign. That
aggregate-only character is the pivot of Tadeus' own EU AI Act position (see
`conformance/tadeus-api-conformance.yml`): a workforce listening tool that returns themes and
feeds no decision about a named person sits outside Annex III point 4. Exporting per-session
results into a system that surfaces individuals to their managers changes the classification
of the deployment, regardless of what the paperwork says. If your pipeline does that, it is
a drift trigger and the classification needs re-running.

## Related

- `skills/tadeus-api-run-interview-campaign.md`
- `data-model/tadeus-api-data-model.yml` — the Template → Campaign → Session → Attempt →
  Transcript/Result → Insight graph.
