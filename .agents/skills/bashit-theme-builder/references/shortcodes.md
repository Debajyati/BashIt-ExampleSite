# BashIt Shortcodes — Exhaustive Parameter Reference

> This file is the authoritative, parameter-level reference for all 79
> shortcode templates in the BashIt Hugo theme. It is intended as a lookup
> document for AI agents. For a higher-level overview with usage examples,
> see the main [SKILL.md](../SKILL.md).

---

## Naming Convention

Many BashIt shortcodes exist with **multiple aliases** — identical template
files under different names for developer convenience. The canonical name is
listed first, with aliases after `=`.

---

## 1. Social Embeds

### `youtube` = `yt` = `webtui-youtube`

Embeds a YouTube video using privacy-enhanced mode (`youtube-nocookie.com`).

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `0` (positional) | string | ✓ | Video ID or full URL (`youtube.com/watch?v=...` or `youtu.be/...`) |
| `id` | string | | Explicit video ID (alternative to positional) |
| `start` | int | | Start time in seconds |
| `autoplay` | bool | | Auto-play video |

**CSS classes**: `tui-youtube-embed`, `tui-video-container`, `tui-social-embed`, `tui-embed-header`, `tui-embed-footer`

```markdown
{{</* youtube "dQw4w9WgXcQ" */>}}
{{</* yt "https://youtu.be/dQw4w9WgXcQ" start="42" */>}}
```

---

### `x` = `tweet` = `twitter` = `webtui-x`

Embeds an X/Twitter post. Supports both live widget and static card modes.

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `0` (positional) | string | | Tweet URL or status ID |
| `url` | string | | Full tweet URL |
| `id` | string | | Status ID |
| `user` | string | | Username (for ID-based embeds) |
| `name` | string | | Display name (static card) |
| `text` | string | | Tweet text (static card — disables widget) |
| `date` | string | | Date string (static card) |
| `likes` | string | | Like count (static card) |
| `retweets` | string | | Retweet count (static card) |
| `replies` | string | | Reply count (static card) |
| `verified` | bool | | Show verified badge (static card) |
| `author` | string | | Author handle (static card) |

**CSS classes**: `tui-x-embed`, `tui-x-body`, `tui-x-container`, `twitter-tweet`, `tui-social-embed`
**External**: `https://platform.twitter.com/widgets.js`

```markdown
{{</* x "https://x.com/jack/status/20" */>}}
{{</* tweet "20" user="jack" */>}}
{{</* x user="jack" name="jack" date="Mar 21, 2006" text="just setting up my twttr" */>}}
```

---

### `instagram` = `insta` = `webtui-instagram`

Embeds an Instagram post or reel.

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `0` (positional) | string | ✓ | Post shortcode or full URL |
| `id` | string | | Explicit post ID |
| `user` / `account` | string | | Username (static card) |
| `image` / `src` | string | | Image URL (static card) |
| `caption` | string | | Caption text (static card) |
| `likes` | string | | Like count (static card) |

**CSS classes**: `tui-instagram-embed`, `tui-instagram-frame`, `tui-instagram-image`, `tui-instagram-caption`, `tui-social-embed`
**External**: `https://www.instagram.com/p/{id}/embed`

```markdown
{{</* instagram "BWNjjyYFxVx" */>}}
{{</* insta "https://www.instagram.com/p/BWNjjyYFxVx/" */>}}
{{</* instagram user="natgeo" image="/img/post.jpg" caption="Amazing photo" */>}}
```

---

### `peerlist` = `webtui-peerlist`

Embeds a Peerlist post with auto-resizing iframe.

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `0` (positional) | string | ✓ | Post ID or full URL |
| `id` | string | | Explicit post ID |
| `url` | string | | Full Peerlist URL |

**CSS classes**: `tui-peerlist-embed`, `tui-peerlist-frame`, `tui-social-embed`
**External**: `https://peerlist.io/embeds/posts?postId={id}`

```markdown
{{</* peerlist "ACTHLKLLGMGQJGPBKI977NQ89DLP8M" */>}}
```

---

### `reddit` = `webtui-reddit`

