# WebTUI CSS Framework & Theme Architecture Reference

> Technical reference guide for the CSS architecture, WebTUI engine, responsive design system, and custom overrides in the BashIt Hugo theme.

---

## 1. CSS Pipeline & Asset Bundling

The CSS asset pipeline in `layouts/_partials/head/css.html` bundles five stylesheets via Hugo Pipes:

```
assets/css/
├── webtui/full.css                   # Core WebTUI reset & component library
├── webtui/theme-catppuccin.css       # Palette tokens (or active webtuiTheme)
├── webtui/plugin-declarative-layout.css # Flexbox layout primitives (<row>, <column>)
├── webtui/plugin-nf.css              # Nerd Font @font-face rules & CDN fallbacks
└── main.css                          # Theme overrides, responsive rules, embeds
```

Processed via:
```go
{{- $bundle := $cssFiles | resources.Concat "css/bundle.css" }}
{{- $opts := dict "minify" (cond hugo.IsDevelopment false true) "sourceMap" (cond hugo.IsDevelopment "linked" "none") }}
{{- with $bundle | css.Build $opts }}
```

In production, the resulting bundle is fingerprinted and served with Subresource Integrity (`integrity="..." crossorigin="anonymous"`).

---

## 2. WebTUI Declarative Attribute Directives

WebTUI relies on HTML attribute selectors using CSS substring matchers (`=`, `~=`, `^=`, `$=`).

### Container Boxes (`box-`)

| Attribute Value | Style | Description |
|:---|:---|:---|
| `box-="square"` | `┌─┐ │ └─┘` | Monospace ASCII borders with square corners |
| `box-="round"` | Rounded | Border with `border-radius: 4px` |
| `box-="double"` | `╔═╗ ║ ╚═╝` | Double-line monospace border |

**Modifier (`shear-`)**:
- `shear-="both"`: Removes top and bottom 1lh padding to dock stacked boxes seamlessly.
- `shear-="top"` / `shear-="bottom"`: Removes top or bottom padding selectively.

---

### Badges (`is-="badge"`)

Syntax: `<span is-="badge" variant-="[color]" cap-="[cap]">Label</span>`

**Color Variants (`variant-`)**:
- Semantic: `green`, `blue`, `red`, `yellow`, `mauve`, `peach`, `teal`, `sky`, `sapphire`, `maroon`, `pink`, `flamingo`, `rosewater`, `lavender`
- Contrast: `foreground0`, `foreground1`, `foreground2`, `background0`, `background1`, `background2`, `background3`

**Cap Styles (`cap-`)**:
- `cap-="round"`: Rounded pill ends
- `cap-="triangle"`: Pointed terminal caps
- `cap-="slant-top"` / `cap-="slant-bottom"`: Angled terminal cuts
- `cap-="ribbon"`: Ribbon swallowtail ends

**Fluid / Wrapping Badges**:
WebTUI badges default to fixed character-cell heights with ASCII end-caps. When badges contain long text inside responsive cards, apply the `.wrap` modifier class or use `.badge-fluid`:
```html
<span is-="badge" variant-="blue" class="wrap">Long wrapping badge text</span>
```
This converts the badge to `display: inline-flex; white-space: normal;` and removes fixed pseudo-element end-caps.

---

### Buttons (`is-="button"`)

Syntax: `<a is-="button" variant-="foreground0" size-="small">Action</a>`

- **Sizes**: `size-="small"` (compact 1lh), `size-="default"`, `size-="large"`, `size-="full"` (100% width)
- **Variants**: `foreground0` (high-contrast inverted fill), `background1` (subtle border/fill), `background2`

---

### Declarative Flexbox Grid (`<row>`, `<column>`)

Implemented via `plugin-declarative-layout.css`. Zero custom CSS classes required.

| Directive | Attribute | Values / Behavior |
|:---|:---|:---|
| `<row>` | Element / `is-="row"` | `display: flex; flex-direction: row;` |
| `<column>` | Element / `is-="column"` | `display: flex; flex-direction: column;` |
| Gap | `gap-="1"` / `gap-="2"` | Spacing: `1` = 1lh / 1ch; `2` = 2lh / 2ch |
| Alignment | `align-^="..."` | `align-items`: `start`, `center`, `end`, `stretch` |
| Justification | `align-$="..."` | `justify-content`: `start`, `center`, `end`, `between` |
| Self Grow/Shrink | `self-~="..."` | `grow`, `nogrow`, `shrink`, `noshrink`, `nobasis`, `size-screen` |
| Padding | `pad-^="..."` / `pad-$="..."` | Horizontal (`^`) and vertical (`$`) padding levels: `0`, `1`, `2` |

**Responsive Flex Pattern for Landing Page Cards**:
```html
<row gap-="1" style="flex-wrap: wrap;">
  <div box-="square" style="flex: 1 1 340px;">Card A</div>
  <div box-="square" style="flex: 1 1 340px;">Card B</div>
</row>
```
On screens ≤768px, `main.css` collapses `flex: 1 1 340px` and `flex: 1 1 300px` to `flex: 1 1 100% !important`.

---

## 3. Upstream Fixes & Critical Workarounds

The theme's `main.css` incorporates crucial fixes for known WebTUI quirks:

### Bug 1: Word-Break Breaking Text Mid-Word
**Problem**: Upstream WebTUI's `base.css` sets `word-break: break-all` globally on `body`, which violently splits English words mid-sentence across lines.
**Fix in `main.css`**:
```css
html, body, p, blockquote, li, h1, h2, h3, h4, h5, h6, .content, article, .page-title {
  word-break: normal !important;
  overflow-wrap: break-word !important;
}
code, pre code, kbd, samp {
  word-break: keep-all !important;
  overflow-wrap: normal !important;
}
```

