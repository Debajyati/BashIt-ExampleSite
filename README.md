# BashIt Sample & Documentation Site 🚀

[![Hugo Extended](https://img.shields.io/badge/Hugo%20Extended-%3E%3D0.146.0-blue?style=flat-square&logo=hugo)](https://gohugo.io/)
[![Theme Submodule](https://img.shields.io/badge/Theme-Debajyati%2FBashIt-teal?style=flat-square)](https://github.com/Debajyati/BashIt)
[![WebTUI CSS](https://img.shields.io/badge/CSS-WebTUI-mauve?style=flat-square)](https://github.com/webtui/webtui)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

The official live demo, documentation portal, and component showcase for the **[BashIt](https://github.com/Debajyati/BashIt)** Hugo theme.

This site demonstrates every feature of BashIt in action, including terminal typography, interactive charts, geographic maps, math formulas, media embeds, and the serverless **Terminal BBS** comment engine.

---

## 📖 What's Included

- **Core Documentation**: [`/posts/bashit-theme-docs`](content/posts/bashit-theme-docs.md) — Architecture, installation, ASCII boxes, badges, buttons, progress bars, tables, and popovers.
- **Extended Shortcodes**: [`/posts/extended-shortcodes`](content/posts/extended-shortcodes.md) — 14 terminal admonition callouts, Mermaid.js diagrams, Apache ECharts, Mapbox/Leaflet maps, and KaTeX math.
- **Terminal BBS Architecture**: [`/posts/terminal-bbs-comments`](content/posts/terminal-bbs-comments.md) — Step-by-step guide to deploying a zero-tracking, ad-free comment backend on Cloudflare Workers + D1.
- **Typography & Themes**: [`/posts/themes-and-typography`](content/posts/themes-and-typography.md) — JetBrains Mono Nerd Font, 3-tier font customization, and Catppuccin Mocha color customization.
- **Social Media Cards**: [`/posts/social-embeds`](content/posts/social-embeds.md) — Monospace embeds for YouTube CRT, X (Twitter), Instagram, Peerlist, GitHub, Dev.to, Daily.dev, and Reddit.

---

## 🏃 Getting Started

### 1. Clone with Submodules

Because the BashIt theme is included as a Git submodule, clone this repository with the `--recurse-submodules` flag:

```bash
git clone --recurse-submodules https://github.com/Debajyati/sample.git
cd sample
```

If you already cloned the repository without `--recurse-submodules`, initialize and pull the submodule:

```bash
git submodule update --init --recursive
```

### 2. Install Dependencies

Install the theme's local WebTUI package dependencies:

```bash
cd themes/BashIt
npm install
cd ../..
```

### 3. Start the Development Server

Launch Hugo's local development server with draft rendering enabled:

```bash
hugo server -D
```

Open your browser and navigate to **`http://localhost:1313/`** to view the live site with instant hot-reloading.

---

## 📁 Repository Structure

```text
├── content/               # Markdown content and documentation articles
│   ├── _index.md          # Homepage / Landing page
│   └── posts/             # Technical guides and feature showcases
├── layouts/               # Shortcodes and partial overrides
├── static/                # Static assets (images, icons, etc.)
├── themes/
│   └── BashIt/            # Git submodule tracking github.com/Debajyati/BashIt
├── hugo.toml              # Site configuration file
├── .gitmodules            # Submodule configuration
└── .gitignore             # Git ignore file (excludes public/, node_modules/, etc.)
```

---

## 🔄 Updating the Theme Submodule

To update the `themes/BashIt` submodule to the latest upstream commit:

```bash
git submodule update --remote --merge
git commit -am "chore: update BashIt theme submodule to latest commit"
```

---

## 🔗 Related Repositories

- **Theme Repository**: [Debajyati/BashIt](https://github.com/Debajyati/BashIt) — The standalone Hugo theme repository.
- **Comment Backend**: [Debajyati/bashit-comments](https://github.com/Debajyati/bashit-comments) — Serverless Cloudflare Workers + D1 SQLite backend for the Terminal BBS comment engine.

---

## 📄 License

This site and the BashIt theme are open-source and licensed under the [MIT License](LICENSE).
Built with [Hugo](https://gohugo.io/) and [WebTUI](https://github.com/webtui/webtui).