Embeds a Reddit thread.

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `0` (positional) | string | ✓ | Full Reddit thread URL |
| `url` | string | | Explicit URL |
| `subreddit` | string | | Subreddit name (static card) |
| `author` | string | | Author username (static card) |
| `title` | string | | Post title (static card) |
| `upvotes` | string | | Upvote count (static card) |
| `comments` | string | | Comment count (static card) |

**CSS classes**: `tui-reddit-embed`, `tui-reddit-container`, `reddit-embed-bq`, `tui-social-embed`
**External**: `https://embed.reddit.com/widgets.js`

```markdown
{{</* reddit "https://www.reddit.com/r/golang/comments/..." */>}}
```

---

### `github` = `webtui-github`

Embeds a GitHub repository card with live stats from the GitHub API.

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `0` (positional) | string | ✓ | `owner/repo` or full GitHub URL |
| `repo` | string | | Explicit `owner/repo` |
| `language` | string | | Override language (manual fallback) |
| `stars` | string | | Override star count |
| `forks` | string | | Override fork count |
| `license` | string | | Override license |
| `description` | string | | Override description |

**CSS classes**: `tui-github-embed`, `tui-github-body`, `tui-social-embed`
**External**: `https://api.github.com/repos/{owner/repo}`

> Uses `site.Store` for build-time caching to avoid GitHub API rate limits.

```markdown
{{</* github "gohugoio/hugo" */>}}
{{</* github "https://github.com/Debajyati/BashIt" */>}}
{{</* github repo="owner/repo" language="Go" stars="85000" description="..." */>}}
```

---

### `devto` = `webtui-devto`

Embeds a Dev.to article card with build-time API fetching.

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `0` (positional) | string | ✓ | Full Dev.to article URL |
| `id` | string | | Article ID |
| `url` | string | | Explicit URL |
| `title` | string | | Override title (manual) |
| `author` | string | | Author name |
| `handle` | string | | Author handle |
| `date` | string | | Publication date |
| `readtime` | string | | Reading time |
| `tags` | string | | Comma-separated tags |
| `reactions` | string | | Reaction count |
| `comments` | string | | Comment count |
| `summary` | string | | Article summary |

**CSS classes**: `tui-devto-embed`, `tui-devto-body`, `tui-social-embed`
**External**: `https://dev.to/api/articles/{id}`

```markdown
{{</* devto "https://dev.to/user/my-article" */>}}
```

---

### `dailydev` = `webtui-dailydev`

Embeds a Daily.dev article card via OpenGraph scraping.

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `0` (positional) | string | ✓ | Daily.dev short URL |
| `url` | string | | Explicit URL |
| `title` | string | | Override title |
| `summary` | string | | Override summary |
| `image` | string | | Override image |
| `source` | string | | Source name |
| `upvotes` | string | | Upvote count |
| `comments` | string | | Comment count |
| `readtime` | string | | Reading time |
| `tags` | string | | Tags |
| `scraperApi` | string | | Custom scraper URL |

**CSS classes**: `tui-dailydev-embed`, `tui-dailydev-body`, `tui-dailydev-image`, `tui-social-embed`
**External**: `https://xogapi.ddebajyati.workers.dev` (or custom `scraperApi`)

```markdown
{{</* dailydev "https://dly.to/g9vaRB1agcs" */>}}
```

---

### `linkcard` = `webtui-linkcard`

Universal OpenGraph link preview card.

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `0` (positional) | string | ✓ | URL to preview |
| `url` | string | | Explicit URL |
| `title` | string | | Override title |
| `desc` / `description` | string | | Override description |
| `image` | string | | Override image |
| `compact` | bool | | Compact card layout |
| `noimage` | bool | | Suppress image |
| `prefer` | string | | Preferred metadata source |
| `scraperApi` | string | | Custom scraper URL |

**CSS classes**: `tui-linkcard-embed`, `tui-linkcard-desc`, `tui-linkcard-image`, `tui-social-embed`
**External**: `https://xogapi.ddebajyati.workers.dev` (or custom `scraperApi`)

```markdown
{{</* linkcard "https://gohugo.io" */>}}
{{</* linkcard "https://example.com" compact=true */>}}
{{</* linkcard "https://example.com" noimage=true */>}}
```

---

## 2. Content Components

### `admonition` = `webtui-admonition`

