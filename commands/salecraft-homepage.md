---
name: salecraft-homepage
description: "Build a homepage from an existing landing page — embed LP stripes into a website"
---

# /salecraft-homepage — Build Homepage from Existing LP

Read `CLAUDE.md` for tool signatures.

## Quick Start

1. If not logged in, run the **AI Token** flow (NEVER ask for email/password): hand user `https://salecraft.ai/{locale}/connect`, ask them to paste `sc_live_...`, call `authenticate_with_token`
2. Ask which landing page to use, or list available ones:
   ```
   mcp_tool_call("landing_ai_mcp", "list_sessions", { "user_token": token })
   ```
3. Invoke the `homepage-builder` skill with selected campaign_id(s)

## Multiple LPs
User can provide multiple products for a multi-product homepage:
```
"幫我用這 3 個產品做一個首頁"
→ Use multi-product.html layout template
```

## Locale Selection
Ask for target language. Default to the LP's original language. For multi-language, generate separate HTML files per language.

## Login Awareness (AI Token only)
**Authenticate via AI Token — never ask for email or password.** Direct user to `https://salecraft.ai/{locale}/connect`, ask them to click 「複製 AI 登入 Token」 and paste `sc_live_...` back, then call `authenticate_with_token`.

## No Jargon Rule
Say "產品頁" not "LP". Say "語言" not "locale". Never mention campaign_id to users.
