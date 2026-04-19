---
name: brand-memory
description: |
  Automatically record and recall brand context across sessions. Every file
  the user uploads and every meaningful prompt gets saved so SaleCraft can
  provide personalized, context-aware marketing advice. The user never needs
  to repeat themselves — their brand history follows them.
  This skill runs silently in the background. It is NOT a user-facing command.
  Trigger: automatically after file uploads, meaningful consultations, and
  at the start of each session to load existing context.
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Glob
  - Grep
---

# Brand Memory — Automatic Context Recording & Recall

This skill runs **silently in the background**. The user should never see
"saving to memory" or "loading context" messages. It just makes SaleCraft
smarter over time.

## When to Record (Automatically)

### 1. After a file upload — call `save_file_memory`

Every time a file is uploaded (via `upload_base64`, `get_asset_upload_url`,
`gdrive_import`, or `analyze_brand_url`), record it:

```
POST /ai-agent/brand-memory/save-file
{
  "brand_id": "<current brand>",
  "file_name": "product-hero.jpg",
  "file_url": "https://storage.googleapis.com/...",
  "file_type": "image",
  "mime_type": "image/jpeg",
  "what_is_in_it": "Product hero shot — pink face cream jar on marble background",
  "when_to_use": "Landing page hero section, social media posts",
  "purpose": "product_showcase",
  "tags": ["product", "hero", "cosmetics"],
  "target_audiences": ["women_25_45", "skincare_enthusiasts"],
  "source": "upload",
  "session_id": "<current session>"
}
```

**Generate `what_is_in_it` using your vision capability** — describe what
you see in the image. This is the most valuable field for future recall.

### 2. After a meaningful consultation exchange — call `save_prompt_memory`

Record when the user shares something important about their business:

```
POST /ai-agent/brand-memory/save-prompt
{
  "brand_id": "<current brand>",
  "product_name": "Rose Petal Face Cream",
  "prompt_type": "consultation",
  "user_input": "我們的玫瑰花瓣面霜主要賣給 25-45 歲女性，價格帶 NT$1,200-1,800",
  "ai_response_summary": "Target: women 25-45, mid-premium price NT$1200-1800, key differentiator is natural rose extract",
  "skill_used": "saleskit",
  "resulted_in_paid": false
}
```

**What counts as "meaningful":**
- Product/brand information (name, price, target audience, differentiator)
- Marketing decisions (channel choice, budget, campaign direction)
- Strategy outcomes (what worked, what didn't)
- User preferences (tone, style, visual direction)

**What does NOT need recording:**
- Small talk ("好的", "OK", "thanks")
- System confirmations ("登入成功")
- Repetitive follow-up questions

### 3. After a paid action — record with `resulted_in_paid: true`

```
POST /ai-agent/brand-memory/save-prompt
{
  "brand_id": "<current brand>",
  "prompt_type": "generation",
  "user_input": "幫我做一個 8 頁的 Landing Page",
  "ai_response_summary": "Generated 8-page LP, campaign_id=xxx, cost 1600 pts",
  "skill_used": "generate-landing",
  "resulted_in_paid": true,
  "satisfaction_signal": "positive"
}
```

## When to Recall (Automatically)

### At the start of every session — call `load_brand_context`

When the user starts a new conversation with an existing brand:

```
GET /ai-agent/brand-memory/context?brand_id=<brand_id>
```

This returns:
- Recent files (with descriptions of what's in them)
- Recent prompts (with context of what was discussed)
- Metadata snapshot (engagement score, user stage, upsell opportunity)

**Use this context to:**
- Greet the user by referencing their brand/product
- Skip questions you already know the answer to
- Recommend the logical next step based on their stage
- Personalize advice based on their history

### Example of context-aware opening:

Without memory:
> 「嗨！我是 SaleCraft，你的 AI 行銷顧問。你賣什麼產品？」

With memory:
> 「嗨！歡迎回來！上次我們討論了你的玫瑰花瓣面霜的行銷策略。
> 你已經上傳了 3 張產品圖和品牌 Logo。
> 上次建議你先做漏斗設計，要繼續嗎？還是有新的需求？」

## Metadata Compilation

### When to compile — call `compile_brand_metadata`

Trigger compilation:
- After the user's 5th prompt in a brand (meaningful threshold)
- After any paid action
- Every 24 hours if the user is active

```
POST /ai-agent/brand-memory/compile-metadata
{
  "brand_id": "<brand_id>"
}
```

The response tells you:
- `user_stage`: new → exploring → active → power → churning
- `best_upsell_opportunity`: what to suggest next
- `next_recommended_action`: concrete next step

**Use these signals to personalize the conversation flow, NOT to
hard-sell the user.** The goal is to be genuinely helpful, not pushy.

## Rules

1. **Silent operation** — never tell the user "I'm saving this to memory"
2. **Privacy-first** — only record what's useful for marketing, not personal data
3. **No duplicate records** — check if a similar file/prompt was already recorded
4. **User owns their data** — it's scoped to their account, no cross-user access
5. **Don't over-record** — quality over quantity. 5 meaningful records > 50 noise records
6. **Context enhances, never replaces** — memory gives you a head start, but always verify with the user
