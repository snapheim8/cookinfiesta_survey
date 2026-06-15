# Builder Handoff — Carly Customer Service Agent

Version: v0.2.0

## Implementation Order

### Phase 1 — Core Agent (estimated: 1–2 days)

1. Configure the Retell agent with the customer service system prompt
2. Populate the Retell knowledge base with cookinfiesta.com content
3. Test in the Retell dashboard (voice and chat)
4. Embed the widget on cookinfiesta.com
5. Test end-to-end on the live website

**Acceptance criteria:**
- Customer can start a voice call from the website
- Customer can start a text chat from the website
- Agent answers questions about tours, pricing, and logistics accurately
- Agent provides the booking link when asked
- Agent acknowledges when it cannot answer a question
- Widget works on desktop and mobile

### Phase 2 — Post-Conversation Automation (estimated: 1–2 days)

1. Set up the n8n webhook endpoint
2. Configure Retell to send webhooks to the n8n endpoint
3. Build the n8n workflow per `n8n-build-instructions.md`
4. Create the Google Sheets spreadsheet per `google-sheets-repository-map.md`
5. Create the Google Drive folder for raw transcripts
6. Configure WhatsApp alerting
7. Test with 5–10 conversations

**Acceptance criteria:**
- Each conversation produces a row in Google Sheets
- Raw transcript is saved to Google Drive
- WhatsApp alert fires when a conversation is escalated or has unanswered questions
- No alert fires for normal resolved conversations

## Open Decisions

1. **Booking system:** what platform does Cook in Fiesta use for bookings? Once identified, evaluate API availability for real-time availability queries.
2. **Language:** should Carly speak Portuguese, English, or both? This affects the system prompt and knowledge base content.
3. **Operating hours:** 24/7 or business hours only?
4. **WhatsApp recipients:** who should receive escalation alerts?
5. **Trend reporting:** reintroduce the every-N-conversation trend analysis from v0.1.0? Not recommended until conversation volume is high enough to make trends meaningful.

## Credentials and Access Needed

The person building this needs:

- Retell dashboard access (agent config, knowledge base, embed code)
- Google account with Drive and Sheets access
- n8n instance (cloud or self-hosted)
- WhatsApp Business API access (or alternative alerting channel)
- Access to edit the cookinfiesta.com website HTML

All API keys and secrets go in `.env` — never commit them to this repository. See `config/v0.2.0/env.example`.
