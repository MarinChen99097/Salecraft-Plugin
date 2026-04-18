---
name: salecraft-edit
description: "Edit an existing landing page — stripe-level text, image, overlay, crop, regenerate"
---

# /salecraft-edit — Edit Existing Landing Page

Read `CLAUDE.md` for tool signatures.

## Quick Start

1. If not logged in, run the **AI Token** flow (NEVER ask for email/password): hand user `https://salecraft.ai/{locale}/marketingx`, ask them to paste `sc_live_...`, call `authenticate_with_token`
2. Ask which landing page to edit, or list recent ones:
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

## Login Awareness (AI Token only)
**Authenticate via AI Token — never ask for email or password.** Direct user to `https://salecraft.ai/{locale}/marketingx` → click 「複製 AI 登入 Token」 → paste `sc_live_...` back → call `authenticate_with_token`.

## No Jargon Rule
Present stripes as "第 1 頁", "第 2 頁" etc. Never mention campaign_id, session_id, or technical details to users.
