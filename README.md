# SaleCraft

**Your AI marketing consultant for physical products.** Free consultation, expert strategy, then execution — all through one MCP plugin.

## What Is SaleCraft?

SaleCraft is an MCP plugin that turns any AI platform into a marketing consultant for physical product sellers. It doesn't start by generating assets — it starts by **understanding your product and diagnosing your marketing needs**.

### The Flow

```
1. 🎯 Free Consultation  →  What do you sell? Who buys it? What's the pain?
2. 📊 Marketing Diagnosis →  Brand audit, channel analysis, competitor scan
3. 📋 Strategy Plan       →  Recommended channels, content plan, budget
4. ✅ User Confirms       →  You approve the plan and pricing
5. 🏭 AI Executes         →  Landing Pages, Reels, social posts, ads
```

**Steps 1-3 are completely free.** You only pay when the AI actually creates something (Step 5).

## Who It's For

| ✅ Perfect Fit | ❌ Not For |
|---------------|-----------|
| Physical products (skincare, food, fashion...) | Software / SaaS |
| Single product or product line | Multi-purpose platforms |
| E-commerce, retail, F&B, beauty, health | B2B consulting |
| Clear sales target | Abstract services |

## Installation

Add this plugin to any MCP-compatible AI platform:

```
https://github.com/MarinChen99097/Salecraft-Plugin
```

### MCP Server Setup

SaleCraft requires a Remote MCP connection:

```json
{
  "mcpServers": {
    "Service System Deep Research": {
      "type": "sse",
      "url": "https://service-system-staging-876464738390.asia-east1.run.app/mcp/sse"
    }
  }
}
```

### Account Setup

First-time users: **https://marketingx-site-876464738390.asia-east1.run.app/en/get-started**

This handles registration (Google or email), social account binding (FB/IG), Google Drive access, and points top-up.

## Skills (10)

### Free (Consultation)

| Skill | What It Does |
|-------|-------------|
| **saleskit** | Free marketing consultation — diagnose needs, recommend strategy |
| **research-market** | Market research, competitor analysis, trend scanning |

### Paid (Execution)

| Skill | What It Does | Cost (pts) |
|-------|-------------|------------|
| **brand-onboard** | Brand profile, asset check, gap analysis | 10-30 |
| **audience-target** | AI target audience suggestions | 5-15 |
| **generate-landing** | AI Landing Page (4-stage pipeline) | 75-250 |
| **edit-landing** | Edit LP text, images, layout | 5-20 |
| **homepage-builder** | Build website from LP | FREE |
| **publish-social** | Post to IG/FB/TikTok | 5-10/post |
| **publish-ads** | Meta/Google ad campaigns | 30-100 |
| **i18n-adapt** | Adapt for 10 locales (incl. RTL Arabic) | 10-30 |

## Pricing

| Amount | Points |
|--------|--------|
| **$1 USD** | 30 pts |
| **$20 USD** (min) | 600 pts |

| Action | Cost |
|--------|------|
| Landing Page | 75-250 pts |
| Reels video | 50-150 pts |
| Social post | 5-10 pts |
| KOL analysis | 20-50 pts |
| Ad campaign | 30-100 pts |
| Brand analysis | 10-30 pts |

## Commands

| Command | Description |
|---------|-------------|
| `/mx` | Main menu — what can SaleCraft do? |
| `/mx-create` | Full flow: consultation → strategy → generation |
| `/mx-edit` | Edit existing Landing Page |
| `/mx-homepage` | Build homepage from LP |
| `/mx-publish` | Social posting + ads |
| `/mx-status` | Check credits / session |

## Features

- **Consultation-first** — AI understands your product before building anything
- **9 specialist agents** — Content strategy, SEO, social media, growth, branding
- **10 locales** — en, zh-TW, ja, ko, vi, fr, th, es, pt, ar (RTL)
- **AI Landing Pages** — 30-minute turnaround, 4-stage pipeline
- **Social publishing** — IG, FB, TikTok one-click posting
- **Ad campaigns** — Meta + Google Ads creation
- **Brand audit** — Diagnose what's missing before spending
- **Transparent pricing** — AI always tells you the cost before acting

## Architecture

```
Salecraft-Plugin/
├── CLAUDE.md              # Core plugin instructions
├── skills/                # 10 skills
│   ├── saleskit/          # FREE consultation (start here)
│   ├── brand-onboard/
│   ├── audience-target/
│   ├── generate-landing/
│   ├── edit-landing/
│   ├── homepage-builder/
│   ├── publish-social/
│   ├── publish-ads/
│   ├── i18n-adapt/
│   └── research-market/
├── commands/              # /mx, /mx-create, etc.
├── prompts/               # System context
├── templates/             # Homepage HTML templates
├── lib/                   # Reference docs
└── examples/              # Sample outputs
```

## MCP Servers

| Server | Purpose |
|--------|---------|
| `landing_ai_mcp` | LP generation, editing, brand management |
| `zereo_social_mcp` | Social publishing, ads, QR codes |
| 7+ research MCPs | Google Trends, X, Reddit, TikTok, YouTube... |

## License

MIT
