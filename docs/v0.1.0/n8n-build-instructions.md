# n8n Build Instructions

Version: v0.1.0

## Workflow Name

`Carly - Retell Post-Call Insight Processing`

## Required Credentials

- Retell API key
- Google Drive OAuth credential
- Google Sheets OAuth credential
- OpenAI API key, or Claude provider credential if using Claude for extraction
- WhatsApp provider credential, such as WhatsApp Cloud API, Twilio, or 360dialog

## Environment Variables

```text
RETELL_API_KEY
OPENAI_API_KEY
GOOGLE_DRIVE_ROOT_FOLDER_ID
GOOGLE_SHEETS_REPOSITORY_ID
WHATSAPP_ALERT_TO
WHATSAPP_PROVIDER_TOKEN
CARLY_SCHEMA_VERSION=v0.1.0
```

## Main Workflow Steps

### 1. Webhook Trigger

Create a webhook trigger for Retell post-call completion events.

Expected input:

- Retell call ID
- Agent ID or agent name
- Call status
- Call start and end time
- Customer phone number, if available
- Booking reference, if available

Filter events so only Carly calls continue through the workflow.

### 2. Normalize Webhook Payload

Create a normalized object with:

```json
{
  "source": "retell",
  "agent_name": "Carly",
  "retell_call_id": "",
  "webhook_received_at": "",
  "call_status": "",
  "booking_reference": "",
  "customer_phone": ""
}
```

### 3. Save Raw Webhook to Google Drive

Save the original webhook payload before any transformation.

Suggested folder:

`Cook in Fiesta/Carly Customer Insights/01 Raw Webhooks/YYYY/MM`

### 4. Fetch Full Retell Call Data

Use an HTTP Request node to retrieve full call details from Retell.

The response should include:

- Full transcript
- Recording URL, if available
- Call duration
- Call timestamps
- Agent ID
- Customer number or customer ID
- Call outcome metadata

### 5. Save Raw Transcript and Metadata

Save transcript and call metadata to Google Drive.

Suggested folder:

`Cook in Fiesta/Carly Customer Insights/02 Raw Transcripts/YYYY/MM`

### 6. Run Per-Call Extraction

Use GPT-5.4 mini for the default extraction step.

Inputs:

- Full transcript
- Retell call metadata
- Booking metadata, if available
- Raw webhook summary
- Schema version

Output:

- Strict JSON matching `schemas/v0.1.0/carly-insight.schema.json`

Model settings:

```text
model: gpt-5.4-mini
temperature: 0.1
response_format: json_schema
```

If the model provider does not support strict schema output, validate JSON in n8n after the model call and retry once with a repair prompt.

### 7. Validate JSON

Validate that required top-level sections exist:

- `record_metadata`
- `booking_context`
- `survey_quality`
- `decision_insights`
- `market_research_signals`
- `product_feedback`
- `knowledge_base_gaps`
- `complaints_or_risks`
- `follow_up`
- `action_items`
- `analysis_flags`

If validation fails, retry extraction once. If it fails again, save to failed runs and alert.

### 8. Save Extracted JSON to Google Drive

Suggested folder:

`Cook in Fiesta/Carly Customer Insights/03 Extracted JSON/YYYY/MM`

### 9. Update Google Sheets

Use Retell call ID as the unique key.

Recommended writes:

- `Calls`: always append or update one row
- `Decision Insights`: append one row if survey quality is usable
- `Follow Up`: append rows when follow-up is needed
- `Action Items`: append each action item
- `KB Gaps`: append each knowledge-base gap
- `Processing Log`: append processing status

### 10. Send WhatsApp Alert

Send a WhatsApp alert after every Carly conversation.

Use:

- Standard alert template for normal calls
- Priority alert template when `analysis_flags.priority_required` is `true`
- Failure alert template when processing fails

### 11. Increment Processed Call Counter

Count successfully processed Carly calls since the last trend run.

When the count reaches 5, trigger the trend-analysis workflow.

Implementation options:

- Query `Calls` where `included_in_trend_run_id` is blank and `processing_status` is `processed`
- Or maintain a lightweight counter in n8n static data

The Google Sheets query approach is easier to audit.

## Trend Analysis Sub-Workflow

Workflow name:

`Carly - Every 5 Calls Trend Analysis`

Trigger:

- Called by main workflow after 5 new processed calls

Steps:

1. Read the 5 newest processed calls not yet included in a trend run.
2. Fetch their extracted JSON files from Google Drive, or read the normalized rows from Google Sheets.
3. Send the 5-call packet to GPT-5.4 using `prompts/v0.1.0/trend-analysis-prompt.md`.
4. Save the trend report to Google Drive.
5. Append a row to `Trend Runs`.
6. Mark the 5 calls with the new trend run ID.
7. Send a WhatsApp trend summary.

Model settings:

```text
model: gpt-5.4
temperature: 0.2
```

## QA Checklist

- Raw webhook saves even if later steps fail
- Raw transcript saves even if extraction fails
- JSON validates against the schema
- Duplicate Retell call IDs do not create duplicate sheet rows
- WhatsApp alert sends for every processed call
- Priority calls are clearly marked
- Follow-up requests appear in the `Follow Up` tab
- Complaints and risks appear in `Action Items`
- Knowledge-base gaps appear in `KB Gaps`
- Trend analysis runs exactly after 5 new processed calls
- Trend run ID is written back to all 5 source calls
