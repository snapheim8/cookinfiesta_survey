# Carly Automation Pack for Cook in Fiesta

Version: v0.1.0

Carly is a Retell AI customer insight agent for Cook in Fiesta customers who have already booked an experience. Carly does not sell, upsell, or persuade. Her role is to collect high-quality customer insight after booking.

This pack contains the working materials needed to build the first n8n automation:

- Versioned workflow documentation
- Per-call extraction prompt
- Structured JSON schema
- Every-5-call trend analysis prompt
- WhatsApp alert templates
- n8n build instructions and QA checklist

## Folder Map

- `docs/v0.1.0/automation-overview.md` - business logic, data flow, and operating rules
- `docs/v0.1.0/n8n-build-instructions.md` - step-by-step n8n workflow build
- `docs/v0.1.0/google-sheets-repository-map.md` - recommended tabs and columns
- `docs/v0.1.0/builder-handoff.md` - implementation order, acceptance criteria, and open decisions
- `prompts/v0.1.0/per-call-extraction-prompt.md` - prompt for GPT-5.4 mini or Claude
- `prompts/v0.1.0/trend-analysis-prompt.md` - prompt for GPT-5.4 every 5 processed calls
- `schemas/v0.1.0/carly-insight.schema.json` - structured output schema
- `templates/v0.1.0/whatsapp-alerts.md` - WhatsApp message templates
- `config/v0.1.0/env.example` - environment variable template
- `fixtures/v0.1.0/sample-retell-webhook.json` - sample webhook input
- `fixtures/v0.1.0/sample-retell-call-response.json` - sample Retell call response
- `fixtures/v0.1.0/sample-extraction-output.json` - example extracted JSON

## Recommended Model Setup

- Per-call extraction: GPT-5.4 mini
- Every-5-call trend analysis: GPT-5.4

## Primary Automation Goal

After each Retell call, n8n should fetch the full transcript and metadata, archive the raw inputs, extract structured insight JSON, update the Google Sheets repository, send a WhatsApp alert, and mark priority when follow-up, complaints, action items, or knowledge-base gaps are present.

Every 5 new processed Carly calls, n8n should run a trend analysis and save/send the report.

## Suggested Next Build Step

Start with the main post-call workflow only:

1. Receive Retell webhook.
2. Save raw webhook and transcript to Google Drive.
3. Run extraction.
4. Save JSON.
5. Write one row to `Calls`.
6. Send one WhatsApp alert.

After that is stable, add the normalized supporting tabs, priority queue, and every-5-call trend workflow.
