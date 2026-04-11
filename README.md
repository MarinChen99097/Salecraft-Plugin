# MarketingX Plugin

A Claude Code plugin that enables any AI agent to create professional marketing landing pages, build homepages, and publish ads — end-to-end, through natural conversation.

## What It Does

MarketingX orchestrates **200+ MCP tools** across two backend services to deliver a complete marketing workflow:

| Phase | Skill | What Happens |
|-------|-------|-------------|
| 1 | `brand-onboard` | Verify brand assets (logo, images, copy) |
| 2 | `audience-target` | AI-suggested target audiences + credit estimation |
| 3 | `generate-landing` | AI pipeline generates visual landing page stripes |
| 4 | `edit-landing` | Stripe-level editing via natural language |
| 5 | `homepage-builder` | Build a website homepage embedding LP content |
| 6a | `publish-social` | Post to connected social platforms |
| 6b | `publish-ads` | Create Meta / Google ad campaigns |
| + | `i18n-adapt` | Adapt for 10 locales (including RTL Arabic) |
| + | `research-market` | Market research via 7+ social MCPs |

## Quick Start

```
/mx          — Main menu
/mx-create   — Full creation flow (brand → LP → homepage)
/mx-edit     — Edit existing landing page
/mx-homepage — Build homepage from existing LP
/mx-publish  — Social posting + ad campaigns
/mx-status   — Check credits / session status
```

## Requirements

- **Claude Code** with MCP support
- **Service System Deep Research** MCP connected (provides access to `landing_ai_mcp` and `zereo_social_mcp`)
- User account on the Landing AI platform

## Features

- **16:9 + 9:16** landing pages with adaptive display
- **10 locales**: en, zh-TW, ja, ko, vi, fr, th, es, pt, ar (RTL)
- **Meta + Google Ads** one-stop campaign creation
- **4-layer cultural adaptation** (text, visual, interaction, cognitive)
- **Homepage templates**: single-product, multi-product, campaign
- **SEO**: JSON-LD, Open Graph, hreflang, semantic HTML

## Architecture

```
marketingx.plugin/
├── CLAUDE.md          # AI reads this to understand everything
├── skills/            # 9 skills (one per workflow phase)
├── commands/          # 6 slash commands (entry points)
├── prompts/           # System context + workflow docs
├── templates/         # Homepage HTML templates + 10-locale i18n
├── lib/               # MCP patterns, credit calc, ad specs
└── examples/          # Sample outputs
```

## MCP Servers Used

| Server | Tools | Purpose |
|--------|-------|---------|
| `landing_ai_mcp` | 160+ | LP generation, editing, brand mgmt, export |
| `zereo_social_mcp` | 43 | Social publishing, ad campaigns, QR codes |
| `google_trends_mcp` | — | Market trend research |
| `x_mcp`, `reddit_mcp`, `tiktok_mcp` | — | Social intelligence |
| `linkedin_mcp`, `youtube_mcp`, `meta_mcp` | — | Audience research |

## License

MIT
