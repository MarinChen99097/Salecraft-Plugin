---
name: salecraft-status
description: "Check generation status, credit balance, and session history"
---

# /salecraft-status — Status Check

Read `CLAUDE.md` for tool signatures.

## Actions

### Check credit balance
```
mcp_tool_call("landing_ai_mcp", "get_me", { "user_token": token })
→ Show: credits_remaining, tier, subscription_status
```

### List recent sessions
```
mcp_tool_call("landing_ai_mcp", "list_sessions", { "user_token": token })
→ Show: session_name, status, created_at, stripe_count
```

### Check specific session status
```
mcp_tool_call("landing_ai_mcp", "get_session", { "user_token": token, "session_id": id })
→ Show: generation status, progress, errors if any
```

### List ad campaigns
```
mcp_tool_call("zereo_social_mcp", "list_ad_campaigns", { "user_token": token })
→ Show: campaign_name, platform, status, budget, performance metrics
```

## Output Format

```
📊 SaleCraft 狀態
━━━━━━━━━━━━━━━━━
剩餘點數: [X] pts
進行中的專案: [N] 個
已發佈的頁面: [N] 個
廣告活動: [N] 個

最近動態:
- [名稱] — [狀態] — [日期]
- [名稱] — [狀態] — [日期]
```

## Login Awareness
**You CAN log users in directly.** Must be logged in to check status. Ask email + password → call `login`.

## No Jargon Rule
Show "剩餘點數" not "credits_remaining". Show "進行中" not "processing". Never expose raw API data to users.
