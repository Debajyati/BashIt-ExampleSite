---
title: "WebTUI Social Embeds: YouTube, X, Instagram, Peerlist, Dev.to, Daily.dev, GitHub, Reddit & Link Cards"
date: 2026-09-12T20:00:00+05:30
tags: ["bashit", "embeds", "social", "components", "privacy", "shortcodes", "webtui", "peerlist"]
categories: ["Documentation", "Guides"]
draft: false
summary: "A complete guide and visual showcase of BashIt's terminal-themed social embeds: YouTube, X (Twitter), Instagram, Peerlist, Reddit, Dev.to, GitHub, Daily.dev, and OpenGraph Link Cards"
---

**BashIt** has a complete suite of **WebTUI-styled social cards and privacy-first embeds**. Each shortcode renders lightweight, authentic terminal UI cards with monospace typography, Symbols Nerd Font iconography, Catppuccin color highlights, and build-time metadata extraction.

---

### 1. YouTube Terminal CRT Embed

Embed YouTube videos inside a privacy-enhanced terminal frame powered by `youtube-nocookie.com`. You can provide an 11-character video ID, a standard YouTube URL (`https://www.youtube.com/watch?v=...`), or a shortened URL (`https://youtu.be/...`). The shortcode automatically extracts the video ID, stripping all manual parameters like `title` and `quality`.

{{< youtube "dQw4w9WgXcQ" >}}

<details is-="accordion">
  <summary>󰅩 View YouTube Embed Syntax</summary>

```markdown
<!-- Positional video ID -->
{{</* youtube "dQw4w9WgXcQ" */>}}

<!-- Full YouTube URL with automatic ID extraction -->
{{</* youtube "https://www.youtube.com/watch?v=dQw4w9WgXcQ" */>}}

<!-- Quick alias with start timestamp parameter -->
{{</* yt "https://youtu.be/dQw4w9WgXcQ" start="42" */>}}
```

</details>

---

### 2. X (formerly Twitter) Terminal Post

Showcase posts from X / Twitter in a native WebTUI terminal frame. Simply provide a tweet URL or status ID—BashIt embeds Twitter's official dark-themed responsive widget (`platform.twitter.com/widgets.js`) with authentic media, live author info, and verified status. Manual overrides with custom text are also fully supported.

{{< x "https://x.com/_devJNS/status/1936459484156600351?s=20" >}}

<details is-="accordion">
  <summary>󰅩 View X / Twitter Shortcode Syntax</summary>

```markdown
<!-- 1. Automatic: Single Tweet URL (official dark-mode responsive embed) -->
{{</* x "https://x.com/_devJNS/status/1936459484156600351?s=20" */>}}

<!-- 2. Positional Status ID with user handle -->
{{</* tweet "20" user="jack" */>}}

<!-- 3. Manual override with authentic custom text (no fake numbers) -->
{{</* x user="@antigravity" name="Google DeepMind" date="Sep 12, 2026" text="Excited to release our latest agentic coding workflows in Antigravity! 🚀 Pairing autonomous reasoning with instant terminal feedback transforms developer velocity." */>}}
```

</details>

---

### 3. Instagram Official Responsive Embed

Embed official Instagram posts and reels seamlessly. BashIt accepts either an Instagram URL or a post ID, embedding Instagram's responsive framed viewer cleanly inside a WebTUI ASCII border.

{{< instagram "https://www.instagram.com/reel/DdHJ1PbtR9I/?stkn=MTh6OW9vbDh4c290ag==" >}}

<details is-="accordion">
  <summary>󰅩 View Instagram Shortcode Syntax</summary>

```markdown
<!-- 1. By Post ID (official captioned embed) -->
{{</* instagram "BWNjjyYFxVx" */>}}

<!-- 2. By Full Post URL or Reel URL -->
{{</* insta "https://www.instagram.com/p/BWNjjyYFxVx/" */>}}

<!-- 3. Offline / Static TUI Image Card Fallback -->
{{</* instagram user="@terminal_minimalism" image="/favicon.svg" caption="Late-night mechanical keyboard builds and monochrome terminal sessions. Pure tactile bliss." */>}}
```

