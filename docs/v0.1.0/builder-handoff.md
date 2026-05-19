# Carly Builder Handoff

Version: v0.1.0

## Build Objective

Create an n8n automation that processes completed Carly calls from Retell into a durable customer insight repository.

Carly speaks with customers who already booked a Cook in Fiesta experience. The automation must support research, operations, customer follow-up, and knowledge-base improvement. It must not introduce sales or upsell behavior.

## Recommended Implementation Order

### Phase 1: Minimum Working Workflow

Build the smallest reliable path first:

1. Retell completed-call webhook triggers n8n.
2. n8n filters for Carly calls only.
3. n8n saves raw webhook payload to Google Drive.
4. n8n fetches full call transcript and metadata from Retell.
5. n8n saves raw transcript and call metadata to Google Drive.
6. n8n sends transcript and metadata to GPT-5.4 mini for JSON extraction.
7. n8n saves extracted JSON to Google Drive.
8. n8n writes or updates one row in the `Calls` tab.
9. n8n sends one WhatsApp alert.

Acceptance criteria:

- A single test webhook produces raw files, extracted JSON, one Google Sheets row, and one WhatsApp message.
- Re-running the same Retell call ID does not create a duplicate `Calls` row.
- Extraction output includes all required top-level schema fields.

### Phase 2: Operational Tabs and Priority Rules

Add normalized writes:

1. `Decision Insights`
2. `Follow Up`
3. `Action Items`
4. `KB Gaps`
5. `Processing Log`

Acceptance criteria:

- Follow-up requests appear in `Follow Up`.
- Complaints and risks appear in `Action Items`.
- Knowledge-base gaps appear in `KB Gaps`.
- Priority WhatsApp template is used when `analysis_flags.priority_required` is true.

### Phase 3: Every-5-Call Trend Analysis

Add the trend sub-workflow:

1. Query 5 processed calls with no `trend_run_id`.
2. Run trend analysis with GPT-5.4.
3. Save trend report to Google Drive.
4. Append a row to `Trend Runs`.
5. Write the trend run ID back to the 5 source calls.
6. Send a trend WhatsApp alert.

Acceptance criteria:

- Trend analysis runs only when 5 unreported calls are available.
- The same call is not included in multiple trend runs unless manually reset.

## Priority Logic

Use these rules in the n8n workflow after extraction:

```text
priority_required =
  follow_up.requested == true
  OR follow_up.required_by_context == true
  OR length(action_items) > 0
  OR length(knowledge_base_gaps) > 0
  OR length(complaints_or_risks) > 0
  OR analysis_flags.needs_manual_review == true
```

Prefer the model-provided `analysis_flags.priority_level`, but let deterministic rules override it upward when needed:

- Any urgent complaint or risk => `urgent`
- Explicit follow-up request => at least `high`
- Any complaint, risk, or action item => at least `medium`
- Knowledge-base gap only => at least `medium`

## Test Fixture

Use the sample files in `fixtures/v0.1.0` to simulate a complete Retell call:

- `sample-retell-webhook.json`
- `sample-retell-call-response.json`
- `sample-extraction-output.json`

Expected result:

- The call is marked priority.
- Follow-up is requested.
- Price sensitivity is medium.
- Alternatives considered include a restaurant dinner and cooking class.
- One knowledge-base gap is created for accessibility or mobility details.
- One action item is created for human follow-up.

## Open Decisions

These should be decided before production:

- Exact Retell webhook event name for completed Carly calls
- Retell call fetch endpoint and response shape in the live account
- WhatsApp provider: Meta Cloud API, Twilio, 360dialog, or another provider
- Google Drive folder IDs
- Google Sheets spreadsheet ID
- Whether customer identifiers should be phone numbers, booking references, hashed IDs, or first names only
- Human owner names for follow-up and KB tasks
- Whether failed processing alerts should go to WhatsApp, email, Slack, or all three

## Production Guardrails

- Store raw payloads before running AI extraction.
- Use Retell call ID as the idempotency key.
- Do not expose full transcripts in WhatsApp messages.
- Do not include unnecessary personal details in trend reports.
- Keep the extraction schema version in every JSON output.
- Save failed model outputs for debugging, but mark them as failed and do not write them as valid insight records.
