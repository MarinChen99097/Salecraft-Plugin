---
name: salecraft-create
description: "Full creation flow: brand onboard → audience target → generate LP → edit → build homepage"
---

# /salecraft-create — Full Landing Page + Homepage Creation

Read `CLAUDE.md` and `prompts/WORKFLOW.md` for complete workflow reference.

Execute these phases in order:

## Phase 1: Brand Onboarding
Invoke the `brand-onboard` skill.
- Authenticate user via **AI Token** (NEVER email/password): hand them `https://salecraft.ai/{locale}/marketingx`, ask them to click 「複製 AI 登入 Token」 and paste `sc_live_...`, then call `authenticate_with_token`
- Verify brand assets
- Fill gaps if needed
- Output: `brand_id`, `user_token`

## Phase 2: Audience Targeting
Invoke the `audience-target` skill.
- AI-suggested TAs
- User selects TAs + aspect ratio (16:9 / 9:16 / both)
- Credit estimation + confirmation
- Output: `ta_groups`, `aspect_ratio`, `locale`

## Phase 3: LP Generation
Invoke the `generate-landing` skill.
- Create session, trigger AI pipeline
- Poll for completion (30-120 seconds)
- Quality verification
- Output: `campaign_id`, `session_id`

## Phase 4: Editing (Iterative)
Invoke the `edit-landing` skill.
- Present generated stripes
- Accept natural language edit requests
- Loop until user says "done"

## Phase 5: Homepage Building
Invoke the `homepage-builder` skill.
- Choose layout template
- Embed LP stripes
- Apply i18n + aspect ratio CSS
- Output: HTML/CSS/JS files

After completion, suggest: "要不要把這個頁面發佈到社群？用 /salecraft-publish 就可以囉！"

## Cross-Phase State
Carry these values across all phases — never re-ask:
- `user_token`, `brand_id`, `ta_groups`, `aspect_ratio`, `locale`
- `session_id`, `campaign_id`, `landing_page_id`

## Login Awareness (AI Token only)
**Authenticate via AI Token — NEVER email/password.** Direct user to `https://salecraft.ai/{locale}/marketingx` (replace `{locale}` to their language), have them click 「複製 AI 登入 Token」, paste back `sc_live_...`, then call `authenticate_with_token`. If they have no account, the same page handles registration.

## No Jargon Rule
Never mention MCP, tokens, skills, or technical internals to users. Just do the work.