</details>

---

### 4. Peerlist Professional Post Embed

Embed interactive posts and project updates from Peerlist. Pass a post ID (`ACTHOK8A7BG66JBGR17ALNR98QPL7Q`), a full post URL (`https://peerlist.io/.../post/...`), or an embed URL. BashIt frames the official responsive embed inside a WebTUI ASCII container and dynamically adjusts frame height using Peerlist's `setHeight` postMessage protocol.

{{< peerlist "ACTHLKLLGMGQJGPBKI977NQ89DLP8M" >}}

<details is-="accordion">
  <summary>󰅩 View Peerlist Shortcode Syntax</summary>

```markdown
<!-- 1. By Post ID -->
{{</* peerlist "ACTHOK8A7BG66JBGR17ALNR98QPL7Q" */>}}

<!-- 2. By Full Peerlist Post URL -->
{{</* peerlist "https://peerlist.io/scroll/post/ACTHLKLLGMGQJGPBKI977NQ89DLP8M" */>}}

<!-- 3. By Named Parameter -->
{{</* peerlist id="ACTHLKLLGMGQJGPBKI977NQ89DLP8M" */>}}
```

</details>

---

### 5. Reddit Thread & Post Embed

Embed Reddit discussions from technical(not restricted to 😅) communities (`r/commandline`, `r/golang`, `r/unixporn`, etc.) using Reddit's official dark-themed embed blockquote, framed inside a WebTUI terminal box. Give it a post URL, and BashIt automatically parses the subreddit and title.

{{< reddit url="https://www.reddit.com/r/zerotomasteryio/comments/1wdb1g9/literally_all_you_do/" author="Crafty_Sort_5946" subreddit="zerotomasteryio" title="Literally all you do." >}}

<details is-="accordion">
  <summary>󰅩 View Reddit Shortcode Syntax</summary>

```markdown
<!-- 1. Automatic from URL -->
{{</* reddit "https://www.reddit.com/r/zerotomasteryio/comments/1wdb1g9/literally_all_you_do/" */>}}

<!-- 2. With author & subreddit metadata -->
{{</* reddit url="https://www.reddit.com/r/zerotomasteryio/comments/1wdb1g9/literally_all_you_do/" author="Crafty_Sort_5946" subreddit="zerotomasteryio" title="Literally all you do." */>}}
```

</details>

---

### 6. GitHub Repository Card

Render live-styled repository metadata cards. Pass a repo name like `gohugoio/hugo` or a full repository URL; BashIt queries GitHub's public API at build time to populate the repository description, primary language, star tally, and fork count.

{{< github "gohugoio/hugo" >}}

<details is-="accordion">
  <summary>󰅩 View GitHub Shortcode Syntax</summary>

```markdown
<!-- 1. Auto-fetch live repository stats via GitHub API -->
{{</* github "gohugoio/hugo" */>}}

<!-- 2. Full GitHub URL -->
{{</* github "https://github.com/webtui/BashIt" */>}}

<!-- 3. Custom / Offline manual parameters -->
{{</* github repo="webtui/BashIt" language="CSS / Go" stars="2.8k" forks="345" license="MIT" description="High-performance retro-modern terminal theme for Hugo built on WebTUI CSS." */>}}
```

</details>

---

### 7. Dev.to Community Card

Embed community articles from Dev.to. By passing the article URL, BashIt uses Dev.to's public API at build time to fetch the real title, reading time, author, summary, tags, and engagement counters.

{{< devto "https://dev.to/superfunicular/from-12-september-2026-eu-connected-products-must-be-accessible-by-design-what-that-asks-of-3jjd" >}}

<details is-="accordion">
  <summary>󰅩 View Dev.to Shortcode Syntax</summary>

```markdown
<!-- 1. Auto-fetch via Dev.to Public API from URL -->
{{</* devto "https://dev.to/superfunicular/from-12-september-2026-eu-connected-products-must-be-accessible-by-design-what-that-asks-of-3jjd" */>}}

<!-- 2. Manual / Offline Card -->
{{</* devto title="Why CSS Layers (@layer) Are the Future of Component Architecture" author="Alex Rivera" handle="arivera_dev" date="Sep 10, 2026" readtime="5 min read" tags="css, webdev" summary="Managing CSS specificity in complex design systems with CSS Cascade Layers." url="https://dev.to" */>}}
```

