# Ad Platform Specifications

## Meta (Facebook + Instagram)

### Ad Objectives
| Objective | When to Use |
|-----------|-------------|
| AWARENESS | Brand recognition, reach new audiences |
| TRAFFIC | Drive visitors to landing page |
| ENGAGEMENT | Likes, comments, shares |
| LEADS | Form submissions, sign-ups |
| CONVERSIONS | Purchases, add-to-cart |
| APP_INSTALLS | Mobile app downloads |

### Creative Formats
| Format | Dimensions | Ratio | Max File Size |
|--------|-----------|-------|--------------|
| Feed Image | 1080×1080 | 1:1 | 30MB |
| Feed Video | 1080×1080 | 1:1 | 4GB |
| Stories/Reels | 1080×1920 | 9:16 | 30MB |
| Carousel | 1080×1080 each | 1:1 | 30MB/card |
| Right Column | 1200×628 | ~1.91:1 | 30MB |

### Text Limits
| Field | Characters |
|-------|-----------|
| Primary text | 125 (recommended), 2200 (max) |
| Headline | 27 (recommended), 255 (max) |
| Description | 27 (recommended), 255 (max) |

### CTA Types
`LEARN_MORE`, `SHOP_NOW`, `SIGN_UP`, `BOOK_NOW`, `CONTACT_US`, `DOWNLOAD`, `GET_OFFER`, `GET_QUOTE`, `SUBSCRIBE`, `WATCH_MORE`

## Google Ads

### Campaign Types
| Type | When to Use |
|------|-------------|
| Search | Intent-based keywords, high-conversion |
| Display | Visual awareness, remarketing |
| Performance Max | AI-optimized across all placements |
| Video | YouTube pre-roll, in-stream |

### Responsive Display Ads
| Asset | Count | Max Length |
|-------|-------|-----------|
| Headlines | 1-5 | 30 chars each |
| Long headline | 1 | 90 chars |
| Descriptions | 1-5 | 90 chars each |
| Images | 1-15 | 1200×628 (landscape), 1200×1200 (square) |
| Logo | 1-5 | 1200×1200 |

### Responsive Search Ads
| Asset | Count | Max Length |
|-------|-------|-----------|
| Headlines | 3-15 | 30 chars each |
| Descriptions | 2-4 | 90 chars each |
| Display URL path | 2 | 15 chars each |

## Workflow: LP → Ad Creative

1. **Extract from LP**: Use `generate_ad(user_token, session_id, platform)` via `landing_ai_mcp`
   - Automatically extracts headlines, descriptions, and images from LP stripes
   - Adapts to platform-specific formats and sizes
   - Generates multiple variants for A/B testing

2. **Review creative**: `get_ad_result(user_token, task_id)` — poll until ready
   - Returns: creative assets (images), copy variants, CTA recommendation

3. **Create campaign**: `create_ad_campaign(user_token, data_json)` via `zereo_social_mcp`
   - Required: account_id, objective, budget, creative, landing_url
   - Optional: targeting, schedule, bid_strategy

4. **Monitor**: `get_ad_campaign(user_token, campaign_id)` — check performance metrics

## Budget Recommendations

| Objective | Min Daily Budget (USD) | Recommended |
|-----------|----------------------|-------------|
| Awareness | $5 | $20-50 |
| Traffic | $10 | $30-100 |
| Conversions | $20 | $50-200 |
| Leads | $15 | $40-150 |

Note: These are starting points. Actual budgets depend on market, audience size, and competition.
