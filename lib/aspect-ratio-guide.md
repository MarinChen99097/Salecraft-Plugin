# Aspect Ratio Guide — 16:9 vs 9:16

## Supported Ratios

| Ratio | Name | Use Case | Best For |
|-------|------|----------|----------|
| **16:9** | Landscape | Desktop web, presentations, YouTube ads | Desktop-first websites, B2B |
| **9:16** | Portrait | Mobile stories, TikTok, Instagram Reels | Mobile-first, social media, DTC |

## Generation

Set aspect ratio in session configuration before triggering generation:

```
update_session(user_token, session_id, aspect_ratio="16:9")
# OR
update_session(user_token, session_id, aspect_ratio="9:16")
```

For **both** ratios: create two separate sessions with the same brand/TA config.

## Homepage Embedding — CSS

### 16:9 (Landscape) Embedding

```css
.lp-embed[data-ratio="16:9"] {
  aspect-ratio: 16 / 9;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
}

/* Mobile: stack naturally */
@media (max-width: 768px) {
  .lp-embed[data-ratio="16:9"] {
    aspect-ratio: auto;
    width: 100%;
  }
}
```

**Desktop**: Full-width hero sections, side-by-side content blocks.
**Mobile**: Stacks vertically, maintains readability.

### 9:16 (Portrait) Embedding

```css
.lp-embed[data-ratio="9:16"] {
  aspect-ratio: 9 / 16;
  max-width: 480px;
  margin: 0 auto;
}

/* Mobile: full width, natural scroll */
@media (max-width: 768px) {
  .lp-embed[data-ratio="9:16"] {
    aspect-ratio: auto;
    width: 100%;
    max-width: 100%;
  }
}
```

**Desktop**: Centered column with max-width 480px, flanking elements optional.
**Mobile**: Full-width, natural vertical scroll (native feel).

### Adaptive Container (Auto-Detect)

```css
.lp-embed-container {
  container-type: inline-size;
  width: 100%;
}

/* Use container queries for fine-grained control */
@container (min-width: 769px) {
  .lp-embed[data-ratio="9:16"] {
    max-width: 480px;
    margin: 0 auto;
    box-shadow: 0 4px 24px rgba(0, 0, 0, 0.1);
    border-radius: 12px;
    overflow: hidden;
  }
}
```

## Image Export Sizes

| Ratio | Export Width | Export Height | Format |
|-------|-------------|---------------|--------|
| 16:9 | 1920px | 1080px | WebP/PNG |
| 9:16 | 1080px | 1920px | WebP/PNG |

Stripe images are exported per-stripe via `get_export_image_url(user_token, campaign_id, stripe_idx)`.

## Ad Creative Sizing

| Platform | Required Ratio | Size |
|----------|---------------|------|
| Meta Feed | 1:1 | 1080×1080 |
| Meta Stories | 9:16 | 1080×1920 |
| Google Display | Various | Responsive |
| TikTok Feed | 9:16 | 1080×1920 |

The `generate_ad` tool automatically adapts LP content to platform-specific ratios.
