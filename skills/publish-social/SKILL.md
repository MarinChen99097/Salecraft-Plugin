---
name: publish-social
description: |
  Publish landing page content to social media platforms. Lists connected accounts,
  imports LP content, validates media specs, suggests optimal timing, and publishes
  to multiple platforms simultaneously via zereo_social_mcp.
  Trigger: Phase 6a of /mx-publish, or "post to social media", "publish to Instagram".
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - AskUserQuestion
---

# Social Publishing — Multi-Platform Content Distribution

You manage social media publishing for LP content. You help the user push their landing page visuals and copy to connected social platforms.

## Prerequisites

- `user_token` and `session_id` from previous phases
- Read `CLAUDE.md` for zereo_social_mcp tool signatures

## Phase 1: Check Connected Accounts

```
mcp_tool_call("zereo_social_mcp", "list_accounts", {
  "user_token": token
})
→ Returns: array of connected social accounts with platform, name, capabilities
```

Present:
```
Connected Accounts:
1. 📘 Facebook — ACME Official Page
2. 📸 Instagram — @acme_brand
3. 🎵 TikTok — @acme_official

Select platforms to publish to (e.g., "1 and 2"):
```

If no accounts connected:
```
⚠ No social accounts connected.
Connect accounts at [Zereo dashboard URL] first, then try again.
```

## Phase 2: Import LP Content

```
mcp_tool_call("zereo_social_mcp", "import_from_session", {
  "user_token": token,
  "session_id": session_id
})
→ Imports LP stripe images and copy into the content library
```

## Phase 3: Validate Media

```
mcp_tool_call("zereo_social_mcp", "validate_content_media", {
  "user_token": token,
  // content references from import
})
→ Returns: validation results (size, format, aspect ratio checks per platform)
```

If validation fails: report issues and suggest fixes (resize, reformat).

## Phase 4: Compose Posts

Help user craft platform-specific captions:

```
Suggested captions:

📘 Facebook:
"Discover [product] — designed for [TA]. [Value proposition]. 
Learn more: [landing_page_url] #[hashtag1] #[hashtag2]"

📸 Instagram:
"✨ [Short hook]
[Key benefit 1]
[Key benefit 2]
[CTA] — Link in bio!
.
.
#[hashtag1] #[hashtag2] ... #[hashtag10]"

🎵 TikTok:
"[Trend-aware hook] #[hashtag] #fyp"

Edit any caption, or approve all?
```

## Phase 5: Schedule or Publish Now

### Get optimal times
```
mcp_tool_call("zereo_social_mcp", "suggest_schedule", {
  "user_token": token
})
→ Returns: recommended posting times per platform based on audience activity
```

Present:
```
Best posting times:
- Facebook: Today 7:00 PM (highest engagement)
- Instagram: Today 8:30 PM
- TikTok: Today 9:00 PM

A) Publish now
B) Schedule at optimal times
C) Custom schedule
```

### Publish
```
mcp_tool_call("zereo_social_mcp", "publish_multi", {
  "user_token": token,
  "targets_json": "[
    {\"account_id\": \"fb_123\", \"caption\": \"...\", \"media_ids\": [\"...\"]},
    {\"account_id\": \"ig_456\", \"caption\": \"...\", \"media_ids\": [\"...\"]},
    {\"account_id\": \"tt_789\", \"caption\": \"...\", \"media_ids\": [\"...\"]}
  ]"
})
```

### Or schedule
```
mcp_tool_call("zereo_social_mcp", "schedule_post", {
  "user_token": token,
  "data_json": "{\"account_id\": \"...\", \"scheduled_at\": \"2026-04-15T19:00:00+08:00\", ...}"
})
```

## Phase 6: QR Code (Optional)

For print materials or physical displays:

```
mcp_tool_call("zereo_social_mcp", "generate_qr", {
  "user_token": token,
  "url": landing_page_url,
  "style": "branded"
})
→ Returns: QR code image URL
```

## Output

```
✅ Published successfully!

📘 Facebook — Posted at [time] — [post_url]
📸 Instagram — Scheduled for [time]
🎵 TikTok — Posted at [time] — [post_url]

Want to:
A) Run ad campaigns on these platforms → /mx-publish (ads)
B) Check engagement later
C) Done
```
