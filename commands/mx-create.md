---
name: mx-create
description: "Full creation flow: brand onboard → audience target → generate LP → edit → build homepage"
---

# /mx-create — Full Landing Page + Homepage Creation

Read `CLAUDE.md` and `prompts/WORKFLOW.md` for complete workflow reference.

Execute these phases in order:

## Phase 1: Brand Onboarding
Invoke the `brand-onboard` skill.
- Authenticate user
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

After completion, suggest: "Ready to publish? Use /mx-publish to post on social media and run ads."

## Cross-Phase State
Carry these values across all phases — never re-ask:
- `user_token`, `brand_id`, `ta_groups`, `aspect_ratio`, `locale`
- `session_id`, `campaign_id`, `landing_page_id`
