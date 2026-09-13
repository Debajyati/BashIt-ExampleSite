---
title: "BashIt Theme Documentation: Core Components, Setup, and Features"
date: 2026-09-12T18:00:00+05:30
tags: ["documentation", "webtui", "bashit", "theme", "guide"]
categories: ["Documentation"]
draft: false
summary: "The definitive guide to the BashIt Hugo theme: architecture, installation, configuration, typography, ASCII boxes, and complete WebTUI shortcode component reference."
---

Welcome to the official documentation and component reference for **BashIt** — a high-performance, retro-modern terminal theme for Hugo powered by the [WebTUI CSS framework](https://webtui.ironclad.sh/).

BashIt brings the tactile, distraction-free aesthetic of classic terminal UIs (TUIs) into the browser without sacrificing modern web performance, semantic HTML5, or responsive design.

---

## 1. Architecture & Design Principles

{{< webtui-box style="square" >}}
**Core Design Tenets:**
- **Zero Heavy Frameworks**: No Bootstrap, Tailwind runtime, or massive JS bundles. Everything is styled via WebTUI's CSS layers (`@layer base, utils, components`).
- **Nerd Fonts Integration**: Native terminal iconography powered by `@webtui/plugin-nf` and *Symbols Nerd Font*.
- **Catppuccin Mocha Palette**: High-contrast, easy-on-the-eyes dark terminal theme with warm pastel accents.
- **Blazing Speed**: Compiled via Hugo Pipes with minification and fingerprinting (<60ms build times).
{{< /webtui-box >}}

---

## 2. Installation & Quickstart

### Step 1: Add BashIt as a Git Submodule
Inside your Hugo site root directory:
```bash
git submodule add https://github.com/Debajyati/BashIt.git themes/BashIt
```

And later you can update the submodule in your site directory to the latest commit using:
```bash
git submodule update --remote --merge
```

### Step 2: Install Local NPM Dependencies
BashIt bundles WebTUI modules locally for offline independence and deterministic asset bundling:
```bash
cd themes/BashIt
npm install
```

### Step 3: Configure `hugo.toml`
Add the following to your site's `hugo.toml`:

```toml
baseURL = 'https://yoursite.org/'
title = 'My Terminal Blog'
theme = 'BashIt'

[params]
  webtuiTheme = 'catppuccin'

# Navbar Brand Customization (Icon, Emoji, or Image Logo)
[params.navbar]
  icon = "󰞷"            # Nerd Font glyph ("󰞷"), Unicode emoji ("⚡"), or HTML entity ("&#xe62b;")
  # logo = "/favicon.svg" # Optional image/SVG logo path
  # disableIcon = true   # Set to true to hide the icon/logo entirely

[markup]
  [markup.goldmark]
    [markup.goldmark.renderer]
      unsafe = true  # Enables custom WebTUI HTML elements in Markdown
```

---

## 3. Navbar Brand & Logo Customization

BashIt allows you to customize the brand icon, emoji, or image logo displayed before the site title in the top navigation bar. By default, it renders the classic Vim terminal logo (`&#xe62b;`), but you can easily customize it to match your project or personal aesthetic.

### Configuration Options (`hugo.toml`)

Under `[params.navbar]` in your `hugo.toml`, you can choose any of the following formats:

{{< webtui-table headers="Style | Setting | Example Syntax | Description" rows="Unicode Emoji | `icon` | `icon = '⚡'` | Any standard emoji (⚡, 💻, 🚀, 🐧) ;; Nerd Font Glyph | `icon` | `icon = '󰞷'` | Symbols Nerd Font terminal glyph or icon ;; HTML / Unicode Entity | `icon` | `icon = '&#xe62b;'` | HTML entity code point (Default: Vim logo) ;; Image / SVG Logo | `logo` (or `icon`) | `logo = '/favicon.svg'` | Path to SVG or PNG logo (auto-detected by extension) ;; Text Only (Disabled) | `disableIcon` | `disableIcon = true` | Hides the icon completely and displays only the title" divide="both" >}}

#### Examples in `hugo.toml`:

```toml
[params.navbar]
  # 1. Unicode Emoji
  icon = "⚡"

  # 2. Nerd Font Icon Glyph
  # icon = "󰞷"

  # 3. Custom Image / SVG Logo
  # logo = "/favicon.svg"

  # 4. Hide the icon completely (text-only brand)
  # disableIcon = true
```

{{< webtui-callout title="Clickable Navigation" variant="green" >}}
Both the custom logo/icon and the site title are unified inside a single link navigating to your homepage (`/`), preserving responsive layout and accessibility.
{{< /webtui-callout >}}

---

## 4. Typography & 3-Tier Font Customization

BashIt loads and uses **JetBrains Mono Nerd Font** by default across the entire website. This eliminates missing glyph boxes and broken terminal icons on operating systems or browsers where the default monospace font lacks Nerd Font symbols.

You can customize or override three distinct font levels independently in your `hugo.toml`:

{{< webtui-table headers="Font Slot | Setting | Default | Scope" rows="Global Font | `global` | JetBrainsMono Nerd Font | Base site font: UI elements, headings, navbar, buttons, and tables ;; Paragraph Font | `paragraph` | Inherits Global | Prose text, article content, blockquotes, and paragraphs ;; Code Blocks Font | `code` | Inherits Global | Preformatted code listings, `pre`, `code`, `kbd`, and terminal blocks" divide="both" >}}

### Example Configuration (`hugo.toml`):

```toml
[params.font]
  # 1. Global Font (UI, headers, navigation, buttons, default base)
  global = "JetBrainsMono Nerd Font"

  # 2. Paragraph Font (Body text, articles, prose paragraphs)
  paragraph = "JetBrainsMono Nerd Font"

  # 3. Code Blocks Font (pre, code, highlights, terminal blocks)
  code = "JetBrainsMono Nerd Font"

  # Optional webfont stylesheet link (e.g. if loading external Google Fonts)
  # fontUrl = "https://fonts.googleapis.com/css2?family=Fira+Code:wght@400;700&display=swap"
```

{{< webtui-callout title="Automated Fallback Protection" variant="green" >}}
Even if you set a custom font that lacks developer icons (like <code>Inter</code> for paragraphs or <code>Cascadia</code> for code), BashIt automatically appends <code>"Symbols Nerd Font", monospace</code> to the fallback stack and isolates icon elements (such as admonition glyphs) to ensure 100% icon rendering reliability.
{{< /webtui-callout >}}

---

## 5. Core Component Reference & Live Demos

All core components are available both as Hugo shortcodes (`{{</* webtui-component */>}}`) and native HTML attributes.

### A. ASCII Boxes (Terminal Borders)

Wrap any content inside classic terminal ASCII character borders. Available in `square`, `round`, and `double` styles:

{{< webtui-box style="square" >}}
**Square ASCII Box (`style="square"` or `box-="square"`)**
Single-line crisp ASCII border — the quintessential TUI container.
{{< /webtui-box >}}

{{< webtui-box style="round" >}}
**Round ASCII Box (`style="round"` or `box-="round"`)**
Rounded corners with terminal box-drawing glyphs.
{{< /webtui-box >}}

{{< webtui-box style="double" >}}
**Double ASCII Box (`style="double"` or `box-="double"`)**
Double-line border for prominent headers and callouts.
{{< /webtui-box >}}

```markdown
{{</* webtui-box style="square" */>}}
Your terminal content here.
{{</* /webtui-box */>}}
```

---

### B. Buttons & Badges

Interactive buttons and metadata status tags with Catppuccin color accents:

#### Buttons:
<row gap-="1" style="flex-wrap: wrap; margin-bottom: 1rem;">
  {{< webtui-button variant="foreground0" size="small" >}}Primary Button{{< /webtui-button >}}
  {{< webtui-button variant="background1" size="small" >}}Secondary Button{{< /webtui-button >}}
  {{< webtui-button variant="background2" size="small" >}}Outline Button{{< /webtui-button >}}
</row>

#### Badges:
<row gap-="1" style="flex-wrap: wrap; margin-bottom: 1rem;">
  {{< webtui-badge variant="green" >}}󰄲 Online{{< /webtui-badge >}}
  {{< webtui-badge variant="blue" >}}󰋼 Info{{< /webtui-badge >}}
  {{< webtui-badge variant="yellow" >}}󱈸 Warning{{< /webtui-badge >}}
  {{< webtui-badge variant="red" >}}󰚌 Urgent{{< /webtui-badge >}}
  {{< webtui-badge variant="mauve" >}}󰉹 Beta{{< /webtui-badge >}}
  {{< webtui-badge variant="peach" >}}󰝗 Notice{{< /webtui-badge >}}
</row>

```markdown
{{</* webtui-button variant="foreground0" size="small" */>}}Click Me{{</* /webtui-button */>}}
{{</* webtui-badge variant="green" */>}}Active{{</* /webtui-badge */>}}
```

---

### C. Data Tables

Render structured tabular data with monospace character alignment and customizable divider lines. Rows are separated by `;;` (or newlines), columns by `|`, and headers by `|` (or `,`):

{{< webtui-table headers="Service | Runtime | Port | Status" rows="API Gateway | Go 1.22 | :8080 | Active ;; Cache Worker | Rust 1.80 | :6379 | Active ;; Static Site | Hugo+WebTUI | :1313 | Online" divide="both" >}}

```markdown
<!-- Standard WebTUI Table (using ;; for rows and | for columns) -->
{{</* webtui-table headers="Service | Runtime | Port | Status" rows="API Gateway | Go 1.22 | :8080 | Active ;; Cache Worker | Rust 1.80 | :6379 | Active ;; Static Site | Hugo+WebTUI | :1313 | Online" divide="both" */>}}

<!-- Multiline rows are also supported -->
{{</* webtui-table headers="Service | Runtime | Port | Status" rows="
API Gateway | Go 1.22 | :8080 | Active
Static Site | Hugo+WebTUI | :1313 | Online
" divide="both" */>}}
```

---

### D. Accordions & Directory File Trees

Interactive collapsible panels with prominent terminal disclosure indicators and specialized directory variants (`variant="directory"`):

#### Standard Accordion:
{{< webtui-accordion title="Click to inspect configuration parameters" >}}
- `baseURL`: Canonical URL of the site.
- `languageCode`: Standard locale (e.g. `en-us`).
- `params.webtuiTheme`: Color theme (default: `catppuccin`).
{{< /webtui-accordion >}}

#### Directory & File Tree:
{{< webtui-file-accordion title="BashIt-Project/" open="true" >}}
{{< webtui-file-accordion title="layouts/" open="true" >}}
{{< webtui-file name="baseof.html" >}}
{{< webtui-file name="home.html" >}}
{{< webtui-file name="page.html" >}}
{{< /webtui-file-accordion >}}
{{< webtui-file-accordion title="assets/css/" >}}
{{< webtui-file name="main.css" >}}
{{< /webtui-file-accordion >}}
{{< webtui-file name="hugo.toml" >}}
{{< webtui-file name="package.json" >}}
{{< /webtui-file-accordion >}}

```markdown
{{</* webtui-file-accordion title="project/" open="true" */>}}
  {{</* webtui-file-accordion title="src/" */>}}
    {{</* webtui-file name="main.rs" */>}}
  {{</* /webtui-file-accordion */>}}
  {{</* webtui-file name="Cargo.toml" */>}}
{{</* /webtui-file-accordion */>}}
```

---

### E. Progress Bars & Terminal Spinners

Visualize background jobs, completion metrics, and loading states:

- **Build Progress Meter:**
{{< webtui-progress value="85" max="100" label="85%" >}}

- **Animated Terminal Spinners:**
<row gap-="2" style="align-items: center; margin: 1rem 0;">
  <span>{{< webtui-spinner variant="cursor" >}} cursor</span>
  <span>{{< webtui-spinner variant="dots" >}} dots</span>
  <span>{{< webtui-spinner variant="arrows" >}} arrows</span>
  <span>{{< webtui-spinner variant="bar-vertical" >}} bars</span>
</row>

```markdown
{{</* webtui-progress value="85" max="100" label="85%" */>}}
{{</* webtui-spinner variant="cursor" */>}} Loading...
```

---

### F. Text Highlights & Callouts

Emphasize code symbols, keywords, or important notifications:

{{< webtui-callout title="Pro Tip: Terminal Performance" variant="green" >}}
All WebTUI styles use standard CSS variables (`--foreground0`, `--background1`, `--blue`, `--green`). They adapt instantaneously to theme overrides.
{{< /webtui-callout >}}

- Highlights: {{< webtui-mark variant="blue" >}}Blue mark{{< /webtui-mark >}}, {{< webtui-mark variant="peach" >}}Peach mark{{< /webtui-mark >}}, and {{< webtui-mark variant="green" >}}Green mark{{< /webtui-mark >}}.

```markdown
{{</* webtui-callout title="Notice" variant="blue" */>}}
Important callout content.
{{</* /webtui-callout */>}}
```

---

### G. Tooltips, Popovers & Switches

Interactive micro-controls without requiring JavaScript libraries:

- **Tooltip on Hover:** {{< webtui-tooltip tip="WebTUI CSS running with zero client JavaScript!" position="top" >}}<span is-="badge" variant-="blue">󰈙 Hover over this terminal badge</span>{{< /webtui-tooltip >}}

- **Popover Dropdown with Terminal Toggles:**
{{< webtui-popover summary="[ 󰘵 Terminal Preferences ]" position="bottom left" >}}
<column gap="1" style="padding: 0.5rem;">
  <span><strong>Display Settings</strong></span>
  {{< webtui-switch name="crt" checked="true" >}}CRT Phosphor Glow{{< /webtui-switch >}}
  {{< webtui-checkbox name="sound" checked="true" >}}Teletype Audio Audio{{< /webtui-checkbox >}}
</column>
{{< /webtui-popover >}}

---

## 6. Looking for Extended Shortcodes?

For advanced diagrams, charts, geo maps, math equations, and image alignment options, see our companion post:
👉 **[Extended Shortcodes Showcase: Admonition, Mermaid, ECharts, Mapbox, and Image Alignments](/posts/extended-shortcodes/)**
