# n8n Build Instructions — Post-Conversation Workflow

Version: v0.2.0

## Workflow Summary

This workflow fires after each Carly conversation (voice or chat). It archives the raw data, extracts structured insight, logs it to Google Sheets, and alerts the team when something needs attention.

## Nodes (in order)

### Node 1: Webhook Trigger

- Type: Webhook
- Method: POST
- Path: `/carly-conversation`
- Purpose: receives the Retell webhook payload when a conversation ends
- Output: raw webhook JSON

### Node 2: Archive Raw Data to Google Drive

- Type: Google Drive (upload file)
- Input: full webhook JSON payload (stringified)
- Destination folder: `Cook in Fiesta / Carly / Raw Transcripts`
- Filename: `{call_id}_{timestamp}.json`
- Purpose: preserve the unprocessed data for audit and debugging

### Node 3: Extract Structured Conversation Log

- Type: HTTP Request (to OpenAI or Anthropic API) or AI Agent node
- Input: transcript text from the webhook payload
- System prompt: use the prompt from `prompts/v0.2.0/per-conversation-logging-prompt.md`
- Expected output: JSON matching `schemas/v0.2.0/carly-conversation.schema.json`
- Model: GPT-4o mini or Claude Haiku

### Node 4: Write to Google Sheets

- Type: Google Sheets (append row)
- Spreadsheet: Cook in Fiesta — Carly Conversations
- Tab: `Conversations`
- Columns: see `google-sheets-repository-map.md`
- Input: parsed JSON from Node 3

### Node 5: Check Escalation / Unanswered

- Type: IF node
- Condition: `escalated == true` OR `unanswered_questions` array is not empty
- True path → Node 6
- False path → end

### Node 6: Send WhatsApp Alert

- Type: HTTP Request (to WhatsApp Business API or n8n WhatsApp node)
- Template: use the appropriate template from `templates/v0.2.0/whatsapp-alerts.md`
- Recipients: configured in environment variables
- Content: conversation summary, escalation reason, unanswered questions

## Environment Variables Needed

See `config/v0.2.0/env.example` for the full list. Key ones:

- `RETELL_API_KEY`
- `N8N_WEBHOOK_URL`
- `OPENAI_API_KEY` or `ANTHROPIC_API_KEY`
- `GOOGLE_DRIVE_FOLDER_ID`
- `GOOGLE_SHEETS_ID`
- `WHATSAPP_API_URL`
- `WHATSAPP_RECIPIENT`

## Testing

1. Use `fixtures/v0.2.0/sample-retell-webhook.json` to manually trigger the webhook
2. Confirm the raw JSON appears in Google Drive
3. Confirm a row is written to Google Sheets with correct fields
4. Confirm a WhatsApp alert is sent when escalation or unanswered questions are present
5. Confirm no WhatsApp alert is sent for normal resolved conversations
