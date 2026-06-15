# Google Sheets Repository Map

Version: v0.2.0

## Spreadsheet: Cook in Fiesta — Carly Conversations

### Tab: Conversations

One row per conversation.

| Column | Type | Description |
|--------|------|-------------|
| conversation_id | string | Unique ID from Retell |
| channel | string | "voice" or "chat" |
| timestamp | datetime | When the conversation started |
| duration_seconds | number | Length of conversation |
| customer_intent | string | What the customer was asking about (e.g. "tour pricing", "booking", "logistics") |
| questions_asked | string | Comma-separated list of questions the customer asked |
| resolved | boolean | Whether the customer's question was answered |
| escalated | boolean | Whether the conversation was escalated to a human |
| escalation_reason | string | Why it was escalated (if applicable) |
| unanswered_questions | string | Questions Carly could not answer (if any) |
| summary | string | Brief summary of the conversation |
| booking_link_provided | boolean | Whether Carly directed the customer to the booking page |

### Tab: KB Sync Log

One row per knowledge base update.

| Column | Type | Description |
|--------|------|-------------|
| sync_date | date | When the sync was performed |
| changes | string | What content was updated |
| synced_by | string | Who performed the sync |
| test_result | string | "pass" or "fail" with notes |

### Tab: Unanswered Questions

Aggregated from conversations where Carly could not answer.

| Column | Type | Description |
|--------|------|-------------|
| question | string | The question Carly could not answer |
| first_seen | date | When this question was first asked |
| frequency | number | How many times this question has been asked |
| resolved | boolean | Whether the answer has been added to the knowledge base |
| resolved_date | date | When the answer was added |
