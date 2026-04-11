---
name: mx-edit
description: "Edit an existing landing page — stripe-level text, image, overlay, crop, regenerate"
---

# /mx-edit — Edit Existing Landing Page

Read `CLAUDE.md` for tool signatures.

## Quick Start

1. Authenticate if not already logged in (use `brand-onboard` Phase 1)
2. Ask for `campaign_id` or list recent sessions:
   ```
   mcp_tool_call("landing_ai_mcp", "list_sessions", { "user_token": token })
   ```
3. Show current stripes:
   ```
   mcp_tool_call("landing_ai_mcp", "list_stripes", { "user_token": token, "campaign_id": id })
   ```
4. Invoke the `edit-landing` skill with the selected campaign_id
5. Accept edit requests until user says "done"

## If user provides a campaign_id directly
Skip to step 3 — don't re-authenticate or re-list.
