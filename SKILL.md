---
name: infographic
description: >
  Generate professional infographic images from articles, tweets, or any structured content.
  Use this skill when the user asks to create an infographic, visual summary, knowledge card,
  or sharable image from an article or topic. Also triggers on: "生成信息图", "做张图",
  "总结成图片", "visual summary", "knowledge card", "shareable image for WeChat/social media".
  Produces high-DPI PNG images optimized for mobile viewing and social sharing (WeChat, Telegram, Twitter).
  NOT for: AI-generated art, photos, illustrations, or anything requiring image generation models.
---

# Infographic Skill

Create professional, mobile-friendly infographic images from structured content using HTML + Playwright screenshot.

## When to Use

- User shares an article/tweet and asks for an infographic or visual summary
- User wants a shareable image for WeChat groups, Telegram, Twitter, etc.
- User asks for a "knowledge card", "one-pager", or "visual breakdown"

## Workflow

```
Article/Content → Extract Key Points → Design HTML → Screenshot (3x) → Deliver PNG
```

### Step 1: Extract & Structure Content

Analyze the source material and identify:
- **Title** (bilingual if applicable: Chinese primary, English subtitle)
- **Core thesis** (1-2 sentence summary for callout box)
- **Main sections** (3-7 sections, each with title + 1-3 bullet points)
- **Key insight or takeaway** (for the dark bottom section)
- **Source attribution** (author, platform, URL)

Keep text concise — infographics are scannable, not readable. If a section needs more than 3 short lines, split it.

### Step 2: Design the HTML

Create a single self-contained HTML file (no external dependencies). Read `{baseDir}/references/design-system.md` for the full color palette, typography, and component library.

**Page setup:**
```html
<!DOCTYPE html>
<html lang="zh">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
* { margin: 0; padding: 0; box-sizing: border-box; }
body {
  width: 100%;
  background: #faf6f0;
  font-family: -apple-system, 'PingFang SC', 'Noto Sans SC', sans-serif;
  padding: 32px 20px;
  font-size: 17px;
  line-height: 1.6;
  color: #444;
}
</style>
</head>
```

**Key rules:**
- **Phone-first**: screenshot viewport is `390px` at `3x` → 1170px wide PNG, crisp on any phone
- No fixed height — content determines length, no wasted whitespace
- Font sizes (phone, primary): body `17px`, section title `19px`, main title `32px`, small labels `13px`
- Background: `#faf6f0` (cream) or `#fff` — never transparent
- **Use CSS classes, not all-inline styles** — inline styles cannot use media queries
- Chinese font stack: `-apple-system, 'PingFang SC', 'Noto Sans SC', sans-serif`
- Grid columns: `repeat(auto-fit, minmax(140px, 1fr))` — naturally stacks to 1-col at 350px, 2-col at 390px
- **Do NOT set `max-width`** on body — let it fill the full 390px viewport

**Layout pattern (adapt to content):**
1. **Header**: badge + title + subtitle + divider
2. **Callout box**: core thesis in yellow box with gold left border
3. **Content cards**: numbered sections in a unified QA block (one container, inline badges, no side circles)
4. **Takeaway box**: dark background, key conclusions
5. **Footer**: source attribution + brand

Use the design system reference for exact colors, spacing, and component HTML.

### Step 3: Screenshot with Playwright

Use the bundled screenshot script:

```bash
node {baseDir}/scripts/screenshot.js /tmp/infographic.html /path/to/output.png 3 390
```

Arguments: `<input.html> <output.png> [scaleFactor=3] [viewportWidth=390]`

> **Phone-first**: 390px viewport at 3x = 1170px wide PNG — fills a phone screen at natural size.

The script automatically finds Playwright in the npx cache or global install.

**If the Node script fails**, try Python Playwright (works when installed via Homebrew/pip):
```bash
python3 - <<'EOF'
from playwright.sync_api import sync_playwright
import os

with sync_playwright() as p:
    browser = p.chromium.launch()
    ctx = browser.new_context(viewport={"width": 390, "height": 600}, device_scale_factor=3)
    page = ctx.new_page()
    page.goto("file:///tmp/infographic.html", wait_until="networkidle")
    page.locator("body").screenshot(path="/tmp/output.png", type="png")
    box = page.locator("body").bounding_box()
    size = os.path.getsize("/tmp/output.png")
    print(f"✅ {int(box['width']*3)}×{int(box['height']*3)}px  ({size//1024}KB)")
    browser.close()
EOF
```

**Never use `npx playwright screenshot` CLI** — it doesn't support `deviceScaleFactor` and produces blurry 1x images.

### Step 4: Deliver

**Telegram delivery (lossless):**

OpenClaw's `message send` routes `.png` files through Telegram's `sendPhoto` API, which compresses images to max 2048px on the longest side. To deliver the original high-res image, use `sendDocument` directly:

```bash
bash {baseDir}/scripts/send_telegram.sh /path/to/output.png "Caption text"
```

Or with the `message` tool if the platform doesn't compress (Discord, etc.), just send normally.

**Other platforms:** Most platforms preserve document uploads. Use the `message` tool with `filePath` for non-Telegram delivery.

## Design Principles

1. **Scannable, not readable** — if someone has to zoom in to read, there's too much text
2. **Visual hierarchy** — title → callout → numbered cards → takeaway. Eyes should flow top-to-bottom naturally
3. **Consistent color coding** — each section gets one accent color from the palette, don't mix randomly
4. **Mobile-first** — 750px width at 3x = 2250px physical. Looks crisp on any phone
5. **Self-contained** — single HTML file, no external resources, works offline

## Customization

- For **English-only** content: same layout, adjust font stack to include serif options
- For **data-heavy** content: use the 2×2 grid pattern or horizontal bar charts in CSS
- For **comparison** content: use side-by-side dark cards in the takeaway section
- For **longer articles** (7+ sections): consider splitting into 2 images or using a more compact card style

## Requirements

- **Node.js** (any recent version)
- **Playwright** with Chromium (`npx playwright install chromium`)
- For Telegram lossless delivery: `curl` + bot token in `~/.openclaw/openclaw.json`