---

### Bug 2: WebTUI ASCII Table Grid Disconnection
**Problem**: Upstream `table.css` uses descendant selectors:
`table[box-] :is(table[divide-=both] th)`
This fails to match when both `box-` and `divide-` are placed on the **same** `<table>` element (e.g. `<table box-="square" divide-="both">`), leaving vertical and horizontal divider lines detached from the outer box border.
**Fix in `main.css`**: Compound attribute selectors with calculated overflow offsets:
```css
table[box-][divide-=vertical] th:not(:last-of-type):before,
table[box-][divide-=vertical] td:not(:last-of-type):before,
table[box-][divide-=both] th:not(:last-of-type):before,
table[box-][divide-=both] td:not(:last-of-type):before {
  top: calc(-.5lh - var(--table-border-width) / 2);
  height: calc(100% + 1lh);
}

table[box-][divide-=horizontal] tr:not(:last-of-type) th:after,
table[box-][divide-=horizontal] tr:not(:last-of-type) td:after,
table[box-][divide-=both] tr:not(:last-of-type) th:after,
table[box-][divide-=both] tr:not(:last-of-type) td:after {
  left: calc(-.5ch - var(--table-border-width) / 2);
  width: calc(100% + 1ch);
}
```

---

### Bug 3: Ghost Dividers on Standard Markdown Tables
**Problem**: WebTUI applies ASCII grid pseudo-elements to all tables by default, creating double lines and broken borders on unboxed, standard GFM markdown tables.
**Fix in `main.css`**:
```css
.content table:not([box-]):not([divide-]) {
  border-collapse: collapse !important;
  border: 1px solid var(--background2) !important;
}
.content table:not([box-]):not([divide-]) th::before,
.content table:not([box-]):not([divide-]) th::after,
.content table:not([box-]):not([divide-]) td::before,
.content table:not([box-]):not([divide-]) td::after {
  display: none !important;
}
```

---

### Bug 4: iframe Collapsing to 150px Fallback Height
**Problem**: Setting `iframe { height: auto; }` causes cross-origin iframes (YouTube, Instagram, Peerlist) to collapse to browser default 150px.
**Fix in `main.css`**:
- Ensure `iframe` is **never** given `height: auto;`.
- Use specific frame containers with calibrated fixed or aspect-ratio heights:
  - `.tui-video-container`: `aspect-ratio: 16 / 9`
  - `.tui-instagram-frame iframe`: `height: 620px !important`
  - `.tui-reddit-container iframe`: `min-height: 240px`

---

### Bug 5: Empty Wrapper Divs in Post Navigation
**Problem**: If `{{ with .PrevInSection }}` or `{{ with .NextInSection }}` only wraps the link inside a static `<div class="post-nav-item">`, an empty div is rendered when a post has no previous or next post, creating an awkward 240px dead-space gap on mobile.
**Fix in `page.html`**:
The entire `.post-nav-item` wrapper must be conditional:
```html
<nav class="post-navigation">
  {{ with .PrevInSection }}
    <div class="post-nav-item prev">...</div>
  {{ end }}
  {{ with .NextInSection }}
    <div class="post-nav-item next">...</div>
  {{ end }}
</nav>
```

---

## 4. Typography & Font Fallback Architecture

### 3-Tier Font System

CSS Custom Properties defined in `@layer base`:
```css
:root {
  --font-family-fallback: "Symbols Nerd Font", monospace;
  --font-family-global: "JetBrainsMono Nerd Font", "JetBrains Mono Nerd Font", "JetBrains Mono", var(--font-family-fallback);
  --font-family-paragraph: var(--font-family-global);
  --font-family-code: var(--font-family-global);
  --font-family: var(--font-family-global);
}
```

### Component Hierarchy Mapping
- **Headings & Chrome**: `h1, h2, h3, [is-~=button], [is-~=badge], table, nav, [box-]` → `--font-family-global`
- **Body & Prose**: `p, .content p, blockquote, .admonition-content p, .tui-bbs-body` → `--font-family-paragraph`
- **Code Listings**: `pre, code, kbd, samp, .highlight pre` → `--font-family-code`
- **Nerd Font Glyphs**: `.admonition-icon, .navbar-brand-icon` → `"Symbols Nerd Font", "JetBrainsMono Nerd Font", monospace !important;` (isolated from prose font changes to prevent missing icon glyphs).

---

## 5. Responsive Design Breakpoints

### Desktop (`> 768px`)
- Layout max-widths: `home.html` (1200px), `section.html`/`term.html` (1000px), `page.html` (800px).
- Post navigation renders two columns side by side (`flex-direction: row`).
- Social embeds use platform-calibrated snug max-widths (Twitter: 550px, Peerlist: 520px, Instagram: 540px, GitHub/Devto/Linkcard: 680px).

### Tablet & Mobile (`≤ 768px`)
- `.site-nav`: Switches to `flex-direction: column !important; align-items: stretch !important;`.
- `.page-article, article[box-]`: Padding compresses to `1.25rem 0.85rem !important;`.
- `.post-navigation`: Stacks vertically (`flex-direction: column !important;`). Items expand to 100% width. Titles truncate via `calc(100vw - 8rem)`.
- Cards with `flex: 1 1 340px` or `300px` collapse to `flex: 1 1 100% !important;`.
- Form controls in `.tui-bbs-form` collapse from side-by-side columns to vertical stack.

### Small Mobile (`≤ 480px`)
- Container padding drops to `0.25rem !important;`.
- Article padding compresses to `1rem 0.65rem !important;`.
- Nav menu gap tightens to `0.35rem 0.6ch !important;` with `font-size: 0.82rem !important;`.
- Fluid clamp typography ensures `h1` scales cleanly between `1.35rem` and `2.2rem` without word overflow.
