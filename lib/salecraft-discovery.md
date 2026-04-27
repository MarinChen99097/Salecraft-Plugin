# SaleCraft Discovery — Shared Question Library

> **Reference doc** consumed by `/salecraft-post-consultant`, `/salecraft-identity-consultant`, and
> `/salecraft-reels-consultant`. Defines the standard 6-question discovery flow that produces a
> structured `DesignBrief` for the marketing_backend pipeline.
>
> Skills `Read` this file (NOT invoke as a sub-skill) — it's pure question + mapping reference.

## Discovery flow contract

The consultant skill walks the user through these questions in order, branching based on prior
answers. Use `AskUserQuestion` for every step (not free-text prompts). Skip a question entirely
when its answer is already in `brand_memory` or has been provided in conversation context.

Goal: produce a `DesignBrief` JSON ready to POST to marketing_backend's `/sessions/{id}/generate-ad`,
`/sessions/{id}/generate-carousel`, or `/brands/{id}/identity/generate`.

---

## Q1 — Goal (maps to `purpose.funnel_stage` + `purpose.primary_kpi`)

> 「這個東西做出來，你最想達成什麼？」

| Option | Maps to funnel_stage | primary_kpi |
|---|---|---|
| 讓更多人認識我們 / 衝聲量 | tofu | reach |
| 讓人滑下去看 / 想點進來看細節 | mofu | engagement / ctr |
| 立刻買 / 立刻報名 / 立刻下載 | bofu | cvr |
| 讓老顧客回購 / 推薦朋友 | retention | save / share |

Sub-question if option 3 (BOFU): urgency level → `purpose.urgency_level`
- "限時 24 小時" → `hard`
- "本週優惠" → `soft`
- "純購買引導" → `none`

---

## Q2 — Audience (maps to `audience` block)

> 「這是給誰看的？」

Offer 4 archetype starting points + custom:

| Option | Pre-fills |
|---|---|
| 25-34 歲都市專業女性，注重品質 | age=(25,34), gender_skew=female, income=premium, lifestyle=[urban_professional, quality_seeker] |
| 18-24 歲學生 / 社會新鮮人 | age=(18,24), gender_skew=balanced, income=mass, lifestyle=[student, gen_z, value_conscious] |
| 35-50 歲家長 | age=(35,50), gender_skew=balanced, income=mid, lifestyle=[parent, family_first] |
| B2B 決策者 | age=(30,55), gender_skew=balanced, income=premium, lifestyle=[professional, decision_maker], decision_style=committee |
| 自訂 | prompt for free-form |

If user picks "自訂", ask 3 follow-ups: age range, gender, lifestyle keywords (3 max).

If `brand_memory` already has an audience profile for this brand, present it as the default
("based on past campaigns, your audience tends to be...") and ask if it still applies.

---

## Q3 — Awareness (CRITICAL — maps to `audience.schwartz_awareness_level`)

> 「你的觀眾現在對你 / 你的產品認識到哪一步？」

| Option | Schwartz level |
|---|---|
| 完全沒聽過我們，連這種產品也沒在想 | unaware |
| 知道有這個問題，但還沒在找解法 | problem_aware |
| 知道有解法，但不知道我們是誰 | solution_aware |
| 認識我們，但還沒下決定 | product_aware |
| 已經在猶豫要不要買 | most_aware |

This single answer drives the Architect's copy register. **Do not skip it.**

---

## Q4 — Format (maps to `image_count`, `aspect_ratios`, `audience.primary_platforms`, `post_format`)

> 「要做幾張？要直的、方的，還是橫的？放在哪裡？」

| Platform | Default aspect | Default count |
|---|---|---|
| IG / FB Story | 9:16 | 1 |
| IG / FB Feed (single) | 4:5 | 1 |
| IG Carousel | 1:1 or 4:5 | 5-7 |
| Threads | 4:5 | 1-3 |
| Xiaohongshu (小紅書) | 3:4 (use 4:5) | 4-6 |
| LinkedIn | 1:1 or 16:9 | 1 |
| TikTok cover | 9:16 | 1 |

