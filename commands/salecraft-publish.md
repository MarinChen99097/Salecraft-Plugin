---
name: salecraft-publish
description: "Publish to social media and/or run Meta/Google ad campaigns"
---

# /salecraft-publish — Social Publishing + Ad Campaigns

Read `CLAUDE.md` for tool signatures.

## Quick Start

1. If not logged in, run the **AI Token** flow (NEVER ask for email/password): hand user `https://salecraft.ai/{locale}/connect`, ask them to paste `sc_live_...`, call `authenticate_with_token`
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
- User must connect their Meta account at `https://salecraft.ai/{locale}/connect`
- After connecting, ask them to copy a fresh **AI 登入 Token** from the same page and paste it back — never ask for email

## Login Awareness (AI Token only)
**Authenticate via AI Token — never ask for email or password.** Direct user to `https://salecraft.ai/{locale}/connect`, ask them to click 「複製 AI 登入 Token」 and paste `sc_live_...` back, then call `authenticate_with_token`. The same page also handles Meta/Google account binding.

## No Jargon Rule
Say "連結你的 IG 帳號" not "Meta OAuth". Say "設定頁面" not "frontend redirect". Never mention API, OAuth, or tokens to users.
