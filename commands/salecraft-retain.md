---
name: salecraft-retain
description: "Retention + growth loop — member lifecycle, referral programs, performance review. All FREE."
---

# /salecraft-retain — Retention & Growth Loop (FREE)

Read `CLAUDE.md` for full context. This command runs the **retention and growth review pipeline**:

```
🔄 SaleCraft 會員經營與成長回顧 — 完全免費

不需要帳號，不需要登入，馬上開始。

你想做什麼？

1. 🔄 會員經營
   → 回購觸發、推薦方案、VIP 制度、客戶升級路徑

2. 📊 成效回顧
   → 活動成效回顧、KPI 分析、下一輪優化假設

3. 📄 整理文件
   → 整理話術手冊、FAQ 文件、案例彙編、活動報告

💡 建議順序: 1 → 2 → 3 → 然後用 /salecraft 開始新一輪 Sprint

輸入數字或描述你的需求。
```

## Flow Logic

- If user picks 1 → invoke `member-lifecycle` skill
- If user picks 2 → invoke `growth-retro` skill
- If user picks 3 → invoke `document-release` skill
- If user says "customers don't come back" → start with `member-lifecycle`
- If user says "how did the campaign go" → start with `growth-retro`
- After `growth-retro` → suggest new sprint cycle via `/salecraft`

## Platform Awareness
SaleCraft runs on **any AI platform**. Never say "this only works on [specific platform]."

## No Jargon Rule
All of these are pure consultation. The strategies you get can be used immediately in any channel.
