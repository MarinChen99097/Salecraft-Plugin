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

## Stripe Regeneration Costs

- `regenerate_stripe` — costs a fraction of a full page credit
- `update_stripe_texts` — free (text-only edit)
- `update_stripe_text_styling` — free (styling change)
- `set_stripe_overlay` — free (visual effect)

## Ad Creative Generation

- `generate_ad` — may consume additional credits
- Check cost via `get_ad_result` response