If `image_count > 1` → branches into Q4b: carousel narrative type
- 問題→解法 → `problem_solution`
- Before / After → `before_after`
- 步驟拆解 → `step_by_step`
- 倒數清單 (5 件事) → `list_countdown`
- 故事推進 → `story_arc`
- 問答 → `question_answer`
- A vs B 比較 → `comparison`

---

## Q5 — Visual mood (maps to `visual_direction`)

Show 4 mood reference image URLs (chosen from a curated mood-board library). Ask which one
fits. Each option pre-fills:

| Mood option | style_keywords | mood | composition_preset | background_type |
|---|---|---|---|---|
| Editorial 雜誌風 | [editorial, kinfolk, restrained] | calm | rule_of_thirds | photo |
| Cinematic 電影感 | [cinematic, dramatic, anamorphic] | dramatic | golden_ratio | photo |
| Minimal 極簡 | [minimal, swiss, geometric] | calm | centered | solid |
| Maximalist 大膽繽紛 | [maximalist, y2k, vibrant] | energetic | asymmetric | abstract |

If user wants something else → free-form `style_keywords` (3-5 words).

If `brand_memory.visual_direction` exists → present as default with "use the same look as
last time?".

---

## Q6 — Constraints (maps to `negative_directives`, `must_include`, `compliance_industry`)

> 「有什麼一定要避免的？或一定要出現的？」

Two-part question:
- 「不要出現的東西」(negative_directives) — free-form, comma-separated, optional
- 「一定要包含的東西」(must_include) — free-form, comma-separated, optional

Auto-detect compliance industry from brand profile (industry field). If detected, surface:
"This is a {industry} brand — we'll auto-apply {industry} compliance rules. Anything else?"

---

## Brief assembly

After Q1-Q6, build the `DesignBrief` JSON. Required fields with no user input:
- `cultural_context.region`: derive from `audience.geo` or `brand.default_language`
- `cultural_context.information_density_preference`: `high` if region in {tw, hk, cn, jp, kr}, else `medium`
- `purpose.big_idea`: ask user for 1 sentence ("這次的核心訊息是？") OR leave for BriefEnricher
- `purpose.primary_message`: ask user OR leave for BriefEnricher
- `purpose.target_emotion`: infer from purpose + audience or ask Q5b explicitly
- `purpose.persuasion_levers`: leave empty → BriefEnricher fills based on funnel_stage

## Confirmation step

Before POSTing, show the user a human-readable summary in Traditional Chinese (or their language):

```
我幫你整理了:
• 給 25-34 歲都市專業女性 (problem_aware 階段)
• 目的: 引發興趣 (mofu)
• 5 張 1:1 IG carousel (problem_solution 結構)
• 風格: editorial 雜誌風，calm 調性
• 不要出現: {negatives}
• 一定要有: {must_include}
• 預估點數: {credits}

確認嗎？
```

Use `AskUserQuestion` with options: "確認 / 修改 / 取消".

## API call

```
POST {MARKETING_BACKEND_URL}/sessions/{session_id}/generate-ad
Authorization: Bearer {user_token}
Body: { "design_brief": <full DesignBrief JSON> }
```

For carousels (image_count > 1):
```
POST {MARKETING_BACKEND_URL}/sessions/{session_id}/generate-carousel
Body: { "design_brief": <full DesignBrief JSON> }
```

Then poll `GET /sessions/{session_id}/ad-result/{project_id}` every 5s until `status == "completed"`.

## Memory write-back

On user approval of the result, write back to brand_memory:
- The selected mood / style_keywords (for next campaign default)
- Any audience refinements
- High-scoring `rules_applied` combinations from `qc_details.image_layouts_summary`

## Error handling

- `402 INSUFFICIENT_CREDITS` → tell user the cost + their balance, offer `/salecraft-status`
- `422 either ta_group_id or design_brief required` → schema mismatch; rebuild brief
- `BUDGET_CEILING_EXCEEDED` → ask user to either raise the ceiling or reduce image_count
- Polling timeout (>5 min, status still "processing") → keep polling, warn at 10 min,
  recommend `/salecraft-status` at 15 min
