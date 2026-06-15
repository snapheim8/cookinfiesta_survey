# Automation Overview — Carly Customer Service Agent

Version: v0.2.0

## Purpose

Carly is a customer service agent for Cook in Fiesta. She answers questions from website visitors about tours, experiences, pricing, availability, and logistics. She operates via voice and text chat on cookinfiesta.com.

## Business Rules

1. Carly answers questions using information from the Retell knowledge base
2. Carly does not process payments, finalize bookings, or modify reservations
3. When a customer wants to book, Carly provides the booking page URL
4. When Carly cannot answer a question, she acknowledges it honestly and offers to connect the customer with the team
5. Carly is friendly, helpful, and concise — she represents the Cook in Fiesta brand
6. Carly operates in the language(s) configured in the Retell dashboard (language TBD — see Open Decisions)

## Data Flow

```
Customer visits cookinfiesta.com
        │
        ├── clicks "Talk to Carly" ──→ Retell Voice Agent
        └── clicks "Chat with Carly" ──→ Retell Chat Agent
                                              │
                                     (conversation happens)
                                              │
                                     Retell fires webhook
                                              │
                                          n8n workflow
                                              │
                        ┌─────────────────────┼─────────────────────┐
                        ▼                     ▼                     ▼
                  Google Drive          Google Sheets          WhatsApp alert
                (raw transcript)     (structured log row)    (if escalated or
                                                              unanswered question)
```

## Knowledge Base

All agent knowledge comes from the Retell knowledge base, which is manually synced from cookinfiesta.com content. The knowledge base should include:

- All tour and experience descriptions
- Pricing information
- Location and logistics (address, transport, parking, what to bring)
- FAQ answers
- Booking page URL
- Contact information

See `knowledge-base-sync.md` for the sync process.

## Post-Conversation Logging

Each conversation is processed by n8n to extract:

- Customer intent (what were they asking about)
- Questions asked
- Whether the question was answered successfully
- Whether the conversation was escalated
- Any unanswered questions (flagged for knowledge base review)

This data is written to Google Sheets and used to identify gaps in the knowledge base and common customer needs.
