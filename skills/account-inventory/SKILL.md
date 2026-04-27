---
name: account-inventory
description: |
  Lists all generated/owned assets across the user's Salecraft account in one
  unified view. Always queries BOTH landing_ai_mcp (creative outputs:
  brands / LP / reels / ads / campaigns) AND zereo_social_mcp (social layer:
  connected accounts + post history) in parallel. Without this skill, LLM
  tends to query only one MCP server and silently hide work the user already
  paid for.
  Trigger: "show my assets", "what's in my account", "list everything I have",
  "我有什麼資產", "看我帳號裡有什麼", "掃描我的帳號", "列出我所有東西",
  "list my generated content", any inventory-style request.
allowed-tools:
  - Read
  - Grep
---

# Account Inventory — Cross-MCP Asset Listing

## 🚨 STOP — READ THIS FIRST

When the user asks any of these:
- 「掃描我的資產」「我有什麼東西」「我帳號裡有什麼」「列出我所有...」「show my assets」「list everything」「what's in my account」

**You MUST call ALL 7 list_* tools in PARALLEL** across both MCP servers — not just one or two. Asset is cross-cutting (brand + LP + reels + ads + social) and missing any layer means hiding work the user already paid for.

實測 2026-04-27：使用者要求「scan 我帳號」，LLM 只跑了 `list_brands` + `list_sessions` + 廣告統計（landing_ai_mcp 那層），**完全沒掃 `zereo_social_mcp`**。結果：使用者剛剛發到 TikTok 的 demo 影片不在報告裡、TikTok / Meta 連線狀態也沒被列出 — 後續使用者問「把那支影片發到 TikTok」時 LLM 找不到要哪支、demo 卡住。

## The 7 Required Calls (parallel, single-message)

### From `landing_ai_mcp` (creative outputs)

1. `list_brands(user_token)` → 品牌（含 logo / primary_color / industry / 預設語言）
2. `list_sessions(user_token)` → LP 草稿（含 wizard data + 已生成的 campaign 數量）
3. `list_reel_sessions(user_token)` → AI 短影音 sessions（含 final_video_url、duration_seconds、status）
4. `list_campaigns(user_token)` → 已上線 LP（含 share_url、creator landing 連結）

### From `zereo_social_mcp` (social layer)

5. `list_accounts(user_token)` → 連結的 IG / FB / TikTok 帳號（含 platform + display_name + can_publish）
6. `get_post_history(user_token, limit=20)` → 發過的貼文（含 video_url / image_url + status + platform）
7. `list_content(user_token)` → Content Library（手動上傳但沒去發布的素材）

## 結果呈現格式（使用者看的版本）

```
你帳號裡的東西：

🏢 品牌（N 個）
  • 「品牌 A」— [一句描述]
  • ...

🎬 AI 短影音（N 支）
  • 「[標題]」— [duration]s — [status]
  • （0 支 → 「還沒生過 reels — 想試試嗎？」）

🖼 廣告圖（N 張，從 LP sessions 數出來）
  • 「[Quick Ad / Carousel]」— from「[session 名]」— [aspect]
  • （含 QC 警示如果有：「⚠️ 文字佔比 > 20%」之類）

📄 LP 草稿（N 個）
  • 「[session 名]」— [品牌] — [phase]

🚀 已上線 LP（N 個）
  • 「[campaign 名]」— [短連結]

📱 連結的社群帳號（N 個）
  • 🎵 TikTok @[name]
  • 📸 Instagram @[username]
  • 📘 Facebook [page]

💬 發過的貼文（N 則，最近 5 則）
  • [日期] [平台 emoji] [post_type] — 「[caption 前 30 字]」— [status]

下一步可以做：
- 把已生成的 [影片/圖片] 發到社群
- 繼續做完某個草稿
- 生新的 [reels / 廣告 / LP]
- 看某個品牌或某個 session 的詳細內容
```

## Rules

### Rule 1: 空的 list 也要顯示「0 個」、不能整個 section 省略
使用者不知道你查過沒查到 vs 你根本沒查。「0 支 reels」是有用資訊（提示「想生一支嗎？」），完全不顯示就誤導。

### Rule 2: 禁止顯示 ID / UUID
跟 [Salecraft-Plugin/CLAUDE.md JARGON BLACKLIST #7] 一致 — 用「名字」+「一句描述」呈現，不顯示 `session_id: 5f988aee-...`。內部記在你心智模型裡、回答時翻成人話。

### Rule 3: 並行呼叫、禁止串列
7 個 call 並行 = 2-3 秒回完。串列 = 7 倍時間、使用者覺得卡。**單一訊息內塞 7 個 mcp_tool_call 區塊**，不要等前一個回完才打下一個。

### Rule 4: get_post_history 要按平台分類顯示
[zereo_social_posts] 表跨平台儲存（IG / FB / TikTok），使用者問 TikTok 相關問題時，要主動把 TikTok 貼文撈出來放最上面。判斷依據：使用者最近的對話脈絡 / 連結帳號裡哪個平台最活躍。

### Rule 5: 後續動作明確列出來
使用者掃完資產後，下一步通常是：發布、續做、或重新生成。**inventory 報告的最後一個 section 必須是「下一步可以做什麼」**，並列出 3-5 個具體選項，讓使用者一句話就能切換到下一個 SKILL（publish-social / generate-reels / edit-landing / generate-landing）。

### Rule 6: 若整個帳號是空的（全 0），轉進 saleskit
新使用者所有層都是 0 → 不要列空清單把使用者嚇跑。改說「你目前帳號是新的、還沒做過任何東西。先來個免費行銷諮詢，我幫你想清楚要做什麼吧？」→ 進 `saleskit`。

## 後續延伸（typical hand-off）

使用者選定要對某個資產做事時：

| 使用者意圖 | 跳到哪個 SKILL | 前置資料 |
|----------|--------------|---------|
| 「把 [影片名] 發到 TikTok」 | `publish-social` | 已知 video_url + tiktok account_id |
| 「繼續做 [LP 名]」 | `generate-landing` | 已知 session_id + 卡在哪個 phase |
| 「生新的 reels」 | `generate-reels` | 詢問是否複用既有 brand 資料 |
| 「重做 [廣告圖]」 | `edit-landing`（regenerate stripe）| 已知 campaign_id + stripe_idx |
| 「看某個品牌素材詳細」 | call `get_brand` + `list_brand_assets`（不離開本 SKILL）| 已知 brand_id |

## 與 brand-memory 的關係

如果 `brand-memory` 有 hydrate 過資料（[Salecraft-Plugin/CLAUDE.md `brand-memory MANDATORY 觸發表]），可優先使用 hydrated 結果，避免重複呼叫 list_brands。但其他 6 個 list 仍需現場查（brand-memory 不快取 sessions / reels / posts）。
