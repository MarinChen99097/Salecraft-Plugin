# Plugin-Wide Audit Report — 2026-04-21

Audit scope: 4 categories of "content integrity" claims in the plugin (platform-lock claims, fake pricing, fake features, skill count consistency). Wave 1 covers Audits 1 / 2 / 4. Audits 3 and 5 deferred to Wave 2 pending user review of Wave 1.

---

## Audit 1 — 平台鎖 claim

**Status: ✅ PASS**

Grep patterns scanned (English + Chinese):
- `only (work|run|available|supported)`
- `requires.*claude.*code`
- `must (install|have|use).*(claude code|chatgpt|gemini)`
- `this (only|just).*(works|runs) (on|with|in)`
- `platform.specific` / `plugin.only` / `claude.code.only`
- 僅 / 只能 / 此功能僅 / 專為.*Claude Code

**All matches are anti-violation rules** (explicit instructions telling the LLM NOT to platform-lock):

| File | Line | Content |
|---|---|---|
| CLAUDE.md | 665 | "DO NOT tell users they need to install Claude Code... Almost every modern LLM can run the consultation skills as instructions." |
| CLAUDE.md | 1637 | "Platform agnostic — SaleCraft works on ANY AI platform (ChatGPT, Claude, Gemini, Kimi, GLM, OpenClaw, etc.). Never say 'this only works on [platform]'..." |
| commands/salecraft-audit.md | 49 | "SaleCraft runs on any AI platform. Never say 'this only works on [specific platform].'" |
| commands/salecraft-engage.md | 41 | same message |
| commands/salecraft-retain.md | 41 | same message |
| commands/salecraft-strategy.md | 46 | same message |
| commands/salecraft.md | 68, 80 | same message + "there is no installation, ever" |
| prompts/BOOTSTRAP.md | 5 | "SaleCraft works on any AI platform. There is no installation..." |

**Edge case reviewed**:
- CLAUDE.md L659: "This repo is structurally a Claude Code plugin (.claude-plugin/ manifest, commands/ slash commands, skills/ Anthropic-format skills, root CLAUDE.md)" — factual description of repo file layout. Same paragraph immediately clarifies: "When read by other LLMs... this repo functions as an instruction set". **Not a violation** — describing structure is not claiming exclusivity.

**Fix needed**: None.

---

## Audit 2 — 虛假定價

**Status: 🟠 HAS ISSUES** — 1 critical drift + 2 unverified numbers

### Ground truth sources (confirmed consistent)

| Pricing claim | Ground truth | Status |
|---|---|---|
| 1 USD = 30 pts / Min $20 = 600 pts | All sources agree | ✅ |
| LP generation: 200 pts/page × page_count × num_tas | CLAUDE.md pricing table, README, commands/salecraft-create | ✅ |
| Regenerate single stripe: 100 pts | Multiple sources agree | ✅ |
| Quick Ad (single image): 200 pts | Multiple sources agree | ✅ |
| Carousel: 300 + 100×N pts | Consistent formula | ✅ |
| Social Copy: 100 pts/set | Consistent | ✅ |
| Reels: 100 pts/sec (min 5s, max 60s) | Consistent | ✅ |
| SEO optimization: 500 pts (beta free) | Consistent + seo_preflight gate | ✅ |
| QR Code: 5 pts | Mentioned only in pricing table | ✅ |
| `generate_ta_spokesperson`: 0 pts (free quota) | Consolidated across all sources | ✅ |

### 🔴 Critical drift

**`lib/credit-calculator.md` L10-28** uses an **outdated "credits" pricing model** that contradicts all other documentation:

```
total_credits = num_ta_groups × num_aspect_ratios × credits_per_page
```

Examples in that block claim "1 credit" / "2 credits" / "3 credits" per LP — which is nonsense under the current 200-pts-per-page model.

The **same file's bottom section** (L41-85) uses the correct pts model. The top section should be purged.

**Impact**: LLM reading this file sees two contradictory cost models in one document. Depending on which block it references first, could quote 1 credit or 200 pts for the same action.

**Fix**:
- Replace L10-28 with current 200-pts/page formula
- Or delete that block entirely since the bottom section + README/CLAUDE.md pricing tables already cover it

### ⚠️ Unverified numbers

1. **`skills/audience-target/SKILL.md:1042`**: "This skill costs 5-15 pts per TA generation."
   - `generate_ta_options` returns candidates without user credits being deducted (per backend source)
   - The "5-15 pts" figure has no ground truth reference
   - **Recommendation**: Verify with backend or remove this line. Likely wrong (TA candidate generation is free).

