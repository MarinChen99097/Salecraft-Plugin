# Credit Calculator Reference

## How Credits Work

- Each user has a weekly credit allocation
- Credits are consumed on `generate_session` (LP generation)
- Credits auto-refund if generation is interrupted
- Check balance: `get_me(user_token)` → `credits_remaining`

## Cost Estimation Formula

```
total_credits = num_ta_groups × num_aspect_ratios × credits_per_page
```

Where:
- `num_ta_groups`: number of selected target audiences (typically 1-5)
- `num_aspect_ratios`: 1 (single) or 2 (both 16:9 + 9:16)
- `credits_per_page`: base cost per landing page (check `get_generation_settings`)

## Example Calculations

| Scenario | TAs | Ratios | Per Page | Total |
|----------|-----|--------|----------|-------|
| Single product, 1 TA | 1 | 1 (16:9) | 1 | 1 credit |
| Single product, both ratios | 1 | 2 | 1 | 2 credits |
| Multi-TA campaign | 3 | 1 | 1 | 3 credits |
| Full campaign, both ratios | 3 | 2 | 1 | 6 credits |

## Pre-Generation Check

Before calling `generate_session`, always:

1. Call `get_me(user_token)` to get `credits_remaining`
2. Calculate `total_credits` needed
3. If `credits_remaining < total_credits`:
   - Inform user: "You need X credits but only have Y remaining"
   - Suggest reducing TAs or aspect ratios
   - Do NOT proceed with generation

## Post-Generation Edit Tool Cost Reference

**🔴 Critical rule**: Never extrapolate a tool's cost from another tool's price. If it's not explicitly listed below, **check `get_generation_settings` or run a preflight** (e.g. `seo_preflight`) before quoting a cost to the user. See the anti-pattern in CLAUDE.md rule 6.5 ("cost extrapolation from tool name").

### FREE — config-only edits (no regeneration, no AI pipeline re-run)

| Tool | What it does |
|------|-----|
| `update_stripe_text` / `update_stripe_texts` | Replace text on a stripe |
| `update_stripe_text_styling` / `update_stripe_text_boxes` | Font / size / color / position of text |
| `update_cta` / `update_cta_link` / `update_cta_style` | CTA button text / URL / style |
| `set_stripe_overlay` | Semi-transparent color layer for text readability |
| `set_stripe_soft_edge` | Gradient fade between stripes |
| `crop_stripe` / `reset_crop` | Change visible rectangle of the image (no re-render) |
| `update_stripe_background` | Change background color / image |
| `hide_stripe` / `restore_stripe` | Toggle visibility |
| `reorder_stripes` | Change stripe order |
| `update_image_layers` | Swap uploaded images into image layers |
| `upload_logo` | Change the header logo at top-left |
| `patch_landing_config` | Generic config patch (header_nav / footer / primary_color / …) |
| `mask_stripe` / `edit_regions` | Region-specific image edits (may have paid variants — verify via response) |

### PAID — actually re-runs AI pipeline

| Tool | Cost |
|------|-----|
| `regenerate_stripe` | 100 pts / stripe（re-runs Factory Agent on that stripe）|
| `regenerate_scene` (reels) | check response `credits_deducted` |
| `regenerate_project_stripe` / `regenerate_campaign_stripe` | check response |
| `run_seo_optimize` | 500 pts（beta: free、always `seo_preflight` first to confirm actual cost） |

### VERIFY BEFORE QUOTING — unknown / conditional cost

| Tool | How to check |
|------|-----|
| `generate_ad` | response includes `credits_deducted` |
| `generate_carousel` | response includes `credits_deducted` |
| `generate_reels` | 100 pts / sec（confirmed）|
| Any new tool not listed above | call once in preview mode if available, OR ask backend team, OR tell user "this action may deduct credits — let me check before I run it" |

## Ad Creative Generation

- `generate_ad` — may consume additional credits
- Check cost via `get_ad_result` response
