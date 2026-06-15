# Website Embed Guide — Carly on cookinfiesta.com

Version: v0.2.0

## Overview

Carly is embedded on cookinfiesta.com using Retell's web widgets. Customers see two options: voice call or text chat. Both connect to the same Retell agent with the same knowledge base.

## Prerequisites

- Retell agent configured and tested in the Retell dashboard
- Retell agent ID (from the Retell dashboard → Agent → Settings)
- Access to edit the cookinfiesta.com website HTML (or CMS)

## Embedding the Widget

Retell provides a JavaScript snippet for embedding. The general pattern is:

1. Log in to the Retell dashboard
2. Go to your agent → Deploy / Embed
3. Copy the embed code snippet
4. Paste it into the cookinfiesta.com website, typically just before the closing `</body>` tag

The exact snippet depends on your Retell plan and agent type. Refer to Retell's current documentation for the embed code:
https://docs.retellai.com

## Two-Button Layout

To give customers a choice between voice and chat, create a simple UI element on the website:

```html
<!-- Example: floating action buttons — adapt styling to match the site -->
<div id="carly-buttons" style="position: fixed; bottom: 20px; right: 20px; z-index: 9999;">
  <button id="carly-voice" onclick="startCarlyVoice()">🎙 Talk to Carly</button>
  <button id="carly-chat" onclick="startCarlyChat()">💬 Chat with Carly</button>
</div>
```

The `startCarlyVoice()` and `startCarlyChat()` functions should call the Retell SDK methods for launching a voice call or chat session. The exact JavaScript depends on the Retell SDK version — see Retell docs for the current API.

## Testing Checklist

- [ ] Voice button launches a voice call in the browser
- [ ] Chat button opens a text chat window
- [ ] Agent responds with accurate information from the knowledge base
- [ ] Agent provides the booking link when asked about booking
- [ ] Agent acknowledges when it cannot answer a question
- [ ] Widget works on mobile devices
- [ ] Widget does not break the existing website layout

## Notes

- The widget styling should match the Cook in Fiesta brand (colors, fonts)
- Consider adding a small avatar or logo to the widget for brand recognition
- If the site uses a CMS (WordPress, Wix, etc.), the embed goes in the custom HTML or header/footer injection area
