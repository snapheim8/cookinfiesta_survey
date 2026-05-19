# Carly Automation Overview

Version: v0.1.0

## Purpose

Carly is a Retell AI customer insight agent for Cook in Fiesta customers who already booked an experience. Her job is to understand why people booked, how they decided, what alternatives they considered, what mattered most, what nearly stopped them, how price affected the decision, who influenced the booking, and whether any human follow-up is needed.

Carly is not a sales agent. The automation must avoid upsell logic, promotional scoring, or conversion nudges.

## Core Flow

1. Retell sends a webhook when Carly finishes a call.
2. n8n receives the webhook payload.
3. n8n fetches the full call transcript and call metadata from Retell.
4. n8n saves the raw webhook payload to Google Drive.
5. n8n saves the raw transcript and metadata to Google Drive.
6. n8n sends transcript and metadata to GPT-5.4 mini, or Claude if selected, for structured extraction.
7. n8n validates the returned JSON against the Carly schema.
8. n8n saves the structured JSON to Google Drive.
9. n8n appends or updates the Google Sheets repository.
10. n8n sends a WhatsApp alert after every Carly conversation.
11. n8n marks the call as priority if any action items, complaints, knowledge-base gaps, risks, or follow-up requests exist.
12. Every 5 new processed Carly calls, n8n runs trend analysis using GPT-5.4, saves the report, and sends the trend summary.

## Data Storage

Recommended Google Drive structure:

```text
Cook in Fiesta/
  Carly Customer Insights/
    01 Raw Webhooks/
      YYYY/
        MM/
    02 Raw Transcripts/
      YYYY/
        MM/
    03 Extracted JSON/
      YYYY/
        MM/
    04 Trend Reports/
      YYYY/
        MM/
    05 Failed Runs/
```

Recommended filename pattern:

```text
YYYY-MM-DD_HH-mm_call-{retell_call_id}_{customer_name_or_unknown}.json
YYYY-MM-DD_HH-mm_call-{retell_call_id}_{customer_name_or_unknown}.txt
```

Use a sanitized customer name. If no customer name is available, use `unknown`.

## Google Sheets Repository

Recommended tabs:

- `Calls` - one row per processed Carly call
- `Decision Insights` - normalized insight fields for research analysis
- `Follow Up` - human follow-up queue
- `Action Items` - operational issues, complaints, and improvement tasks
- `KB Gaps` - questions Carly could not answer or answered weakly
- `Trend Runs` - one row per every-5-call trend report
- `Processing Log` - status, errors, retries, and raw file links

## Priority Rules

Set `analysis_flags.priority_required` to `true` when any of these are true:

- Customer asked for human follow-up
- Customer expressed a complaint, disappointment, or trust concern
- Customer mentioned a safety, accessibility, payment, dietary, allergy, or logistics risk
- Carly failed to answer an important question
- There is a knowledge-base gap
- There is an action item assigned to the Cook in Fiesta team
- Survey quality is low because the customer was upset, confused, or the call ended abruptly

Priority level guidance:

- `urgent` - safety, allergy, payment failure, angry customer, reputational risk, time-sensitive booking issue
- `high` - explicit follow-up request, serious hesitation, complaint, clear operational issue
- `medium` - non-urgent action item, knowledge-base gap, repeated uncertainty, unclear expectation
- `low` - useful research insight only, no human action needed

## Idempotency

Use the Retell call ID as the primary idempotency key. Before writing a new row, n8n should check whether the call ID already exists in the `Calls` tab.

If the call ID already exists:

- Do not duplicate the row.
- Update the row only if the existing row is incomplete or marked as failed.
- Keep the existing Google Drive file links unless the rerun created corrected files.

## Failure Handling

If extraction fails:

- Save the raw transcript and webhook payload first.
- Write a failed processing row to `Processing Log`.
- Save the model error or validation error to `05 Failed Runs`.
- Send an internal WhatsApp failure alert if configured.

If Google Sheets update fails:

- Keep the raw and JSON files in Google Drive.
- Retry the sheet update.
- If retry fails, log the error and alert the operator.

## Privacy Notes

Store only the fields needed for customer insight and follow-up. Avoid adding sensitive personal data to WhatsApp alerts. Keep allergy, accessibility, and payment details in the structured JSON and follow-up queue, but summarize them carefully in chat notifications.
