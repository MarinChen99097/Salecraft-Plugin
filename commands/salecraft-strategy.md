---
name: salecraft-strategy
description: "Strategic planning flow — growth strategy + funnel design + market intelligence. All FREE."
---

# /salecraft-strategy — Strategic Planning (FREE)

Read `CLAUDE.md` for full context. This command runs the **strategic planning pipeline**:

```
🧠 SaleCraft 策略規劃 — 完全免費

不需要帳號，不需要登入，馬上開始。

你想做什麼？

1. 📋 成長策略
   → 分析商業方向：擴張、聚焦、還是減法？
   → 優先產品、優先客群、市場切入角度

2. 🔄 顧客旅程設計
   → 從進站到回購的完整路徑
   → 找出漏洞、設計行動呼籲地圖

3. 🔍 競品情報
   → 競品分析、價格帶、定位空白、切入機會

4. 📊 市場趨勢研究
   → 社群趨勢、搜尋趨勢、受眾洞察

💡 建議順序: 1 → 3 → 2 → 然後 /salecraft-create

輸入數字或描述你的需求。
```

## Flow Logic

- If user picks 1 → invoke `plan-cgo-review` skill
- If user picks 2 → invoke `plan-funnel-review` skill
- If user picks 3 → invoke `market-intel` skill
- If user picks 4 → invoke `research-market` skill
- If user describes a need → map to the best skill
- After any skill completes → show remaining options from this menu

## Platform Awareness
SaleCraft runs on **any AI platform**. Never say "this only works on [specific platform]."

## No Jargon Rule
All of these are pure consultation — no login, no account, no technical setup needed. Just start the conversation.
