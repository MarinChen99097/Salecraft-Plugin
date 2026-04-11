---
name: mx-publish
description: "Publish to social media and/or run Meta/Google ad campaigns"
---

# /mx-publish — Social Publishing + Ad Campaigns

Read `CLAUDE.md` for tool signatures.

## Quick Start

1. Authenticate if needed
2. Ask what the user wants:
   ```
   Publishing options:

   A) 📱 Social Media — Post to connected platforms
   B) 💰 Ad Campaign — Create Meta/Google ad campaigns
   C) 🚀 Both — Social post + ad campaign

   Which would you like?
   ```

3. Based on selection:
   - **A**: Invoke `publish-social` skill
   - **B**: Invoke `publish-ads` skill
   - **C**: Run both sequentially

## Prerequisites
- Need `session_id` and `campaign_id` from a generated LP
- If not provided, list recent sessions and let user select
