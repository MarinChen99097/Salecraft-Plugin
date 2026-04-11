---
name: mx-homepage
description: "Build a homepage from an existing landing page — embed LP stripes into a website"
---

# /mx-homepage — Build Homepage from Existing LP

Read `CLAUDE.md` for tool signatures.

## Quick Start

1. Authenticate if needed
2. Ask for `campaign_id` or list available LPs:
   ```
   mcp_tool_call("landing_ai_mcp", "list_sessions", { "user_token": token })
   ```
3. Invoke the `homepage-builder` skill with selected campaign_id(s)

## Multiple LPs
User can provide multiple campaign_ids for a multi-product homepage:
```
"Build a homepage with these 3 products: [id1], [id2], [id3]"
→ Use multi-product.html layout template
```

## Locale Selection
Ask for target locale. Default to the LP's original locale. For multi-locale, generate separate HTML files per locale.
