---
name: salecraft-engage
description: "Engagement + conversion strategy — interaction design, closing scripts, objection handling. All FREE."
---

# /salecraft-engage — Engagement & Conversion (FREE)

Read `CLAUDE.md` for full context. This command runs the **engagement and conversion pipeline**:

```
💬 SaleCraft 互動與成交策略 — 完全免費

不需要帳號，不需要登入，馬上開始。

你想做什麼？

1. 💬 互動設計
   → 私訊腳本、FAQ 對答樹、自動回覆、留資引導、預約腳本

2. 🎯 成交策略
   → 異議處理庫、價格鋪墊、社會證明、收單腳本、跟進節奏

3. 🔍 顧客旅程測試
   → 測試完整顧客旅程（從進站到成交）

💡 建議順序: 1 → 2 → 3 → 然後 /salecraft-audit

輸入數字或描述你遇到的情況。
```

## Flow Logic

- If user picks 1 → invoke `engage-operator` skill
- If user picks 2 → invoke `conversion-closer` skill
- If user picks 3 → invoke `journey-qa` skill
- If user says "people visit but don't buy" → start with `conversion-closer`
- If user says "no one contacts us" → start with `engage-operator`
- After completion → suggest the next skill in sequence

## Platform Awareness
SaleCraft runs on **any AI platform**. Never say "this only works on [specific platform]."

## No Jargon Rule
All of these are pure consultation — no login, no account, no technical setup needed. The scripts and strategies you get can be used anywhere: Line, IG DM, phone calls, in-store, flyers.
