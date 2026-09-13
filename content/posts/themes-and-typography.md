---
title: "Palettes, Nerd Fonts, and CSS Customization: Tuning the BashIt Aesthetic"
date: 2026-09-12T19:30:00+05:30
tags: ["bashit", "themes", "catppuccin", "typography", "nerdfonts", "css"]
categories: ["Documentation", "Design"]
draft: false
summary: "A deep dive into BashIt's visual engine: switching color palettes, utilizing Nerd Font glyphs, and extending WebTUI CSS layers."
---

**BashIt** is engineered with a strict terminal-first design philosophy. Rather than relying on heavyweight CSS utility frameworks or complex Sass build steps, styling is organized around modern CSS features: `@layer`, CSS custom properties, and semantic HTML5 attributes.

---

## 1. Built-in Color Palettes

BashIt bundles multiple authentic developer color themes through the WebTUI ecosystem:

{{< tabs defaultTab=0 >}}
{{< tab title="Catppuccin Mocha" >}}
- **Base Background:** `#1e1e2e`
- **Surface Accent:** `#313244`
- **Primary Text:** `#cdd6f4`
- **Accent Colors:** Mauve (`#cba6f7`), Blue (`#89b4fa`), Green (`#a6e3a1`), Peach (`#fab387`)
{{< /tab >}}
{{< tab title="Gruvbox Dark" >}}
- **Base Background:** `#282828`
- **Surface Accent:** `#3c3836`
- **Primary Text:** `#ebdbb2`
- **Accent Colors:** Aqua (`#8ec07c`), Orange (`#fe8019`), Yellow (`#fabd2f`), Red (`#fb4934`)
{{< /tab >}}
{{< tab title="Nord" >}}
- **Base Background:** `#2e3440`
- **Surface Accent:** `#3b4252`
- **Primary Text:** `#eceff4`
- **Accent Colors:** Frost Blue (`#88c0d0`), Polar Night (`#4c566a`), Aurora Green (`#a3be8c`)
{{< /tab >}}
{{< tab title="Everforest" >}}
- **Base Background:** `#2d353b`
- **Surface Accent:** `#343f44`
- **Primary Text:** `#d3c6aa`
- **Accent Colors:** Forest Green (`#a7c080`), Amber (`#dbbc7f`), Blue (`#7fbbb3`)
{{< /tab >}}
{{< /tabs >}}

### Switching Themes in `hugo.toml`
To change your active color palette, modify `params.webtuiTheme` in `hugo.toml`:

```toml
[params]
  webtuiTheme = 'catppuccin' # Options: 'catppuccin', 'gruvbox', 'nord', 'everforest'
```

---

## 2. JetBrains Mono Nerd Font & 3-Tier Font System

BashIt defaults to **JetBrains Mono Nerd Font** as its primary typeface across the entire site. Unlike generic system monospace fonts (which often fail to display Unicode Nerd Font glyphs, PUA icons, and symbols on various operating systems), JetBrains Mono Nerd Font bundles both pristine code typography and complete Nerd Font iconography in a single unified webfont.

### The 3 Customizable Font Slots

You can customize typography across three independent levels in your `hugo.toml`:
1. **Global Font (`global`)**: Controls the base site font, UI navigation, headings, badges, buttons, and tables.
2. **Paragraph Font (`paragraph`)**: Controls body text, prose paragraphs, and article reading content.
3. **Code Blocks Font (`code`)**: Controls code listings, `pre`, `code`, and terminal output blocks.

### Configuring Fonts in `hugo.toml`

```toml
[params.font]
  # 1. Global Font (defaults to "JetBrainsMono Nerd Font")
  global = "JetBrainsMono Nerd Font"

  # 2. Paragraph Font (defaults to global font)
  paragraph = "JetBrainsMono Nerd Font"

  # 3. Code Blocks Font (defaults to global font)
  code = "JetBrainsMono Nerd Font"

  # Optional webfont stylesheet URL (if using external webfonts like Google Fonts)
  # fontUrl = "https://fonts.googleapis.com/css2?family=Fira+Code:wght@400;700&display=swap"
```

### Bulletproof Glyph & Admonition Fallback

Whenever you customize fonts (e.g. choosing a non-Nerd Font such as `Inter` for paragraphs or `Fira Code` for code blocks), BashIt automatically appends `"JetBrainsMono Nerd Font", "Symbols Nerd Font", monospace` to the fallback chain. Furthermore, all admonitions, badges, and icon elements strictly isolate glyph lookups:

```css
:root {
  --font-family-fallback: "Symbols Nerd Font", monospace;
  --font-family-global: "JetBrainsMono Nerd Font", var(--font-family-fallback);
  --font-family-paragraph: var(--font-family-global);
  --font-family-code: var(--font-family-global);
}

.admonition-icon {
  font-family: "Symbols Nerd Font", "JetBrainsMono Nerd Font", monospace !important;
}
```

This guarantees that admonitions (`󰎞`, `󰋼`, `󰌵`, `󰄲`, `󱈸`, `󰚌`, etc.), badges, and terminal icons display with 100% fidelity on every browser and operating system.

---

## 3. Extending Styles via CSS `@layer`

WebTUI organizes stylesheets into three distinct cascade layers:
1. `@layer base`: Reset rules, font sizes, color variables.
2. `@layer utils`: Layout primitives (`<row>`, `<column>`, `gap-="1"`, `align-="between"`).
3. `@layer components`: ASCII borders (`box-="square"`), badges, buttons, accordions, and dialogs.

To customize or extend any component in your site without breaking specificity, define rules under `@layer components`:

```css
@layer components {
  /* Custom prompt styling for code blocks */
  .content pre code::before {
    content: "bash$ ";
    color: var(--green);
    font-weight: bold;
  }
}
```
