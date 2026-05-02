# Infographic Design System

## Core Design Philosophy (from Impeccable)

**Bold intentionality over safe defaults.** Every design choice must be deliberate — commit to a clear aesthetic direction and execute with precision. Avoid "AI slop": generic gradients, cyan-on-dark, purple-to-blue, neon accents.

**The Squint Test:** Blur your eyes. Can you still identify the most important element, the second most important, and clear groupings? If everything looks the same weight, you have a hierarchy problem.

**Less is more:** Fewer sizes with more contrast beats many similar sizes. A 5-level hierarchy covers all needs.

## Color Palette

### Design Principles
- **Never use pure black (#000) or pure white (#fff)** — always tint; pure extremes never appear in nature
- **Tint neutrals toward your accent hue** — even subtle 1% chroma creates subconscious cohesion
- **Dominant color + sharp accent > evenly distributed palette** — commit, don't hedge
- **Reduce saturation at extremes** — light/dark variants need lower chroma to avoid garish results
- **No gray text on colored backgrounds** — use a shade of the background color instead

### Section Accent Colors
| Name   | Hex       | Use Case |
|--------|-----------|----------|
| Blue   | `#4A90D9` | Section 1, trust/tech |
| Orange | `#E8734A` | Section 2, energy/action |
| Green  | `#5BB978` | Section 3, growth/positive |
| Purple | `#9B6BB9` | Section 4, creativity |
| Gold   | `#E8A84A` | Section 5, highlight/warning |
| Red    | `#D94A6B` | Section 6, urgent/important |
| Teal   | `#3AAFA9` | Section 7, calm/balance |

### Background Colors
| Name        | Hex       | Use Case |
|-------------|-----------|----------|
| Cream       | `#faf6f0` | Page background (paper feel) |
| White       | `#fefefe` | Card background (tinted, not pure) |
| Light Blue  | `#EBF3FB` | Highlight box |
| Light Orange| `#FDF0EB` | Highlight box |
| Light Green | `#EDF7F0` | Highlight box |
| Light Purple| `#F3EDF7` | Highlight box |
| Yellow      | `#fffbe6` | Callout/quote box |
| Dark        | `#1a1a1a` | Takeaway section background |

### Text Colors
| Name  | Hex       | Use Case |
|-------|-----------|----------|
| Black | `#1a1a1a` | Headings, primary text |
| Dark  | `#333333` | Subheadings |
| Body  | `#444444` | Body text |
| Muted | `#888888` | Subtitles, English labels |
| Light | `#e8e4df` | Text on dark background (tinted cream, not pure white) |
| Gold  | `#f5c542` | Accent text on dark background |

## Typography

### Font Stack
```css
font-family: -apple-system, 'PingFang SC', 'Noto Sans SC', 'Helvetica Neue', sans-serif;
```

### Design Principles
- **Use fewer sizes with more contrast** — muddy hierarchy comes from too many similar sizes (14, 15, 16, 18...)
- **Vertical rhythm**: line-height is the base unit for ALL vertical spacing; spacing should be multiples of it
- **Weight creates hierarchy**: Bold (800-900) vs Regular (400) is more effective than slight size differences
- **Increase line-height for light-on-dark text** — perceived weight is lighter, needs more breathing room

### Scale (phone-first, 5-level hierarchy)

Screenshots are taken at **390px viewport × 3x** = 1170px wide PNG, viewed on phones at near-native resolution.

| Element | Size | Weight | Line Height |
|---------|------|--------|-------------|
| Main title | 30-34px | 900 | 1.2 |
| Section title | 18-20px | 800 | 1.3 |
| Body text | 17px | 400 | 1.6 |
| Small text (English, labels) | 13px | 400 | 1.4 |
| Minimum size | 13px | — | — |

**Ratio**: Title-to-body is ~2:1 — clear hierarchy readable without zooming on a phone screen.

## Spatial Design

### Principles
- **Use 4pt base grid**: 4, 8, 12, 16, 24, 32, 48px — more granular than 8pt
- **Use `gap` for sibling spacing** — eliminates margin collapse issues
- **Generous whitespace signals quality** — cramped layouts feel cheap
- **Group related items tightly, separate groups generously** — proximity = relationship

### Spacing Rules (390px phone viewport)
| Element | Value |
|---------|-------|
| Page padding (top/bottom) | 32-36px |
| Page padding (sides) | 18-22px |
| Card padding | 14-16px |
| Grid gap | 12-14px |
| Section margin-bottom | 18-24px |
| Between tightly related items | 6-8px |
| Between distinct groups | 20-28px |

### Key Rule
No fixed page height — content determines length. No empty filler space at bottom.

## Layout Patterns

### Page Structure
```
┌─────────────────────────┐
│       HEADER            │  Badge + Title + Subtitle
│    ─────────────        │  Divider line
├─────────────────────────┤
│  CALLOUT BOX            │  Yellow bg, gold left border
├────────────┬────────────┤
│  CARD 1    │  CARD 2    │  auto-fit grid (stacks to 1-col on phone)
├────────────┴────────────┤
│  FULL-WIDTH CARD        │  Spans both columns
├─────────────────────────┤
│  CARD 3    │  CARD 4    │  Continue grid
├─────────────────────────┤
│  ██ TAKEAWAY BOX ██     │  Dark background
├─────────────────────────┤
│       FOOTER            │  Source + Brand
└─────────────────────────┘
```

### Hierarchy Through Multiple Dimensions (not size alone)
| Tool | Strong | Weak |
|------|--------|------|
| Size | 3:1 ratio+ | <2:1 |
| Weight | Bold vs Regular | Medium vs Regular |
| Color | High contrast | Similar tones |
| Position | Top/left (primary) | Bottom/right |
| Space | Surrounded by whitespace | Crowded |

**Combine at least 3 dimensions** for any important element.

### Numbered Card Pattern (Unified QA Block)

Group all numbered sections into **one shared container** — never use the side-by-side circle + narrow card layout, which wastes horizontal space and looks disconnected.

```
┌─────────────────────────────┐
│ ① Section title             │  ← tinted header, colored left border, inline badge
│ · Bullet one                │
│ · Bullet two                │
├ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┤
│ ② Next section title        │
│ · ...                       │
└─────────────────────────────┘
```

```html
<div class="qa-block">

  <div class="qa-header" style="border-left-color:#4A90D9; background:#EBF3FB;">
    <span class="qa-num" style="background:#4A90D9;">1</span>
    <span class="qa-title">Section title</span>
  </div>
  <div class="qa-body">
    <div class="bullet">Bullet point one</div>
    <div class="bullet">Bullet point two</div>
  </div>

  <div class="qa-divider"></div>
  <!-- repeat for each section -->

</div>
```

```css
.qa-block {
  background: #fefefe;
  border-radius: 14px;
  border: 1px solid #ece7e1;
  overflow: hidden;
  box-shadow: 3px 3px 0 rgba(0,0,0,0.05);
  margin-bottom: 18px;
}
.qa-header {
  display: flex; align-items: center; gap: 10px;
  padding: 11px 14px; border-left: 4px solid;
}
.qa-num {
  width: 22px; height: 22px; border-radius: 50%;
  color: #fefefe; font-size: 12px; font-weight: 900;
  display: flex; align-items: center; justify-content: center; flex-shrink: 0;
}
.qa-title { font-size: 17px; font-weight: 800; color: #1a1a1a; line-height: 1.3; }
.qa-body { padding: 9px 16px 13px 18px; }
.qa-divider { height: 1px; background: #ece7e1; margin: 0 14px; }
.bullet {
  font-size: 15px; color: #444; line-height: 1.65;
  padding-left: 14px; position: relative; margin-bottom: 5px;
}
.bullet:last-child { margin-bottom: 0; }
.bullet::before {
  content: "·"; position: absolute; left: 2px;
  font-weight: 900; font-size: 18px; line-height: 1.2; color: #bbb;
}
```

- Use section accent colors for `border-left-color`, `background` (light tint), and `.qa-num` background
- No English subtitles — they waste a line and add no value
- For sub-categories within a bullet (e.g. 公共/职场/家庭), use `<strong style="color:[accent]">label</strong>:` inline

### Callout/Quote Box
```css
background: #fffbe6;
border-left: 4px solid #f5c542;
border-radius: 0 10px 10px 0;
padding: 14px 18px;
```

### Takeaway Section (dark)
```css
background: #1a1a1a;
color: #e8e4df;          /* tinted cream, not pure white */
border-radius: 14px;
padding: 22px 24px;
line-height: 1.7;        /* extra breathing room on dark bg */
```

## Component Library

### Badge
```html
<div style="display:inline-block; background:#222; color:#e8e4df;
  font-size:11px; padding:4px 14px; border-radius:12px; letter-spacing:2px;
  font-weight:600;">
  BADGE TEXT
</div>
```

### 2-Column Grid
At 390px viewport (350px usable after padding), `minmax(140px, 1fr)` fits two columns naturally.

```html
<div style="display:grid; grid-template-columns:repeat(auto-fit, minmax(140px, 1fr)); gap:12px;">
  <div style="border-radius:10px; padding:14px; background:#EBF3FB;">
    <div style="font-size:19px; font-weight:800; color:#4A90D9;">Label</div>
    <div style="font-size:17px; color:#444; margin-top:6px;">Description</div>
  </div>
</div>
```

### Dark Bottom Cards
```html
<div style="flex:1; background:#222; color:#e8e4df; border-radius:8px;
  padding:12px; text-align:center;">
  <div style="font-size:15px; font-weight:800;">Title</div>
  <div style="font-size:12px; color:#aaa; margin-top:4px;">Description</div>
</div>
```

## Anti-Patterns (Never Do)

- ❌ Emoji as section icons (use colored shapes, SVG, or text symbols instead)
- ❌ Pure black/white anywhere
- ❌ Gradient text for "impact" — decorative, not meaningful
- ❌ Cyan-on-dark, purple-to-blue gradients, neon accents (AI slop aesthetics)
- ❌ Dark mode with glowing accents as default
- ❌ Gray text on colored backgrounds
- ❌ Many similar font sizes (14, 15, 16, 17...) — pick 5 and commit
- ❌ Evenly distributed colors — commit to a dominant + accent
- ❌ Heavy drop shadows — keep shadows subtle or use none
- ❌ Marketing-style flashy layouts (floating decorations, scattered icons, over-the-top gradients)

## Style Presets

Each preset defines a complete visual system: background, card style, QA block, verse box, takeaway, and footer. Apply the preset's CSS variables/overrides on top of the base component library.

---

### 1. Clean Editorial (default)

Cream background, warm hierarchy, colored QA block, professional but approachable. Best all-purpose choice.

| Token | Value |
|-------|-------|
| Page bg | `#faf6f0` |
| Card bg | `#fefefe` |
| Card border | `#ece7e1` |
| Badge bg | `#222` / text `#e8e4df` |
| Callout bg | `#fffbe6` / border `#f5c542` |
| Verse box bg | `#1c2b3a` |
| Takeaway bg | `#1a1a1a` |
| QA section 1 | border `#4A90D9` / tint `#EBF3FB` / num `#4A90D9` |
| QA section 2 | border `#E8734A` / tint `#FDF0EB` / num `#E8734A` |
| QA section 3 | border `#5BB978` / tint `#EDF7F0` / num `#5BB978` |

---

### 2. Dark Elegant

All-dark warm background, gold accents, premium/contemplative feel. Good for evening sharing or serious content.

| Token | Value |
|-------|-------|
| Page bg | `#13110f` |
| Body text | `#e0d8cc` |
| Card bg | `#1e1b18` |
| Card border | `#2a2520` |
| Badge | underline style, color `#d4a250` |
| Callout bg | `#1e1b18` / border `#d4a250` |
| Verse box bg | `#0d0b09` |
| Takeaway bg | `#1a1208` |
| Highlight / accent | `#d4a250` (gold) |
| QA section 1 | border `#d4a250` / tint `#1a1714` / num `#d4a250` |
| QA section 2 | border `#c4866a` / tint `#1a1512` / num `#c4866a` |
| QA section 3 | border `#7aab85` / tint `#141a15` / num `#7aab85` |

```css
/* Dark Elegant — key overrides */
body { background: #13110f; color: #e0d8cc; }
.badge { background: transparent; color: #d4a250; border-bottom: 1px solid #d4a250; border-radius: 0; }
.main-title { color: #e8e0d5; }
.subtitle { color: #7a7168; }
.divider { background: linear-gradient(to right, transparent, #3a3228, transparent); }
.callout { background: #1e1b18; border-left-color: #d4a250; color: #ccc4b8; }
.callout-label { color: #d4a250; }
.qa-block { background: #1e1b18; border-color: #2a2520; }
.qa-title { color: #e8e0d5; }
.bullet { color: #b8b0a4; }
.bullet::before { color: #4a4540; }
.verse-box { background: #0d0b09; border: 1px solid #2a2520; }
.verse-text { color: #ccc4b8; }
.verse-ref { color: #5a5248; }
.takeaway { background: #1a1208; border: 1px solid #2a2520; }
.highlight { color: #d4a250; }
.closing { border-top-color: #2e2e2e; color: #ccc4b8; }
.footer { color: #4a4540; }
```

---

### 3. Warm Scripture

Parchment background, deep brown typography, square-corner badges, devotional/scroll feel. Best for church and Bible study content.

| Token | Value |
|-------|-------|
| Page bg | `#f5ede0` |
| Body text | `#3d2b1f` |
| Card bg | `#fffaf3` |
| Card border | `#d4b896` |
| Badge bg | `#7a3520` (brick red), border-radius `3px` |
| Callout bg | `#fdf0da` / border `#c4864a` |
| Verse box bg | `#2c1810` |
| Takeaway bg | `#1e1208` |
| Highlight | `#d4a250` |
| QA section 1 | border `#4A90D9` / tint `#eef4fb` / num `#4A90D9` |
| QA section 2 | border `#c4864a` / tint `#fdf5ea` / num `#c4864a` |
| QA section 3 | border `#5a8a5a` / tint `#eef5ee` / num `#5a8a5a` |
| Bullet marker | `–` (en-dash), color `#c4a882` |
| QA num border-radius | `3px` (square corners, not circles) |

```css
/* Warm Scripture — key overrides */
body { background: #f5ede0; color: #3d2b1f; }
.badge { background: #7a3520; color: #f5ede0; border-radius: 3px; }
.main-title { color: #2c1a0e; }
.subtitle { color: #9a7c6a; }
.divider { background: linear-gradient(to right, transparent, #c4a882, transparent); }
.callout { background: #fdf0da; border-left-color: #c4864a; }
.callout-label { color: #7a3520; }
.qa-block { background: #fffaf3; border-color: #d4b896; }
.qa-num { border-radius: 3px; }
.bullet::before { content: "–"; color: #c4a882; font-weight: 400; }
.verse-box { background: #2c1810; }
.takeaway { background: #1e1208; }
.takeaway-item::before { border-radius: 1px; background: #c4864a; }
```

---

### 4. Bold Magazine

Full-bleed dark hero header, flat rectangular section bands (no top border-radius), strong typographic contrast. Good for attention-grabbing social sharing.

| Token | Value |
|-------|-------|
| Page bg | `#f4f2ee` |
| Hero bg | `#1a3050` (deep navy) |
| Hero title | `#fefefe`, 38px |
| Section band 1 | `#2a5a8a` |
| Section band 2 | `#8a3a20` |
| Section band 3 | `#2a5a3a` |
| QA body bg | `#fefcf8` with colored left border |
| Verse bg | `#1a3050` (same as hero) |
| Takeaway bg | `#111820` |
| Highlight | `#7ab0e0` |
| Bullet marker | `›` chevron, color `#aaa` |
| QA num | large `28px` decorative, `rgba(255,255,255,0.4)` |

Key structural difference: hero is a **full-bleed block** (no horizontal margin), verse and takeaway are also full-bleed. Only the QA body has horizontal padding.

```css
/* Bold Magazine — key overrides */
body { background: #f4f2ee; padding: 0 0 40px; }
.hero { background: #1a3050; padding: 36px 22px 28px; margin-bottom: 24px; }
.main-title { font-size: 38px; color: #fefefe; }
.badge { background: rgba(255,255,255,0.15); color: #c8d8e8; border-radius: 3px; }
/* QA: flat bands, no rounded top */
.qa-header { padding: 12px 16px; }
.qa-num { font-size: 28px; color: rgba(255,255,255,0.4); width: 28px; }
.qa-body { background: #fefcf8; padding: 10px 16px 14px 56px; border-left: 3px solid; }
.bullet::before { content: "›"; color: #aaa; }
.verse-box { border-radius: 0; margin: 0 0 20px; }
.takeaway { border-radius: 0; }
.highlight { color: #7ab0e0; }
```

---

### 5. Minimal Ink

White background, generous whitespace (44px sides), no cards or shadows, single navy accent, typography-driven hierarchy. Best for short/impactful content or high-quality brand feel.

| Token | Value |
|-------|-------|
| Page bg | `#fefefe` |
| Page padding | `44px 28px` |
| Body text | `#2a2a2a` |
| Single accent | `#1c2b4a` (deep navy) |
| Callout | left border only `3px solid #1c2b4a`, italic text |
| QA sections | no card, horizontal rule dividers, `01/02/03` labels |
| Verse box | top + bottom `1px solid #e8e8e8`, no background |
| Takeaway | no card background, plain text list |
| Bullet marker | `·` (middot), color `#ccc` |
| QA num | `11px` uppercase label `01`, color `#1c2b4a` |

```css
/* Minimal Ink — key overrides */
body { background: #fefefe; padding: 44px 28px; }
.badge { background: none; color: #aaa; letter-spacing: 3px; border-radius: 0; padding: 0; }
.main-title { font-size: 36px; }
.callout { border-left: 3px solid #1c2b4a; background: none; font-style: italic; color: #1c2b4a; }
/* No card styling — remove background, border, shadow */
.qa-block { background: none; border: none; box-shadow: none; }
.qa-num { font-size: 11px; color: #1c2b4a; background: none; letter-spacing: 1px; width: 20px; height: auto; border-radius: 0; }
.qa-title { font-size: 18px; }
.qa-divider { background: #f0f0f0; margin: 0; }
.verse-box { background: none; border-radius: 0; border-top: 1px solid #e8e8e8; border-bottom: 1px solid #e8e8e8; }
.verse-text { font-size: 18px; font-weight: 300; color: #1a1a1a; }
.takeaway { background: none; padding: 0; }
.highlight { color: #1c2b4a; font-weight: 600; }
.takeaway-item::before { width: 5px; height: 1px; border-radius: 0; background: #1c2b4a; top: 10px; }
```

---

### 6. WeChat

WeChat green (`#07C160`) hero header, `#ededed` gray page background, white floating cards, iOS-native feel. Optimized for WeChat group sharing.

| Token | Value |
|-------|-------|
| Page bg | `#ededed` |
| Hero bg | `#07C160` |
| Card bg | `#fff`, border-radius `10px`, no explicit border |
| Card margin | `0 12px 10px` (cards float on gray bg) |
| Primary accent | `#07C160` (WeChat green) |
| Secondary accent | `#576B95` (WeChat link blue-gray) |
| Tertiary accent | `#FA9D3B` (orange) |
| Verse box bg | `#07C160` |
| Takeaway bg | `#191919` |
| Highlight | `#4cd964` (lighter green on dark) |
| Footer | white card with icon + two-line text |

Key structural difference: **no horizontal padding on body** — cards use `margin: 0 12px` to float on the gray background, like native WeChat UI.

```css
/* WeChat — key overrides */
body { background: #ededed; padding: 0 0 32px; }
/* Full-bleed hero, no margin */
.hero { background: #07C160; padding: 40px 20px 28px; margin-bottom: 12px; }
.badge { background: rgba(255,255,255,0.15); color: rgba(255,255,255,0.75); border-radius: 3px; }
.main-title { color: #fff; font-size: 30px; }
.subtitle { color: rgba(255,255,255,0.75); }
/* Floating cards */
.card, .qa-card { background: #fff; border-radius: 10px; margin: 0 12px 10px; border: none; box-shadow: none; }
.verse-box { background: #07C160; border-radius: 10px; margin: 0 12px 10px; }
.verse-text { color: #fff; }
.verse-label { color: rgba(255,255,255,0.75); }
.verse-ref { color: rgba(255,255,255,0.65); }
.takeaway { background: #191919; border-radius: 10px; margin: 0 12px 10px; }
.highlight { color: #4cd964; }
/* Footer card with icon */
.footer-card { background: #fff; border-radius: 10px; margin: 0 12px; padding: 12px 16px; display: flex; align-items: center; gap: 10px; }
```
