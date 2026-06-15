# cookinfiesta_survey

AI-powered customer service agent for [cookinfiesta.com](https://www.cookinfiesta.com)

## What is this

Carly is a voice and text chat customer service agent built on Retell AI. She is embedded on the Cook in Fiesta website and answers customer questions about tours, experiences, pricing, availability, and logistics.

When a customer is ready to book, Carly directs them to the booking page.

## How it works

- **Agent:** Retell AI (voice + text chat widget on the website)
- **Knowledge source:** Retell knowledge base, manually synced from cookinfiesta.com content
- **Automation:** n8n workflow processes each conversation — archives transcript, extracts structured data, logs to Google Sheets, sends WhatsApp alerts for escalations
- **Data store:** Google Sheets (conversation logs, escalation queue, knowledge base sync log)

## Repository structure

All working materials are versioned. Current version: `v0.2.0`.

```
config/          — environment variable templates
docs/            — automation overview, build instructions, embed guide, handoff docs
fixtures/        — sample webhook payloads and conversation logs for testing
prompts/         — LLM prompts for post-conversation extraction
schemas/         — JSON schemas for structured conversation output
templates/       — WhatsApp alert message templates
```

See `CARLY_AUTOMATION_PACK.md` for the full file map and build order.

## Quick start

1. Read `CARLY_AUTOMATION_PACK.md` for the complete scope and build phases
2. Read `docs/v0.2.0/builder-handoff.md` for implementation order and acceptance criteria
3. Copy `config/v0.2.0/env.example` to `.env` and fill in your API keys and webhook URLs

## Status

Phase 1 (core agent) — in progress
Phase 2 (post-conversation automation) — not started
Phase 3 (booking system integration, trend analysis) — future
