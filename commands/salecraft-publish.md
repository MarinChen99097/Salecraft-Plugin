---
name: salecraft-publish
description: "Publish to social media and/or run Meta/Google ad campaigns"
---

# /salecraft-publish — Social Publishing + Ad Campaigns

Read `CLAUDE.md` for tool signatures.

## Quick Start

1. If not logged in, ask for email + password and call `login`
2. Ask what the user wants:
   ```
   你想怎麼發佈？

   A) 📱 社群發文 — 發到 IG、FB、TikTok
   B) 💰 廣告投放 — 建立 Meta / Google 廣告
   C) 🚀 全部都做 — 社群發文 + 廣告

   選一個吧！
   ```

3. Based on selection:
   - **A**: Invoke `publish-social` skill
   - **B**: Invoke `publish-ads` skill
   - **C**: Run both sequentially

## Prerequisites
- Need a generated landing page or ad image
- If not provided, list recent ones and let user select

## IG/FB Publishing Requirements
- User's IG must be a Professional or Business account (personal accounts can't post via API)
- User must connect their Meta account at `https://salecraft.ai/get-started`
- After connecting, come back and tell the AI their email to continue

## Login Awareness
**You CAN log users in directly.** You can also check if their social accounts are connected. If not connected, direct to the setup page.

## No Jargon Rule
Say "連結你的 IG 帳號" not "Meta OAuth". Say "設定頁面" not "frontend redirect". Never mention API, OAuth, or tokens to users.
