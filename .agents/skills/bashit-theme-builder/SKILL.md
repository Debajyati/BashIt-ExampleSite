---
name: bashit-theme-builder
description: >-
  Use this skill when the user asks to build, configure, or customize a Hugo
  static site using the BashIt terminal/TUI theme. Covers installation,
  hugo.toml configuration, all 79 shortcodes (social embeds, UI components,
  diagrams, comments), WebTUI CSS framework directives, responsive design
  patterns, Catppuccin/Gruvbox/Nord/Everforest palettes, Nerd Font typography,
  Cloudflare Workers comment system, and Cloudflare Pages deployment. Also
  use when troubleshooting WebTUI CSS bugs, broken shortcodes, or layout
  issues in BashIt-powered sites.
---

# BashIt Theme Builder — AI Agent Skill

> Build visually stunning, performant Hugo sites with retro-terminal TUI
> aesthetics using the **BashIt** theme and the **WebTUI CSS** framework.

**Theme repo**: `github.com/Debajyati/BashIt`
**Example site repo**: `github.com/Debajyati/BashIt-ExampleSite`
**Comments worker repo**: `github.com/Debajyati/bashit-comments`

---

## Table of Contents

1. [Prerequisites & Installation](#1-prerequisites--installation)
2. [Hugo Configuration (`hugo.toml`)](#2-hugo-configuration-hugotoml)
3. [Theme Architecture](#3-theme-architecture)
4. [WebTUI CSS Framework Directives](#4-webtui-css-framework-directives)
5. [Shortcode Reference](#5-shortcode-reference)
6. [CSS Architecture & Customization](#6-css-architecture--customization)
7. [Responsive Design Patterns](#7-responsive-design-patterns)
8. [Color Palettes & Typography](#8-color-palettes--typography)
9. [Comments System](#9-comments-system)
10. [Content Authoring Patterns](#10-content-authoring-patterns)
11. [Deployment](#11-deployment)
12. [Common Pitfalls & Troubleshooting](#12-common-pitfalls--troubleshooting)
13. [Validation Checklist](#13-validation-checklist)

For the exhaustive shortcode parameter reference, see
[references/shortcodes.md](./references/shortcodes.md).

---

## 1. Prerequisites & Installation

### Requirements

- **Hugo** `v0.160.1+extended` (extended edition required for Hugo Pipes CSS
  bundling)
- **Node.js** (for theme npm dependencies — Nerd Font webfonts)
- **Git** (theme is added as a git submodule)

### Installation Steps

```bash
# 1. Create a new Hugo site
hugo new site my-site && cd my-site

# 2. Initialize git
git init

# 3. Add BashIt theme as a git submodule
git submodule add https://github.com/Debajyati/BashIt.git themes/BashIt

# 4. Install theme npm dependencies (Nerd Font webfonts)
cd themes/BashIt && npm install && cd ../..

# 5. Set the theme in hugo.toml
echo 'theme = "BashIt"' >> hugo.toml

# 6. Start development server
hugo server -D
```

> **IMPORTANT**: Always add BashIt as a **git submodule**, not by copying files.
> This ensures proper updates and asset pipeline integration. After cloning a
> site that already uses the submodule, run:
> ```bash
> git submodule update --init --recursive
> ```

### Archetype

The theme ships with a default archetype at `archetypes/default.md`:

```markdown
+++
title = '{{ replace .File.ContentBaseName "-" " " | title }}'
date = {{ .Date }}
draft = true
+++
```

---

## 2. Hugo Configuration (`hugo.toml`)

Below is the **complete, annotated** `hugo.toml` reference. Copy and customize:

```toml
baseURL = '/'
languageCode = 'en-us'
title = "My TUI Site"
theme = 'BashIt'

# ─── WebTUI Color Palette ────────────────────────────────────────────
[params]
  # Options: 'catppuccin' (default), 'gruvbox', 'nord', 'everforest',
  #          'osmium', 'vitesse'
  webtuiTheme = 'catppuccin'

# ─── Navbar Brand ────────────────────────────────────────────────────
[params.navbar]
  icon = "󰞷"            # Nerd Font glyph (recommended)
  # icon = "⚡"          # Unicode emoji alternative
  # logo = "/favicon.svg" # Image/SVG logo path (overrides icon)
  # disableIcon = true    # Disable brand icon entirely

# ─── Typography (3-Tier Font System) ─────────────────────────────────
[params.font]
  # global    = "JetBrainsMono Nerd Font"   # UI, headers, nav, buttons
  # paragraph = "JetBrainsMono Nerd Font"   # Article prose, blockquotes
  # code      = "JetBrainsMono Nerd Font"   # pre, code, kbd blocks
  # fontUrl   = "https://fonts.googleapis.com/css2?family=..."

# ─── OpenGraph Scraper (for linkcard & dailydev shortcodes) ──────────
[params.opengraph]
  scraperApi = "https://xogapi.ddebajyati.workers.dev"

# ─── Comments ────────────────────────────────────────────────────────
# provider: "cloudflare" | "giscus" | "disabled"
[params.comment]
  enable = true
  provider = "cloudflare"

  [params.comment.cloudflare]
    api = "https://bashit-comments.your-name.workers.dev"
    mockMode = false        # true = offline localStorage mode

  [params.comment.giscus]
    repo = "username/repo"
    repoId = ""
    category = "Announcements"
    categoryId = ""
    mapping = "pathname"
    theme = "catppuccin_mocha"
    lazy = false

# ─── Goldmark Renderer ──────────────────────────────────────────────
# CRITICAL: unsafe = true is REQUIRED for BashIt
# Without it, raw HTML (WebTUI attributes, <row>, <column>, badges,
# accordions) inside markdown will be stripped by Hugo
[markup]
  [markup.goldmark]
    [markup.goldmark.renderer]
      unsafe = true

# ─── Navigation Menu ────────────────────────────────────────────────
[menus]
  [[menus.main]]
    name = 'Home'
    pageRef = '/'
    weight = 10
  [[menus.main]]
    name = 'Articles'
    pageRef = '/posts'
    weight = 20
  [[menus.main]]
    name = 'Tags'
    pageRef = '/tags'
    weight = 30
```

### Critical Configuration Notes

| Setting | Why It Matters |
|:--------|:---------------|
| `unsafe = true` | **Required.** BashIt uses raw HTML custom elements (`<row>`, `<column>`) and WebTUI attributes (`box-`, `is-`, `variant-`) in markdown. Without this, Hugo strips them. |
| `theme = 'BashIt'` | Case-sensitive. Must match the submodule directory name exactly. |
| `webtuiTheme` | Controls the color palette CSS loaded. Invalid values fall back to default dark theme. |

---

## 3. Theme Architecture

### Directory Structure

```text
themes/BashIt/
├── archetypes/
│   └── default.md                    # Post scaffold
├── assets/
│   ├── css/
│   │   ├── main.css                  # Theme overrides (~1370 lines)
│   │   └── webtui/
│   │       ├── full.css              # WebTUI component bundle
│   │       ├── base.css              # Root CSS variables & reset
│   │       ├── plugin-declarative-layout.css  # <row>/<column> flex grid
│   │       ├── plugin-nf.css         # Nerd Font @font-face rules
│   │       ├── theme-catppuccin.css  # Catppuccin Mocha palette
│   │       ├── theme-gruvbox.css     # Gruvbox Dark palette
│   │       ├── theme-nord.css        # Nord palette
│   │       ├── theme-everforest.css  # Everforest palette
│   │       ├── theme-osmium.css      # Osmium palette
│   │       ├── theme-vitesse.css     # Vitesse palette
│   │       ├── components/           # Individual WebTUI components
│   │       │   ├── accordion.css
│   │       │   ├── badge.css
│   │       │   ├── button.css
│   │       │   ├── checkbox.css
│   │       │   ├── table.css
│   │       │   ├── tooltip.css
│   │       │   ├── spinner.css
│   │       │   ├── ... (20 component files)
│   │       │   └── view.css
│   │       └── utils/
│   │           └── box.css           # ASCII box borders
│   └── js/
│       └── main.js                   # Theme JavaScript (~387 lines)
├── layouts/
│   ├── baseof.html                   # Base HTML skeleton (16 lines)
│   ├── home.html                     # Homepage (1200px max-width)
│   ├── page.html                     # Single post (800px max-width)
│   ├── section.html                  # Section list (1000px max-width)
│   ├── taxonomy.html                 # Taxonomy list (1000px max-width)
│   ├── term.html                     # Term list (1000px max-width)
│   ├── shortcodes/                   # 79 shortcode templates
│   │   ├── youtube.html, yt.html, webtui-youtube.html
│   │   ├── x.html, tweet.html, twitter.html, webtui-x.html
│   │   ├── ... (see Shortcode Reference)
│   │   └── webtui-separator.html
│   └── _partials/
│       ├── head.html                 # <head> with preconnects, fonts
│       ├── head/
│       │   └── css.html              # Hugo Pipes CSS bundle pipeline
│       ├── header.html               # Navbar with brand, menu
│       ├── footer.html               # Site footer
│       ├── comment.html              # Comment provider dispatcher
│       ├── menu.html                 # Nav menu items
│       └── terms.html                # Tag/category term links
└── package.json                      # npm dependencies
```

### Layout Max-Width Summary

| Layout | Max Width | Purpose |
|:-------|:----------|:--------|
| `home.html` | 1200px | Landing pages, dashboards |
| `page.html` | 800px | Single posts/articles (optimal readability) |
| `section.html` | 1000px | Post listing pages |
| `taxonomy.html` | 1000px | Taxonomy overview |
| `term.html` | 1000px | Term listing |

### CSS Pipeline

Hugo Pipes bundles these CSS files in order:

1. `webtui/full.css` — Core WebTUI component engine
2. `webtui/theme-{palette}.css` — Color palette variables
3. `webtui/plugin-declarative-layout.css` — Flex grid (`<row>`, `<column>`)
4. `webtui/plugin-nf.css` — Nerd Font `@font-face` rules
5. `main.css` — Theme overrides, responsive rules, component styles

The bundle is minified, fingerprinted, and served with SRI integrity in
production.

### Key Layout Template: `baseof.html`

```html
<!DOCTYPE html>
<html lang="{{ .Site.LanguageCode }}" data-webtui-theme="{{ ... }}">
<head>{{ partial "head.html" . }}</head>
<body>
  {{ partial "header.html" . }}
  {{ block "main" . }}{{ end }}
  {{ partial "footer.html" . }}
  <script src="{{ $js.RelPermalink }}" defer></script>
</body>
</html>
```

The `data-webtui-theme` attribute on `<html>` selects the palette. The value
maps to CSS selectors like `[data-webtui-theme=catppuccin]`.

---

## 4. WebTUI CSS Framework Directives

BashIt uses the **WebTUI CSS framework** — a declarative, zero-JS terminal UI
system built on custom HTML attributes. Master these directives:

### Box Borders

```html
<div box-="square">Square ASCII border ┌─┐│ │└─┘</div>
<div box-="round">Rounded border with border-radius</div>
<div box-="double">Double-line border ╔═╗║ ║╚═╝</div>
```

Modifier: `shear-="both|top|bottom"` strips vertical padding to dock boxes.

### Badges

```html
<span is-="badge" variant-="green">ONLINE</span>
<span is-="badge" variant-="red">ERROR</span>
<span is-="badge" variant-="blue">v2.1</span>
```

Available colors: `rosewater`, `flamingo`, `pink`, `mauve`, `red`, `maroon`,
`peach`, `yellow`, `green`, `teal`, `sky`, `sapphire`, `blue`, `lavender`,
`foreground0`–`foreground2`, `background0`–`background3`.

Cap styles: `cap-="round|triangle|slant-top|slant-bottom|ribbon"`.

### Buttons

```html
<a is-="button" variant-="foreground0" size-="small">Click Me</a>
```

Sizes: `small`, `default`, `large`, `full`.

### Separators

```html
<hr is-="separator" direction-="horizontal" cap-="bisect">
```

Caps: `bisect` (center diamond), `edge` (end caps).
Directions: `horizontal`, `vertical`.

### Declarative Flex Layout

```html
<row gap-="1">             <!-- Horizontal flex, 1ch/1lh gap -->
  <column self-="grow">    <!-- Vertical flex, grows to fill -->
    Content A
  </column>
  <column>Content B</column>
</row>
```

| Attribute | Values | Purpose |
|:----------|:-------|:--------|
| `gap-` | `1`, `2` | Gap spacing (1lh/1ch or 2lh/2ch) |
| `self-` | `grow`, `nogrow`, `shrink`, `noshrink`, `nobasis` | Flex item sizing |
| `align-` | Start: `start\|end\|center\|stretch` (align-items). End: `between\|start\|end\|center` (justify-content) | Alignment |
| `pad-` | `0`, `1`, `2` | Padding (horizontal via `^=`, vertical via `$=`) |

### Tables

```html
<table box-="square" divide-="both">
  <tr><th>Name</th><th>Status</th></tr>
  <tr><td>Server A</td><td>Online</td></tr>
</table>
```

Divide: `vertical`, `horizontal`, `both`.

### Accordions

```html
<details is-="accordion">
  <summary>Click to expand</summary>
  Hidden content here.
</details>
```

Variant: `variant-="directory"` renders folder icons (``, ``).

### Other Components

| Component | Attribute | Example |
|:----------|:----------|:--------|
| Spinner | `is-="spinner"` | `<span is-="spinner" variant-="cursor" speed-="fast"></span>` |
| Progress | `is-="progress"` | `<progress is-="progress" value="75" max="100"></progress>` |
| Switch | `is-="switch"` | `<input is-="switch" type="checkbox" checked>` |
| Checkbox | `is-="checkbox"` | `<input is-="checkbox" type="checkbox">` |
| Tooltip | `is-="tooltip"` | `<span is-="tooltip"><span is-="tooltip-trigger">Hover</span><span is-="tooltip-content">Tip text</span></span>` |
| Popover | `is-="popover"` | `<details is-="popover" position-="bottom left"><summary>Menu</summary>Items</details>` |
| Mark | `is-="mark"` | `<span is-="mark" bg-="blue" fg-="background0">Highlighted</span>` |

---

## 5. Shortcode Reference

BashIt ships with **79 shortcode templates**. Many shortcodes have
**aliases** — identical template copies for developer convenience.

> For the exhaustive parameter-by-parameter reference, see
> [references/shortcodes.md](./references/shortcodes.md).

### Social Embeds

| Shortcode | Aliases | Platform | Auto-Fetch? |
|:----------|:--------|:---------|:------------|
| `youtube` | `yt`, `webtui-youtube` | YouTube (privacy-enhanced) | Extracts video ID from URL |
| `x` | `tweet`, `twitter`, `webtui-x` | X/Twitter | Loads official widget JS |
| `instagram` | `insta`, `webtui-instagram` | Instagram | Official embed iframe |
| `peerlist` | `webtui-peerlist` | Peerlist | Auto-resize via postMessage |
| `reddit` | `webtui-reddit` | Reddit | Official embed blockquote |
| `github` | `webtui-github` | GitHub | **Yes** — REST API at build time |
| `devto` | `webtui-devto` | Dev.to | **Yes** — REST API at build time |
| `dailydev` | `webtui-dailydev` | Daily.dev | Via OpenGraph scraper |
| `linkcard` | `webtui-linkcard` | Any URL | Via OpenGraph scraper |

#### YouTube

```markdown
{{</* youtube "dQw4w9WgXcQ" */>}}
{{</* yt "https://youtu.be/dQw4w9WgXcQ" start="42" */>}}
```

Uses `youtube-nocookie.com` for privacy. Accepts full URLs, short URLs, or
bare video IDs. `start` parameter for timestamp (seconds).

#### X / Twitter

```markdown
{{</* x "https://x.com/user/status/1234567890" */>}}
{{</* tweet "20" user="jack" */>}}
<!-- Manual static card (no JS widget): -->
{{</* x user="jack" name="jack" date="Mar 21, 2006" text="just setting up my twttr" */>}}
```

#### GitHub Repository Card

```markdown
{{</* github "gohugoio/hugo" */>}}
{{</* github "https://github.com/Debajyati/BashIt" */>}}
<!-- Manual fallback: -->
{{</* github repo="owner/repo" language="Go" stars="85000" forks="7200" license="Apache-2.0" description="..." */>}}
```

Fetches stars, forks, language, description from `api.github.com` at build
time. Uses Hugo's `site.Store` for build-time caching to avoid rate limits.

#### Link Card (OpenGraph)

```markdown
{{</* linkcard "https://gohugo.io" */>}}
{{</* linkcard "https://example.com" compact=true */>}}
{{</* linkcard "https://example.com" noimage=true */>}}
```

Requires `params.opengraph.scraperApi` to be configured. Fetches title,
description, and image via OpenGraph metadata scraping.

#### Instagram

```markdown
{{</* instagram "BWNjjyYFxVx" */>}}
{{</* insta "https://www.instagram.com/p/BWNjjyYFxVx/" */>}}
<!-- Static fallback: -->
{{</* instagram user="natgeo" image="/images/natgeo-post.jpg" caption="..." */>}}
```

#### Peerlist

```markdown
{{</* peerlist "ACTHLKLLGMGQJGPBKI977NQ89DLP8M" */>}}
{{</* peerlist id="..." */>}}
```

#### Reddit

```markdown
{{</* reddit "https://www.reddit.com/r/golang/comments/..." */>}}
```

#### Dev.to

```markdown
{{</* devto "https://dev.to/user/article-slug" */>}}
<!-- Manual: -->
{{</* devto title="..." author="..." handle="..." date="..." readtime="..." tags="go,webdev" summary="..." url="..." */>}}
```

#### Daily.dev

```markdown
{{</* dailydev "https://dly.to/g9vaRB1agcs" */>}}
```

### Content Components

#### Admonitions (14 Types)

```markdown
{{</* admonition type="note" title="Heads Up" open=true */>}}
This is an informational note with a terminal-styled border.
{{</* /admonition */>}}
```

Types: `note`, `abstract`, `info`, `tip`, `success`, `question`, `warning`,
`failure`, `danger`, `bug`, `example`, `quote`, `important`, `caution`.

Each type has a unique Nerd Font icon, Catppuccin accent color border, and
collapsible `<details>` behavior (`open=true` or `open=false`).

#### Image

```markdown
{{</* image src="/images/demo.png" alt="Demo" caption="Figure 1" align="center" width="600px" box="square" */>}}
```

Params: `src`, `alt`, `caption`, `align` (`left|center|right`), `width`,
`height`, `box` (`square|round`), `href`/`link` (clickable wrap).

#### Tabs

```markdown
{{</* tabs defaultTab=0 */>}}
  {{</* tab title="Go" */>}}
  ```go
  fmt.Println("Hello")
  ```
  {{</* /tab */>}}
  {{</* tab title="Rust" */>}}
  ```rust
  println!("Hello");
  ```
  {{</* /tab */>}}
{{</* /tabs */>}}
```

#### Audio Player

```markdown
{{</* audio src="/audio/sample.mp3" title="Track Name" artist="Artist" */>}}
```

#### File Tree (Directory Accordion)

```markdown
{{</* file-accordion title="src/" open="true" */>}}
  {{</* file name="main.go" icon="󰟓" */>}}
  {{</* file name="utils.go" icon="󰟓" */>}}
{{</* /file-accordion */>}}
```

### Data Display & UI Widgets

#### Table (Monospace ASCII)

```markdown
{{</* webtui-table
  headers="Command | Description | Status"
  rows="ls | List files | ✓ ;; cd | Change dir | ✓ ;; rm | Remove | ⚠"
  box="square"
  divide="both"
*/>}}
```

- `headers`: Column headers separated by `|`
- `rows`: Rows separated by `;;`, columns by `|`
- `box`: `square`, `round`, or `double`
- `divide`: `vertical`, `horizontal`, or `both`

#### Badge

```markdown
{{</* webtui-badge variant="green" */>}}ACTIVE{{</* /webtui-badge */>}}
```

#### Box

```markdown
{{</* webtui-box style="square" */>}}
Content inside a terminal box.
{{</* /webtui-box */>}}
```

Param `shear`: `both|top|bottom` for docking boxes vertically.

#### Button

```markdown
{{</* webtui-button variant="foreground0" size="small" */>}}Click Me{{</* /webtui-button */>}}
```

#### Other Widgets

| Shortcode | Key Params | Renders |
|:----------|:-----------|:--------|
| `webtui-progress` | `value`, `max`, `label` | Terminal progress bar |
| `webtui-spinner` | `variant` (`cursor\|dots\|arrows\|bar-vertical`) | CSS animation |
| `webtui-mark` | `variant` or `bg`/`fg` | Highlighted inline text |
| `webtui-tooltip` | `tip`, `position` (`top\|bottom\|left\|right`) | Hover tooltip |
| `webtui-popover` | `summary`, `position` | Dropdown menu |
| `webtui-switch` | `name`, `checked`, `bar` | Toggle switch |
| `webtui-checkbox` | `name`, `checked` | Form checkbox |
| `webtui-radio` | `name`, `value`, `checked` | Radio button |
| `webtui-separator` | `direction`, `cap` | Terminal divider |
| `webtui-callout` | `title`, `variant` | Simpler alert box |
| `webtui-accordion` | `title`, `open`, `variant` | Collapsible panel |

### Interactive & Visualization

#### Mermaid Diagrams

```markdown
{{</* mermaid title="System Architecture" */>}}
graph TD
    A[Client] --> B[CDN]
    B --> C[Hugo Static Site]
    C --> D[Cloudflare Workers]
{{</* /mermaid */>}}
```

Renders responsive diagrams with dark Catppuccin theme inside terminal frames.

#### ECharts Data Visualization

```markdown
{{</* echarts width="100%" height="320px" title="Monthly Stats" */>}}
{
  "xAxis": { "type": "category", "data": ["Jan", "Feb", "Mar"] },
  "yAxis": { "type": "value" },
  "series": [{ "data": [120, 200, 150], "type": "bar" }]
}
{{</* /echarts */>}}
```

#### Mapbox (Dark Geo Maps)

```markdown
{{</* mapbox lat="37.7749" lng="-122.4194" zoom="11" title="San Francisco" height="280px" */>}}
```

#### TypeIt (Typewriter Animation)

```markdown
{{</* typeit speed="45" cursor="█" prompt="guest@bashit:~$ " title="Terminal Demo" */>}}
echo "Hello, World!"
{{</* /typeit */>}}
```

#### Math (KaTeX)

```markdown
{{</* math title="Fourier Transform" */>}}
\hat{f}(\xi) = \int_{-\infty}^{\infty} f(x) e^{-2\pi i x \xi} dx
{{</* /math */>}}
```

### Comment Systems

#### BBS (Cloudflare Workers + D1)

```markdown
{{</* bbs */>}}
```

Auto-configured from `hugo.toml` `[params.comment]`. No parameters needed in
most cases. Manual override: `api="..."` and `mock="true"`.

#### Giscus (GitHub Discussions)

```markdown
{{</* giscus */>}}
```

Auto-configured from `hugo.toml` `[params.comment.giscus]`.

#### Disqus

```markdown
{{</* disqus shortname="your-shortname" */>}}
```

### Utility Shortcodes

| Shortcode | Purpose | Example |
|:----------|:--------|:--------|
| `gist` | GitHub Gist embed | `{{</* gist "user" "gist_id" */>}}` |
| `showcase` | Project card | `{{</* showcase title="..." summary="..." link="..." image="..." */>}}` |
| `friend` / `person` | Developer card | `{{</* friend name="..." title="..." url="..." avatar="..." bio="..." */>}}` |
| `script` | Inject JS | `{{</* script src="/js/custom.js" */>}}` |
| `style` | Inline CSS wrapper | `{{</* style "color: var(--green);" */>}}Green text{{</* /style */>}}` |

---

## 6. CSS Architecture & Customization

### main.css Section Map (~1370 lines)

| Lines | Section | Purpose |
|:------|:--------|:--------|
| 1–78 | Global resets | `box-sizing`, `overflow-x: hidden`, `word-break: normal !important` (overrides WebTUI), responsive images/iframes |
| 79–133 | Fluid typography | `clamp()` scaling for h1–h6 across viewports |
| 134–151 | `@layer base` | CSS custom property font stack declarations |
| 152–860 | `@layer components` | All component styles: accordions, admonitions, images, diagrams, tabs, typewriter, cards, comments, tooltips, nav, social embeds |
| 860–980 | Social embed geometry | Fixed max-widths per platform (Twitter: 550px, Instagram: 540px, etc.) |
| 980–1100 | Navbar, header, footer | Site chrome component styles |
| 1100–1158 | Tables | ASCII table fixes, markdown table styles, ghost divider suppression |
| 1158–1371 | Media queries | Responsive rules at 768px and 480px breakpoints |

### CSS Custom Properties

```css
:root {
  --font-family-fallback: "Symbols Nerd Font", monospace;
  --font-family-global: "JetBrainsMono Nerd Font", "JetBrains Mono Nerd Font",
                        "JetBrains Mono", var(--font-family-fallback);
  --font-family-paragraph: var(--font-family-global);
  --font-family-code: var(--font-family-global);
}
```

### CSS Cascade Layers

BashIt uses CSS `@layer` for predictable specificity:

- `@layer base` — Font variable declarations
- `@layer components` — All component styles

**Best practice**: Add custom site styles under `@layer components` to maintain
cascade order without `!important`:

```css
@layer components {
  .my-custom-class {
    color: var(--green);
  }
}
```

### Adding Custom CSS

Create a file at `assets/css/custom.css` in your **site** directory (not the
theme). Hugo's asset pipeline will pick it up if referenced in a layout
override.

> **IMPORTANT**: Never modify files inside `themes/BashIt/`. Always use site-
> level overrides. Theme files should only be changed in the theme repository
> itself.

---

## 7. Responsive Design Patterns

### Breakpoints

| Breakpoint | Target | Key Changes |
|:-----------|:-------|:------------|
| `> 768px` | Desktop | Full side-by-side layouts, fixed-width embeds |
| `≤ 768px` | Tablet | Navbar stacks vertically, post nav becomes column, flex cards go `1 1 100%` |
| `≤ 480px` | Phone | Tighter padding, smaller fonts, compressed gaps |

### Fluid Typography (No-Breakpoint)

```css
h1 { font-size: clamp(1.35rem, 4.5vw, 2.2rem); }
h2 { font-size: clamp(1.15rem, 3.5vw, 1.65rem); }
h3 { font-size: clamp(1.05rem, 2.8vw, 1.35rem); }
```

### Card Grid Pattern

For responsive card grids in content, use flexbox with `flex` shorthand:

```html
<row gap-="1" style="flex-wrap: wrap;">
  <div box-="square" style="flex: 1 1 340px;">Card 1</div>
  <div box-="square" style="flex: 1 1 340px;">Card 2</div>
  <div box-="square" style="flex: 1 1 340px;">Card 3</div>
</row>
```

At `≤768px`, the CSS automatically collapses `flex: 1 1 340px` items to
`flex: 1 1 100% !important`, stacking cards vertically.

### Social Embed Max-Widths

Each social embed platform has a calibrated max-width to prevent layout
overflow:

| Platform | Max Width |
|:---------|:----------|
| Twitter/X | 550px |
| Peerlist | 520px |
| Instagram | 540px |
| Reddit | 640px |
| GitHub, Dev.to, Daily.dev, LinkCard | 680px |

All embeds center with `margin: 1.5rem auto`.

---

## 8. Color Palettes & Typography

### Available Palettes

Set via `params.webtuiTheme` in `hugo.toml`:

| Palette | Base | Surface | Text | Signature Accents |
|:--------|:-----|:--------|:-----|:-----------------|
| `catppuccin` | `#1e1e2e` | `#313244` | `#cdd6f4` | Mauve `#cba6f7`, Blue `#89b4fa`, Green `#a6e3a1`, Peach `#fab387` |
| `gruvbox` | `#282828` | `#3c3836` | `#ebdbb2` | Aqua `#8ec07c`, Orange `#fe8019`, Yellow `#fabd2f` |
| `nord` | `#2e3440` | `#3b4252` | `#eceff4` | Frost `#88c0d0`, Aurora `#a3be8c` |
| `everforest` | `#2d353b` | `#343f44` | `#d3c6aa` | Green `#a7c080`, Amber `#dbbc7f`, Blue `#7fbbb3` |
| `osmium` | Dark | — | — | Custom palette |
| `vitesse` | Dark | — | — | Multiple sub-themes |

### Catppuccin 14-Color Accent Palette

These CSS variables are available for all components:

`--rosewater`, `--flamingo`, `--pink`, `--mauve`, `--red`, `--maroon`,
`--peach`, `--yellow`, `--green`, `--teal`, `--sky`, `--sapphire`, `--blue`,
`--lavender`

Use them via `variant-="green"` on badges/buttons or `var(--green)` in CSS.

### 3-Tier Font System

| Tier | Purpose | CSS Variable | Default |
|:-----|:--------|:-------------|:--------|
| Global | UI, headers, navigation, buttons | `--font-family-global` | JetBrainsMono Nerd Font |
| Paragraph | Article prose, blockquotes, comments | `--font-family-paragraph` | Inherits global |
| Code | `pre`, `code`, `kbd`, terminal blocks | `--font-family-code` | Inherits global |

When customizing fonts via `hugo.toml`, the theme auto-appends
`"JetBrainsMono Nerd Font", "Symbols Nerd Font", monospace` to ensure Nerd
Font glyphs never break.

### Font Loading Pipeline

```
1. Preconnect → cdn.jsdelivr.net, fonts.googleapis.com
2. Preload   → JetBrainsMonoNerdFont-Regular.woff2
3. Google Fonts <link> → JetBrains Mono (400, 700, italic)
4. CSS plugin-nf.css → @font-face local-first + CDN fallback
5. Dynamic <style> → hugo.toml font overrides injected
```

---

## 9. Comments System

### Option A: Terminal BBS (Cloudflare Workers + D1)

Privacy-first, zero-ad comments with sub-50ms edge response times.

#### Architecture

```
Browser → Hugo Static Page → Cloudflare Worker → D1 (SQLite)
```

- **Spam defense**: Honeypot field (`website_hp`) — no CAPTCHAs
- **Offline mode**: `mockMode = true` uses browser `localStorage`
- **Free tier**: D1 is serverless SQLite within Cloudflare free limits

#### Deployment

```bash
# 1. Clone the comments worker
git clone https://github.com/Debajyati/bashit-comments.git
cd bashit-comments

# 2. Create D1 database
npx wrangler d1 create bashit-comments

# 3. Update wrangler.toml with the database ID

# 4. Apply schema
npx wrangler d1 execute bashit-comments --file=./schema.sql

# 5. Deploy
npx wrangler deploy
```

#### Configure in hugo.toml

```toml
[params.comment]
  enable = true
  provider = "cloudflare"
  [params.comment.cloudflare]
    api = "https://bashit-comments.your-name.workers.dev"
    mockMode = false
```

### Option B: Giscus (GitHub Discussions)

```toml
[params.comment]
  enable = true
  provider = "giscus"
  [params.comment.giscus]
    repo = "username/repo"
    repoId = "R_..."
    category = "Announcements"
    categoryId = "DIC_..."
    mapping = "pathname"
    theme = "catppuccin_mocha"
```

### Option C: Disable Comments

```toml
[params.comment]
  enable = false
```

---

## 10. Content Authoring Patterns

### Homepage Dashboard Pattern

The landing page (`content/_index.md`) should use WebTUI components for a
terminal-dashboard aesthetic:

```markdown
+++
title = 'My TUI Site'
date = 2024-01-01
draft = false
+++

<div box-="square" style="background: var(--background0); padding: 1.5rem;">
  <row align-="between">
    <span is-="badge" variant-="green">󰘵 SYSTEM ACTIVE</span>
    <span style="color: var(--foreground2);">tty1 • Catppuccin Mocha</span>
  </row>
  <h1>Welcome to My Site</h1>
  <p>Your site description here.</p>
  <row gap-="1">
    <a is-="button" variant-="foreground0" size-="small" href="/posts/">
      [ 󰈙 Read Articles ]
    </a>
  </row>
</div>

{{< webtui-separator direction="horizontal" cap="bisect" >}}

<!-- Feature cards using flex grid -->
<row gap-="1" style="flex-wrap: wrap;">
  <div box-="square" style="flex: 1 1 340px;">
    <column style="gap: 0.75rem;">
      <span is-="badge" variant-="blue">󰈙 DOCS</span>
      <p>Comprehensive documentation and guides.</p>
      <a is-="button" size-="small" href="/docs/">Read →</a>
    </column>
  </div>
  <div box-="square" style="flex: 1 1 340px;">
    <column style="gap: 0.75rem;">
      <span is-="badge" variant-="green">󰭹 BLOG</span>
      <p>Latest articles and tutorials.</p>
      <a is-="button" size-="small" href="/posts/">Browse →</a>
    </column>
  </div>
</row>
```

### Post Frontmatter Pattern

```yaml
---
title: "Your Post Title"
date: 2024-01-15T10:00:00+05:30
tags: ["hugo", "webdev", "terminal"]
categories: ["Tutorial"]
draft: false
summary: "A brief description for listings and SEO."
---
```

### Using Shortcodes in Posts

Combine shortcodes with standard markdown:

```markdown
## Getting Started

Here's a quick overview table:

{{</* webtui-table
  headers="Step | Command | Description"
  rows="1 | hugo new site | Create new site ;; 2 | hugo server -D | Start dev server"
  box="square"
  divide="both"
*/>}}

{{</* admonition type="tip" title="Pro Tip" open=true */>}}
Always use `hugo server -D` during development to see draft posts.
{{</* /admonition */>}}

Check out this related video:

{{</* youtube "dQw4w9WgXcQ" */>}}
```

### Syntax Disclosure Pattern

For documentation sites, show shortcode syntax inside expandable accordions:

```html
<details is-="accordion">
  <summary>󰅩 View Shortcode Syntax</summary>

  ```markdown
  {{</* youtube "VIDEO_ID" */>}}
  ```

</details>
```

---

## 11. Deployment

### Cloudflare Pages (Recommended)

1. Push your site to GitHub/GitLab
2. In Cloudflare Pages dashboard → Create a project → Connect repo
3. Build settings:
   - **Build command**: `hugo --minify`
   - **Build output directory**: `public`
   - **Environment variable**: `HUGO_VERSION` = `0.160.1`
4. Add custom domain if desired

### Other Platforms

Hugo's output is a static `public/` folder. Deploy anywhere:

- **Netlify**: `hugo --minify`, publish `public/`
- **Vercel**: Same build command
- **GitHub Pages**: Use `peaceiris/actions-hugo` GitHub Action
- **Self-hosted**: `rsync` the `public/` directory

---

## 12. Common Pitfalls & Troubleshooting

### ❌ Raw HTML stripped from markdown

**Cause**: `unsafe = true` not set in `hugo.toml`.

```toml
[markup.goldmark.renderer]
  unsafe = true
```

### ❌ Nerd Font icons show empty boxes

**Cause**: Missing npm install or CDN blocked.

```bash
cd themes/BashIt && npm install
```

The theme uses local-first `@font-face` with CDN fallback. Ensure
`cdn.jsdelivr.net` is accessible.

### ❌ Social embed iframes collapse to 150px height

**Cause**: CSS `height: auto` on iframes forces cross-origin fallback. The
theme's `main.css` already handles this — but if you override iframe styles,
don't set `height: auto` on iframes.

### ❌ Markdown tables have double dividers

**Cause**: WebTUI applies pseudo-element grid lines to all tables. The theme
suppresses these on standard markdown tables via:
```css
.content table:not([box-]):not([divide-]) td::before,
.content table:not([box-]):not([divide-]) td::after { display: none; }
```

### ❌ WebTUI ASCII table grid lines don't connect

**Cause**: Upstream WebTUI uses descendant selectors
`table[box-] :is(table[divide-=both] th)` which fail when both attributes
are on the SAME element. The theme fixes this with compound selectors:
```css
table[box-][divide-=both] th { /* compound selector fix */ }
```

### ❌ `word-break: break-all` breaks paragraphs mid-word

**Cause**: WebTUI's `base.css` sets `word-break: break-all` globally. The
theme overrides this:
```css
html, body, p, blockquote, li, h1, h2, h3, h4, h5, h6, .content, article {
  word-break: normal !important;
  overflow-wrap: break-word !important;
}
```

### ❌ Post navigation has large gaps on mobile

**Cause**: Empty `<div>` wrappers for prev/next when one direction is missing.
The fix wraps the entire nav item inside `{{ with .PrevInSection }}` /
`{{ with .NextInSection }}` instead of just the link.

### ❌ Theme changes don't take effect

**Cause**: Editing files inside `themes/BashIt/` submodule but not committing
changes in the submodule, or modifying the wrong directory.

- Theme files → modify in the BashIt repo, then update submodule
- Site content → modify in the site repo (never in themes/)

---

## 13. Validation Checklist

After building or modifying a BashIt site, verify:

```bash
# 1. Build with no errors
hugo --minify

# 2. Check build output size
du -sh public/

# 3. Start dev server and check console for errors
hugo server -D --navigateToChanged
```

### Visual Checks

- [ ] Homepage renders with terminal boxes and badges
- [ ] Navbar brand icon/logo displays correctly
- [ ] Color palette matches `webtuiTheme` setting
- [ ] Posts render with proper typography (no broken glyphs)
- [ ] Shortcodes render without empty frames or console errors
- [ ] Social embeds load and are properly centered
- [ ] Tables display ASCII grid lines (if using `box-`/`divide-`)
- [ ] Admonitions show colored borders and Nerd Font icons
- [ ] Mobile layout stacks properly at 768px and 480px
- [ ] Comments load (if enabled) or show mock mode
- [ ] Post navigation prev/next links display correctly

### Build Performance Target

BashIt is designed for sub-60ms builds with Hugo Pipes. If builds are slow:

- Check for large unoptimized images (use Hugo's image processing)
- Verify `api.github.com` calls aren't hitting rate limits (check cache)
- Ensure npm dependencies are installed (missing fonts trigger CDN fetches)
