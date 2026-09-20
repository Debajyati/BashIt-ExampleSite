---
title: "Hello Terminal: Building with the BashIt Hugo Theme"
date: 2026-09-20T10:00:00+05:30
tags: ["bashit", "webtui", "terminal", "showcase"]
categories: ["Guides"]
draft: false
summary: "A practical starter post demonstrating WebTUI boxes, badges, tables, social embeds, and terminal tabs in BashIt."
---

Welcome to your new terminal-styled publication! This starter article demonstrates how to combine WebTUI HTML primitives, extended shortcodes, and markdown prose.

## 1. Terminal Callouts & Admonitions

Use collapsible admonitions with Nerd Font icons to highlight essential information:

{{< admonition type="tip" title="Deployment Tip" open=true >}}
BashIt builds in under 55ms with Hugo Extended and Hugo Pipes. Ensure `unsafe = true` is set in your `hugo.toml` so all WebTUI attributes render properly.
{{< /admonition >}}

{{< admonition type="warning" title="Submodule Update Reminder" open=false >}}
After cloning your repository onto a new machine, remember to initialize submodules:
`git submodule update --init --recursive`
{{< /admonition >}}

---

## 2. ASCII Data Tables

Render monospace tables with connected ASCII boundary lines:

{{< webtui-table
  headers="Environment | Engine | Latency | Status"
  rows="Edge CDN | Cloudflare Pages | <20ms | 󰄲 Operational ;; Comments | Cloudflare D1 | <45ms | 󰄲 Active ;; Build Pipeline | Hugo Extended | 52ms | 󰄲 Fast"
  box="square"
  divide="both"
>}}

---

## 3. Code Listings with Terminal Tabs

Present multiple languages or formats in interactive tabbed panes:

{{< tabs defaultTab=0 >}}
  {{< tab title="Go" >}}
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, WebTUI Terminal!")
}
```
  {{< /tab >}}

  {{< tab title="Rust" >}}
```rust
fn main() {
    println!("Hello, WebTUI Terminal!");
}
```
  {{< /tab >}}

  {{< tab title="Bash" >}}
```bash
#!/usr/bin/env bash
echo "Hello, WebTUI Terminal!"
```
  {{< /tab >}}
{{< /tabs >}}

---

## 4. Rich Social Embeds

Showcase your open source projects with live build-time cached statistics:

{{< github "Debajyati/BashIt" >}}

Embed responsive privacy-friendly YouTube videos:

{{< youtube "dQw4w9WgXcQ" >}}

---

## 5. Directory Trees & Progress

Demonstrate project directory structures using native WebTUI accordions:

{{< file-accordion title="project-root/" open=true >}}
  {{< file name="hugo.toml" icon="󰅩" >}}
  {{< file name="content/_index.md" icon="󰈙" >}}
  {{< file name="themes/BashIt/" icon="󰉋" >}}
{{< /file-accordion >}}

Terminal progress bars:

{{< webtui-progress value="85" max="100" label="Build Optimization: 85%" >}}
