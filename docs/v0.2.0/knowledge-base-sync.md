# Knowledge Base Sync Process

Version: v0.2.0

## Overview

Carly's knowledge comes from the Retell knowledge base. This is manually updated whenever the content on cookinfiesta.com changes (new tours, price changes, schedule updates, etc.).

## When to Sync

- A new tour or experience is added
- Pricing changes
- Schedule or availability changes
- Location or logistics information changes
- FAQ content is added or updated
- Any information Carly has been giving incorrectly (identified via unanswered question flags)

## Sync Steps

1. **Collect the current content** — visit cookinfiesta.com and copy/export the relevant pages:
   - Tours and experiences (descriptions, what's included, duration)
   - Pricing
   - Schedule / availability
   - Location and logistics
   - FAQ
   - Contact information
   - Booking page URL

2. **Format for the knowledge base** — organize the content into clear sections. Use plain language. Remove navigation elements, footers, and other website chrome.

3. **Update the Retell knowledge base** — log in to the Retell dashboard → Agent → Knowledge Base → replace or update the content.

4. **Test the agent** — ask 3–5 representative questions to confirm the new content is reflected:
   - "What tours do you offer?"
   - "How much does [specific tour] cost?"
   - "How do I book?"
   - "Where are you located?"
   - A question related to the content that changed

5. **Log the sync** — add a row to the Google Sheets `KB Sync Log` tab:
   - Date
   - What changed
   - Who synced it
   - Test result (pass/fail)

## Future Improvement

If sync frequency becomes a burden, consider automating the scrape of cookinfiesta.com content and pushing it to the Retell knowledge base via API. This is not in scope for v0.2.0.
