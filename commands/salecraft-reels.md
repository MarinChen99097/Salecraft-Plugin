---
name: salecraft-reels
description: "Full Reels creation flow: consultation → script → review → generate → publish"
---

# /salecraft-reels — Short Video (Reels) Creation

Read `CLAUDE.md` and `prompts/WORKFLOW.md` for complete workflow reference.

Execute these phases in order:

## Phase 1: Authentication & Context

If not logged in:
- Ask for email and password, call `login` to authenticate
- Check credits via `get_me`
- If no account, direct to `https://salecraft.ai/get-started`

If `brand_id` available from previous session, carry it forward.

## Phase 2: Consultation (FREE)

Gather product info conversationally:
- Product name & niche
- Target audience
- Key message & CTA
- Video goal (awareness / conversion / engagement)
- Preferred duration (5-60 seconds, default 10s)
- Tone & style preferences (optional)

**Show cost estimate before proceeding:**
```
影片時長: 10 秒
預估費用: 1,000 pts（約 $33 美金）
你的餘額: X pts
```

## Phase 3: Script Generation & Review

Invoke the `generate-reels` skill (Phase 1-3).
- AI generates strategy + script (~30 seconds)
- Present script as structured table
- User reviews and optionally revises
- Loop until user approves script

## Phase 4: Video Generation

Invoke the `generate-reels` skill (Phase 4-6).
- Confirm cost with user before proceeding
- Trigger 7-agent pipeline (2-8 minutes)
- Report progress every 30 seconds
- Present final video with links

## Phase 5: Publish (Optional)

Suggest publishing options:
- `/salecraft-publish` → Post to IG Reels, TikTok, Facebook
- Direct share link for manual posting

## Cross-Phase State

Carry these values across all phases — never re-ask:
- `user_token`, `brand_id`
- `session_id` (reel session)
- `script_data` (approved script)

## Quick Start Examples

**User says:** "幫我做一個10秒的產品介紹影片"
→ Start at Phase 2, gather product details

**User says:** "用上次的品牌資料做 Reels"
→ Load existing `brand_id`, skip to Phase 2

**User says:** "我有腳本了，直接製作"
→ Input user's script, skip to Phase 4

## Login Awareness
**You CAN log users in directly.** Ask email + password → call `login`. Never say login isn't available.

## No Jargon Rule
Say "影片" not "Reels pipeline". Say "費用" not "pts deduction". Never mention agents, MCP, or technical internals to users.