Renders a collapsible callout/alert with Nerd Font icon and colored border.

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `type` | string | ✓ | One of: `note`, `abstract`, `info`, `tip`, `success`, `question`, `warning`, `failure`, `danger`, `bug`, `example`, `quote`, `important`, `caution` |
| `title` | string | | Custom title (defaults to type name) |
| `open` | bool | | `true` = expanded by default, `false` = collapsed |

**CSS classes**: `admonition`, `admonition-badge`, `admonition-icon`, `admonition-title`, `admonition-content`, `admonition-chevron`, `admonition-{type}`

```markdown
{{</* admonition type="warning" title="Caution" open=true */>}}
Be careful with this operation.
{{</* /admonition */>}}
```

---

### `webtui-callout`

Simple alert box (less complex than admonition).

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `title` | string | | Callout title |
| `variant` | string | | Color variant (e.g., `blue`, `green`, `red`) |

**CSS classes**: `webtui-callout-box`, `callout-body`, `admonition-badge`

```markdown
{{</* webtui-callout title="Note" variant="blue" */>}}
Simple callout content.
{{</* /webtui-callout */>}}
```

---

### `image` = `webtui-image`

Enhanced image with alignment, boxes, and captions.

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `src` / `0` | string | ✓ | Image source path |
| `alt` | string | | Alt text |
| `caption` | string | | Figure caption |
| `align` | string | | `left`, `center`, `right` |
| `width` | string | | CSS width value |
| `height` | string | | CSS height value |
| `box` | string | | `square`, `round` (ASCII border) |
| `href` / `link` / `linked` | string | | Wrap image in link |
| `title` | string | | Title attribute |

**CSS classes**: `tui-image-figure`, `tui-image-wrapper`, `tui-image-caption`, `tui-caption-prefix`

```markdown
{{</* image src="/images/demo.png" caption="Figure 1" align="center" box="square" */>}}
```

---

### `audio` = `webtui-audio`

Terminal-styled HTML5 audio player.

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `src` / `0` | string | ✓ | Audio file path |
| `title` | string | | Track title |
| `artist` | string | | Artist name |

**CSS classes**: `tui-audio-box`, `tui-audio-header`, `tui-audio-badge`

```markdown
{{</* audio src="/audio/track.mp3" title="Demo Track" artist="Producer" */>}}
```

---

### `tabs` = `webtui-tabs` (container)

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `defaultTab` | int | | Zero-indexed default active tab |

**CSS classes**: `tui-tabs-container`, `tui-tab-nav`, `tui-tab-panels`

### `tab` = `webtui-tab` (panel)

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `title` / `0` | string | ✓ | Tab label |

**CSS classes**: `tui-tab-panel`

```markdown
{{</* tabs defaultTab=0 */>}}
  {{</* tab title="Tab A" */>}}Content A{{</* /tab */>}}
  {{</* tab title="Tab B" */>}}Content B{{</* /tab */>}}
{{</* /tabs */>}}
```

---

### `webtui-accordion`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `title` / `0` | string | ✓ | Accordion summary text |
| `open` | bool | | Expanded by default |
| `variant` | string | | Visual variant (e.g., `directory`) |

**CSS classes**: `webtui-accordion-content`

---

### `file-accordion` = `webtui-file-accordion`

Directory tree accordion with folder icons.

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `title` / `0` | string | ✓ | Directory name |
| `open` | bool | | Expanded by default |

---

### `file` = `webtui-file`

File entry inside a directory accordion.

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `name` / `0` | string | ✓ | File name |
| `icon` | string | | Nerd Font icon (default: ``) |
| `url` / `href` | string | | Link target |

**CSS classes**: `webtui-file-item`, `webtui-file-icon`

---

## 3. Data Display & UI Widgets

### `webtui-table`

Monospace ASCII table with grid dividers.

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `headers` | string | ✓ | Column headers delimited by `\|` |
| `rows` | string | ✓ | Data rows — columns by `\|`, rows by `;;` |
| `box` | string | | Border style: `square`, `round`, `double` |
| `divide` | string | | Grid lines: `vertical`, `horizontal`, `both` |
| `colSep` | string | | Custom column separator (default: `\|`) |
| `rowSep` | string | | Custom row separator (default: `;;`) |

**CSS classes**: `webtui-table-wrapper`