2. **`skills/generate-reels/SKILL.md:400`**: "| Scene regeneration | ~50 pts |"
   - No other source documents scene regeneration cost
   - Backend `regenerate_scene` response likely carries `credits_deducted`; plugin should reference that instead of hardcoding ~50 pts
   - **Recommendation**: Replace with "check response `credits_deducted`" language, matching other tools in `lib/credit-calculator.md` VERIFY section.

### 🟡 Minor issues

- **"stripe" vs "page" used interchangeably** in several places (e.g., `skills/generate-landing/SKILL.md:1190` says "200 pts/stripe"). Technical: both are the same thing in backend, but user-facing communication should standardize on "page" per JARGON BLACKLIST.
- **CLAUDE.md Pricing table** (L1599-1609) intentionally duplicates content from `lib/credit-calculator.md`. This is deliberate (quick-reference) but creates drift risk. Already flagged in v3 CLAUDE.md cleanup plan.

### Fix summary

| Priority | File | Fix |
|---|---|---|
| 🔴 P0 | `lib/credit-calculator.md` L10-28 | Delete or replace outdated "credits" model |
| 🟠 P1 | `skills/audience-target/SKILL.md:1042` | Verify or remove "5-15 pts per TA" |
| 🟠 P1 | `skills/generate-reels/SKILL.md:400` | Replace "~50 pts" with "check credits_deducted" |
| 🟡 P2 | Multiple files | Standardize "page" vs "stripe" in user-facing pricing descriptions |

---

## Audit 4 — Skill 數量一致性

**Status: 🟠 HAS ISSUES** — CLAUDE.md off-by-one, README incomplete

### Ground truth

`skills/` directory contains **26 skills**:

```
audience-target, brand-memory, brand-onboard, brand-risk-review,
campaign-ship, careful-publish, conversion-closer, document-release,
edit-landing, engage-operator, generate-landing, generate-reels,
growth-retro, guard-brand, guard-offer, homepage-builder, journey-qa,
market-intel, member-lifecycle, plan-cgo-review, plan-funnel-review,
publish-ads, publish-social, research-market, saleskit, upload-media
```

### Discrepancies

| File | Claimed count | Actually lists | Missing skills |
|---|---|---|---|
| `skills/` directory | — | **26** | (ground truth) |
| `CLAUDE.md` L1435 header: "Available Skills (25)" | 25 | 25 | `upload-media` |
| `README.md` L80 header: "Skills (26)" | 26 (header correct) | 24 (body incomplete) | `brand-memory`, `upload-media` |

### Fix summary

| File | Action |
|---|---|
| `CLAUDE.md` L1435 | Change "Available Skills (25)" → "(26)"; add `upload-media` to appropriate tier |
| `README.md` L80 region | Add `brand-memory` + `upload-media` to skill tables |

### Note on missing skills

- **`brand-memory`** (CLAUDE.md already has it at L1495): "Auto-record files, prompts, and metadata per brand for personalized experience" — runs silently in background. README should mention it even if as "background skill, no user interaction needed".
- **`upload-media`** (exists as directory): Needs a line confirming what it does and when triggered. If it's an internal utility skill, both CLAUDE.md and README should document it.

---

## Wave 1 Summary

| Audit | Status | Issues found |
|---|---|---|
| 1. Platform lock | ✅ PASS | 0 violations |
| 2. Fake pricing | 🟠 1 critical + 2 unverified + minor stripe/page drift | 4 fixes |
| 4. Skill count | 🟠 Off-by-one in CLAUDE.md, 2 skills missing from README | 2 fixes |

**Critical (P0)**: 1 item (outdated credits model in `lib/credit-calculator.md`)
**High (P1)**: 3 items (2 unverified prices, skill count fixes)
**Minor (P2)**: 1 item (stripe/page standardization)

Total fixes: **~6 distinct edits**, all low-risk and localized. No file structure changes needed, no SKILL behavior changes needed.

---

## Wave 2 Preview (deferred pending user review)

**Audit 3** — Virtual features: each plugin claim must have corresponding skill + MCP tool + command. Previously caught `i18n-adapt` fabrication and `get_suggested_buttons` misuse. Need to walk through remaining feature claims in README Features section and SKILL description frontmatters.

**Audit 5** — Backend coverage: plugin references 74/311 landing_ai_mcp tools. Of the 237 unreferenced, some are intentionally internal (backfill_gcs_urls, etc.) but others may be user-facing features that plugin doesn't surface. Requires category-by-category sampling.

---

## Next step (recommended)

1. User reviews this report
2. If Wave 1 findings accepted, apply 6 fixes as a single commit (all localized, low-risk)
3. After Wave 1 fixes land and validate, decide whether to run Wave 2 (Audit 3 + 5)
