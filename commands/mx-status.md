---
name: mx-status
description: "Check generation status, credit balance, and session history"
---

# /mx-status — Status Check

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
📊 MarketingX Status
━━━━━━━━━━━━━━━━━
Credits: [X] remaining (weekly reset: [day])
Active Sessions: [N]
Published Pages: [N]
Active Ad Campaigns: [N]

Recent Activity:
- [session_name] — [status] — [date]
- [session_name] — [status] — [date]
```