```markdown
{{</* webtui-table
  headers="Name | Status | Action"
  rows="Server A | Online | ✓ ;; Server B | Offline | ✗"
  box="square" divide="both"
*/>}}
```

---

### `webtui-badge`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `variant` | string | | Color name (e.g., `green`, `blue`, `mauve`) |
| `color` | string | | Alternative to variant |

```markdown
{{</* webtui-badge variant="green" */>}}ACTIVE{{</* /webtui-badge */>}}
```

---

### `webtui-box`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `style` | string | | `square`, `round`, `double` |
| `shear` | string | | `both`, `top`, `bottom` |

```markdown
{{</* webtui-box style="square" */>}}Content{{</* /webtui-box */>}}
```

---

### `webtui-button`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `variant` | string | | Color variant |
| `size` | string | | `small`, `default`, `large`, `full` |

```markdown
{{</* webtui-button variant="foreground0" size="small" */>}}Click{{</* /webtui-button */>}}
```

---

### `webtui-progress`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `value` | int | ✓ | Current value |
| `max` | int | | Maximum value (default: 100) |
| `label` | string | | Display label (e.g., `"85%"`) |
| `class` | string | | Additional CSS class |

```markdown
{{</* webtui-progress value="85" max="100" label="85%" */>}}
```

---

### `webtui-spinner`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `variant` | string | | `cursor`, `dots`, `arrows`, `bar-vertical`, `bar-horizontal`, `cross`, `square`, `pie`, `half` |

```markdown
{{</* webtui-spinner variant="cursor" */>}}
```

---

### `webtui-mark`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `variant` | string | | Color name for background |
| `bg` | string | | Background color |
| `fg` | string | | Foreground color |

```markdown
{{</* webtui-mark variant="blue" */>}}Highlighted{{</* /webtui-mark */>}}
```

---

### `webtui-tooltip`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `tip` | string | ✓ | Tooltip text |
| `position` | string | | `top`, `bottom`, `left`, `right` |

```markdown
{{</* webtui-tooltip tip="Extra info" position="top" */>}}Hover me{{</* /webtui-tooltip */>}}
```

---

### `webtui-popover`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `summary` | string | ✓ | Popover trigger text |
| `position` | string | | Placement (e.g., `bottom left`) |

```markdown
{{</* webtui-popover summary="Options" position="bottom left" */>}}
Menu items here.
{{</* /webtui-popover */>}}
```

---

### `webtui-switch`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `name` | string | | Form field name |
| `checked` | bool | | Initial state |
| `disabled` | bool | | Disabled state |
| `id` | string | | Element ID |
| `value` | string | | Form value |
| `bar` | string | | Track style: `thick`, `line`, `thin` |
| `size` | string | | Size variant |

---

### `webtui-checkbox`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `name` | string | | Form field name |
| `checked` | bool | | Initial state |
| `disabled` | bool | | Disabled state |
| `id` | string | | Element ID |
| `value` | string | | Form value |

---

### `webtui-radio`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `name` | string | ✓ | Radio group name |
| `value` | string | ✓ | Radio value |
| `checked` | bool | | Initial state |
| `disabled` | bool | | Disabled state |
| `id` | string | | Element ID |

---

### `webtui-separator`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `direction` | string | | `horizontal`, `vertical` |
| `cap` | string | | `bisect`, `edge` |

```markdown
{{</* webtui-separator direction="horizontal" cap="bisect" */>}}
```

---

## 4. Interactive & Visualization

### `mermaid` = `webtui-mermaid`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `title` | string | | Diagram title |

Inner content: Mermaid diagram syntax.
**Lazy-loads**: `mermaid@10` from jsDelivr with dark Catppuccin theme.

```markdown
{{</* mermaid title="Flow" */>}}
graph TD
  A --> B --> C
{{</* /mermaid */>}}
```

---

### `echarts` = `webtui-echarts`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `width` | string | | CSS width (default: `100%`) |
| `height` | string | | CSS height (default: `300px`) |
| `title` | string | | Chart title |

Inner content: JSON ECharts option object.
**Lazy-loads**: `echarts@5` from jsDelivr.

