---
name: salecraft-audit
description: "Quality & governance audit — brand, offer, compliance, journey QA, launch checklist. All FREE."
---

# /salecraft-audit — Quality & Governance Audit (FREE)

Read `CLAUDE.md` for full context. This command runs the **quality and governance pipeline**:

```
🛡️ SaleCraft 品質檢查 — 完全免費

不需要帳號，不需要登入，馬上開始。

你想檢查什麼？

1. 🛡️ 品牌一致性
   → 品牌語氣、視覺調性、禁用詞、差異化一致性

2. 💰 報價一致性
   → 價格、承諾、優惠條件跨觸點一致性

3. ⚖️ 合規審查
   → 療效承諾、投資保證、過度宣稱、法規風險

4. 🔍 顧客旅程測試
   → 測試完整顧客旅程（頁面、行動呼籲、表單、手機版）

5. 🔐 高風險內容把關
   → 敏感產業必跑的最終檢查

6. 🚀 上線前檢查清單
   → 完整檢查清單，確認一切就緒

💡 完整檢查順序：1 → 2 → 3 → 4 → 5 → 6
💡 快速檢查（上線前）：4 → 6

輸入數字或描述你要檢查的內容。
```

## Flow Logic

- Full audit: run skills in sequence 1 → 2 → 3 → 4 → 5 → 6
- Quick audit: just `journey-qa` → `campaign-ship`
- If sensitive industry detected → auto-include `brand-risk-review` + `careful-publish`
- Each skill outputs a pass/fail → aggregate into final launch readiness

## Platform Awareness
SaleCraft runs on **any AI platform**. Never say "this only works on [specific platform]."

## No Jargon Rule
All of these are pure consultation. Present results in plain language, not technical metrics.
