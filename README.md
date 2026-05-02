# Text2Shot

Generate professional, mobile-friendly infographic images from articles, tweets, or any structured content using HTML + Playwright high-DPI screenshot.

## Example output

<img src="assets/example-wechat.png" width="390" alt="Text2Shot example — WeChat style">

## What it does

When you ask Claude things like:

- "这篇文章生成一张信息图"
- "帮我做一张可以发微信群的总结图"
- "create an infographic from this article"
- "make a visual summary for social media"

Claude reads the skill, designs an HTML page with a structured layout (header → callout → QA block → verse/quote → takeaway → footer), then takes a 3x Retina screenshot to produce a crisp PNG optimized for mobile viewing.

## Features

- **6 style presets**: Clean Editorial, Dark Elegant, Warm Scripture, Bold Magazine, Minimal Ink, WeChat — each with a complete color token table and CSS overrides
- **Unified QA block**: Numbered sections share one container with inline badges and tinted headers — space-efficient and visually clean
- **Mobile-optimized**: 390px CSS width × 3x DPI = 1170px physical — sharp on any phone
- **Chinese-first**: Uses system PingFang SC / Noto Sans SC — zero external font dependencies
- **Self-contained HTML**: No CDN, no Google Fonts, works fully offline
- **Playwright screenshot**: Node.js script with Python fallback for high-DPI capture
- **Telegram lossless delivery**: Bundled script sends via `sendDocument` API to bypass Telegram's image compression

## Style Presets

| # | Name | Feel | Best For |
|---|------|------|----------|
| 1 | **Clean Editorial** | Cream bg, warm colored cards | General purpose (default) |
| 2 | **Dark Elegant** | Dark bg, gold accents | Evening sharing, serious content |
| 3 | **Warm Scripture** | Parchment, deep brown, square badges | Church, Bible study, devotional |
| 4 | **Bold Magazine** | Full-bleed navy hero, flat bands | Social media, attention-grabbing |
| 5 | **Minimal Ink** | White, generous whitespace, no cards | Short content, premium brand feel |
| 6 | **WeChat** | WeChat green, floating white cards | WeChat group sharing |

Full color tokens and CSS overrides for each preset are in `references/design-system.md`.

## Usage

### With Claude (recommended)

Share an article or text and say "生成信息图" or specify a style — Claude reads the skill and handles everything automatically.

### Standalone

```bash
# 1. Write your HTML infographic (see references/design-system.md for components and presets)

# 2. Take a high-DPI screenshot (Node.js)
node scripts/screenshot.js input.html output.png 3 390
# → output.png at 1170px wide, 3x DPI

# 3. Or use Python Playwright if Node script fails
python3 - <<'EOF'
from playwright.sync_api import sync_playwright
with sync_playwright() as p:
    browser = p.chromium.launch()
    ctx = browser.new_context(viewport={"width": 390, "height": 600}, device_scale_factor=3)
    page = ctx.new_page()
    page.goto("file:///path/to/input.html", wait_until="networkidle")
    page.locator("body").screenshot(path="output.png", type="png")
    browser.close()
EOF

# 4. (Optional) Send to Telegram without compression
bash scripts/send_telegram.sh output.png "Caption text"
```

### Screenshot script options

```
node scripts/screenshot.js <input.html> <output.png> [scaleFactor] [viewportWidth]

  scaleFactor     Device pixel ratio (default: 3)
  viewportWidth   CSS viewport width in px (default: 750)
```

## File structure

```
Text2Shot/
├── SKILL.md                  # Instructions for Claude
├── README.md                 # This file
├── references/
│   └── design-system.md      # Color palette, typography, QA block pattern, 6 style presets
└── scripts/
    ├── screenshot.js          # Playwright high-DPI screenshot tool (Node.js)
    └── send_telegram.sh       # Telegram lossless image delivery
```

## Requirements

- **Node.js** (v18+) — for the screenshot script
- **Playwright** with Chromium: `npx playwright install chromium`
- **Python 3** + `playwright` pip package — fallback screenshot method: `pip install playwright && playwright install chromium`
- For Telegram delivery: `curl` + Telegram bot token

## Design philosophy

1. **Scannable, not readable** — infographics are glanced at, not studied
2. **Visual hierarchy** — eyes flow naturally: title → callout → QA cards → takeaway
3. **Consistent color coding** — one accent color per section, don't mix randomly
4. **Mobile-first** — 390px viewport at 3x = 1170px physical, sharp on any phone
5. **Self-contained** — single HTML file, no external dependencies

## Credits

Based on the [infographic skill](https://github.com/jincai/openclaw-skills) from [openclaw-skills](https://github.com/jincai/openclaw-skills). This project extends and adapts it as a standalone tool.

## License

MIT