```markdown
{{</* echarts width="100%" height="320px" title="Stats" */>}}
{"xAxis":{"type":"category","data":["A","B"]},"yAxis":{"type":"value"},"series":[{"data":[10,20],"type":"bar"}]}
{{</* /echarts */>}}
```

---

### `mapbox` = `webtui-mapbox`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `lat` / `0` | float | ✓ | Latitude |
| `lng` / `1` | float | ✓ | Longitude |
| `zoom` / `2` | int | | Zoom level (default: 9) |
| `title` | string | | Map title |
| `height` | string | | CSS height |
| `width` | string | | CSS width |
| `marked` | bool | | Show marker at coordinates |

**Fallback**: If no Mapbox token is configured, uses Leaflet + CARTO dark tiles.

```markdown
{{</* mapbox lat="37.7749" lng="-122.4194" zoom="11" title="SF" */>}}
```

---

### `typeit` = `webtui-typeit`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `speed` | int | | Typing speed in ms (default: 50) |
| `cursor` | string | | Cursor character (default: `█`) |
| `prompt` | string | | Terminal prompt prefix (default: `> `) |
| `title` | string | | Box title |

Inner content: Text to type.

```markdown
{{</* typeit speed="45" cursor="█" prompt="guest@bashit:~$ " title="Demo" */>}}
echo "Hello, World!"
{{</* /typeit */>}}
```

---

### `math` = `webtui-math`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `title` | string | | Block title |

Inner content: LaTeX math expression.
**Lazy-loads**: KaTeX 0.16.8 from jsDelivr.

```markdown
{{</* math title="Euler's Identity" */>}}
e^{i\pi} + 1 = 0
{{</* /math */>}}
```

---

## 5. Comment Systems

### `bbs` = `webtui-bbs`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `api` | string | | Override Worker API URL |
| `mock` | bool | | Override mock mode |
| `slug` | string | | Override post slug |

Auto-configured from `[params.comment.cloudflare]` in `hugo.toml`.

---

### `giscus` = `webtui-giscus`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `repo` | string | | Override GitHub repo |
| `repoId` | string | | Override repo ID |
| `category` | string | | Override category |
| `categoryId` | string | | Override category ID |
| `theme` | string | | Override Giscus theme |

Auto-configured from `[params.comment.giscus]` in `hugo.toml`.

---

### `disqus` = `webtui-disqus`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `shortname` | string | ✓ | Disqus shortname |

---

## 6. Utility Shortcodes

### `gist` = `webtui-gist`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `user` / `0` | string | ✓ | GitHub username |
| `id` / `1` | string | ✓ | Gist ID |
| `file` / `2` | string | | Specific file in gist |

```markdown
{{</* gist "octocat" "abc123" */>}}
{{</* gist user="octocat" id="abc123" file="main.go" */>}}
```

---

### `showcase` = `webtui-showcase`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `title` / `0` | string | ✓ | Project title |
| `summary` / `1` | string | | Project description |
| `link` / `2` | string | | Primary link URL |
| `image` / `3` | string | | Preview image |
| `linkExtra` / `link_extra` / `4` | string | | Secondary link |

```markdown
{{</* showcase title="My Project" summary="Description" link="https://..." image="/img/preview.png" */>}}
```

---

### `friend` = `webtui-friend`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `name` / `0` | string | ✓ | Person's name |
| `title` / `1` | string | | Job title |
| `url` / `link` / `2` | string | | Profile URL |
| `avatar` / `3` | string | | Avatar image URL |
| `bio` | string | | Short bio |

---

### `person` = `webtui-person`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `name` / `0` | string | ✓ | Person's name |
| `bio` / `1` | string | | Short bio |
| `url` / `2` | string | | Profile URL |
| `avatar` / `3` | string | | Avatar image URL |

---

### `style` = `webtui-style`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `0` (positional) | string | ✓ | CSS styles or class |
| `tag` | string | | HTML tag (default: `span`) |
| `style` | string | | Explicit CSS styles |
| `class` | string | | CSS class |

```markdown
{{</* style "color: var(--green); font-weight: bold;" */>}}Green bold text{{</* /style */>}}
```

---

### `script` = `webtui-script`

| Param | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `src` / `0` | string | | External script URL |

Inner content: Inline JavaScript (if no `src`).

```markdown
{{</* script src="/js/custom.js" */>}}
```