</details>

---

### 8. Daily.dev Curated Transmission

Feature curated engineering posts and trending developer updates from Daily.dev with automatic social share preview image extraction (query parameters cleanly stripped) and OpenGraph summary metadata.

{{< dailydev "https://dly.to/g9vaRB1agcs" >}}

<details is-="accordion">
  <summary>󰅩 View Daily.dev Shortcode Syntax</summary>

```markdown
<!-- 1. Auto-fetch post title, summary, and clean social share preview image -->
{{</* dailydev "https://dly.to/g9vaRB1agcs" */>}}

<!-- 2. Custom curated engineering post with optional verified stats -->
{{</* dailydev url="https://app.daily.dev" title="Rust in Linux Kernel 6.12" source="Kernel Newbies" summary="A technical review of newly stabilized Rust abstractions in the Linux kernel tree." */>}}
```

</details>

---

### 9. WebTUI OpenGraph Link Card

For arbitrary articles, documentation links, and external references, the `linkcard` shortcode fetches OpenGraph / Twitter Card metadata at build time via Cloudflare Workers and formats it into a WebTUI monospace terminal card.

{{< linkcard "https://gohugo.io" >}}

<details is-="accordion">
  <summary>󰅩 View Link Card Shortcode Syntax</summary>

```markdown
<!-- Standard OpenGraph link card -->
{{</* linkcard "https://gohugo.io" */>}}

<!-- Compact card mode without description -->
{{</* linkcard "https://github.com" compact=true */>}}

<!-- Without preview thumbnail image -->
{{</* linkcard "https://kernel.org" noimage=true */>}}

<!-- Using custom or self-hosted OpenGraph scraper API -->
{{</* linkcard "https://example.com" scraperApi="https://my-scraper.workers.dev" */>}}
```

> **Configuring the OpenGraph Scraper:**
> By default, link cards and Daily.dev cards use the built-in OpenGraph microservice. You can customize the scraper endpoint globally in `hugo.toml`:
> ```toml
> [params.opengraph]
>   scraperApi = "https://your-custom-scraper.workers.dev"
> ```

</details>

---

### Summary of Embed Shortcodes

| Shortcode | Platform | Automatic Fetch Source | Syntax Example |
|---|---|---|---|
| `{{</* youtube */>}}` / `{{</* yt */>}}` | YouTube | Privacy Embed (`youtube-nocookie.com`) | `{{</* youtube "dQw4w9WgXcQ" */>}}` |
| `{{</* x */>}}` / `{{</* tweet */>}}` | X / Twitter | Official Dark Widget (`platform.twitter.com`) | `{{</* x "https://x.com/jack/status/20" */>}}` |
| `{{</* instagram */>}}` / `{{</* insta */>}}` | Instagram | Official Captioned Iframe | `{{</* instagram "BWNjjyYFxVx" */>}}` |
| `{{</* peerlist */>}}` | Peerlist | Official Responsive Iframe + Resize Listener | `{{</* peerlist "ACTHOK8A7BG66JBGR17ALNR98QPL7Q" */>}}` |
| `{{</* reddit */>}}` | Reddit | Official Dark Widget & URL Parser | `{{</* reddit "https://reddit.com/r/.../..." */>}}` |
| `{{</* github */>}}` | GitHub | GitHub REST API (`api.github.com`) | `{{</* github "gohugoio/hugo" */>}}` |
| `{{</* devto */>}}` | Dev.to | Dev.to REST API (`dev.to/api`) | `{{</* devto "https://dev.to/.../..." */>}}` |
| `{{</* dailydev */>}}` | Daily.dev | OpenGraph API + Post Image Cleaned | `{{</* dailydev "https://dly.to/g9vaRB1agcs" */>}}` |
| `{{</* linkcard */>}}` | Universal Link | OpenGraph API (`xogapi`) | `{{</* linkcard "https://gohugo.io" */>}}` |
