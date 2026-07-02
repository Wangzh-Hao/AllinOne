# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

**AllinOne** is a Hugo theme for blogging and personal websites. It is used as a subdirectory (`themes/AllinOne/`) inside a Hugo site, not as a standalone project.

- **Author**: ZHENG Zi'ou (Orianna)
- **License**: MIT
- **Min Hugo version**: v0.46 (tested up to v0.163+; recent compatibility fixes applied)
- **Demo**: https://orianna-zzo.github.io/AllinOne-html/

## Key Features

- Full image carousel on homepage with configurable slide directory
- Hierarchical TOC with Scrollspy (h1~h4), opt-in per post via `toc: true`
- Katex for LaTeX math rendering (opt-in via `katex: true` in config)
- Syntax highlighting via highlight.js (opt-in via `highlightjs: true`)
- Google Analytics integration (uses Hugo's internal template)
- Font Awesome 5 icons (loaded from `css/vendors-extensions/fontawesome/`)
- Tags, Series, and Categories taxonomies
- Post card list with summary and optional intro picture (`img` frontmatter)
- Series sidebar recommendation cards with metadata from `data/series.toml`
- Previous/Next post navigation
- Pagination for list pages (uses Hugo's internal pagination template)
- CJK language support (`hasCJKLanguage` config)
- i18n: English and Simplified Chinese translation files

## Template Composition (How Pages Render)

All page templates extend `layouts/_default/baseof.html`, which defines two `block` slots:

```
baseof.html
  ├── block "header" → default: site-navbar.html + page-header.html
  └── block "main"   → default: (empty, must be filled by child)
```

| Template | Overrides | Used For |
|----------|-----------|----------|
| `layouts/index.html` | **header** (replaces `page-header` with `homepage-header` carousel) + **main** (post cards with sidebar) | Homepage (`/`) |
| `layouts/_default/single.html` | **main** (post content, prev/next nav, tags, TOC sidebar) | Individual blog posts / pages |
| `layouts/_default/list.html` | **main** (paginated post cards with sidebar) | Section listing (e.g. `/blog/`) |
| `layouts/_default/taxonomy.html` | **main** (paginated post cards filtered by taxonomy) | Taxonomy term pages (tags, categories, series) |
| `layouts/_default/terms.html` | **main** (list of all terms in a taxonomy) | Taxonomy root pages (e.g. `/tags/`, `/categories/`) |
| `layouts/404.html` | **main** (full override, no baseof) | 404 page |
| `layouts/robots.txt` | (standalone template) | `/robots.txt` |

## Project Structure

| Path | Description |
|------|-------------|
| `layouts/` | Hugo Go templates (baseof, single, list, partials, shortcodes, 404, robots) |
| `layouts/_default/baseof.html` | Base HTML skeleton with `header` and `main` blocks |
| `layouts/_default/` | Core page types: `single.html`, `list.html`, `taxonomy.html`, `terms.html` |
| `layouts/partials/` | Reusable template fragments (see Partials section below) |
| `layouts/shortcodes/` | Custom Hugo shortcodes: `center`, `img-no-border` |
| `static/` | Static assets: CSS, JS, fonts, images, vendor libraries |
| `static/css/vendors/` | Unmodified third-party CSS (Bootstrap 4, MDB, highlight.js themes) |
| `static/css/vendors-extensions/` | Modified/extension third-party CSS (MDB overrides, Font Awesome 5) |
| `static/js/vendors/` | Third-party JS (jQuery 3.3.1, Bootstrap 4, MDB, highlight.js, Katex, holder.js, popper) |
| `static/js/vendors-extensions/` | Modified/extension third-party JS (Bootstrap 4 custom build) |
| `static/img/` | Theme images (profile `zheng.png`, avatar `profile.jpg`, header slides, SVG icons, masks) |
| `static/fonts/fontawesome/` | Font Awesome 5 font files |
| `i18n/` | Translation files (`en.yaml`, `zh-Hans.yaml`) |
| `archetypes/` | Default content template (`default.md`) |
| `data/` | Series metadata (`series.toml` for name/img/summary) |
| `exampleSite/` | Example Hugo site with sample content (Chinese posts), config, and images |

## Partials (Functional Groups)

### Head / Scripts (loaded on every page)
- **`head.html`** — `<head>` element: meta tags, Open Graph, Font Awesome, Google Fonts, Bootstrap+MDB CSS, KaTeX CSS, highlight.js theme, favicon, RSS link, HTTPS redirect script
- **`site-scripts.html`** — End-of-body scripts: jQuery + smooth-scroll, popper, holder.js, Bootstrap JS, MDB JS, main.js, highlight.js init, KaTeX auto-render, Google Analytics, WOW.js init

### Navigation & Page Headers
- **`site-navbar.html`** — Fixed-top bootstrap navbar with avatar brand, Home link + all `menu.main` entries; active state detection via `.RelPermalink` and `.Type`
- **`page-header.html`** — Carousel header used on non-homepage pages (shorter, no avatar overlay or social links, no controls)
- **`homepage-header.html`** — Full carousel with avatar, author name, description, social links, RSS; reads slides from `slidesDirPath` config

### Post Content
- **`post-header.html`** — Post title + metadata (categories, series, date, reading time, intro image)
- **`post-card.html`** — Linked post card for list views: title, date, reading time, optional intro image, summary, categories/series/tags links
- **`tags.html`** — Renders tag links for a post
- **`toc.html`** — Hierarchical table of contents (h1~h4) with Bootstrap Scrollspy, shown only when `toc: true` in frontmatter

### Sidebar Components
- **`sidebar-categories.html`** — Lists all categories with post counts
- **`sidebar-tags.html`** — Lists all tags with post counts
- **`sidebar-series.html`** — Shows top 5 series by post count; on homepage renders series cards (with data from `data/series.toml`), on other pages renders simple link list
- **`sidebar-card.html`** — Renders a single series recommendation card (image + title + summary)

### Other
- **`li.html`** — Simple list item for archive/blog listing pages (date + linked title)
- **`page-heading.html`** — Section page title heading
- **`footer.html`** — Page footer: social icons (same set as homepage), copyright line
- **`homepage-header.html`** (also listed above) — Full homepage hero carousel

## Shortcodes

- **`center`** — Wraps content in a centered `<div>` with `text-align: center`
- **`img-no-border`** — Renders an image without card-style borders (uses raw `<img>` tag)

## Post Frontmatter Fields

```yaml
---
title: "Post Title"
date: "2018-08-13T00:14:19+08:00"
publishdate: "2018-08-13+08:00"
lastmod: "2018-08-13+08:00"
draft: false
tags: ["css", "blog"]
series: ["Example"]
categories: ["Sci"]
img: "images/blog/2018-08/test5.jpg"    # optional intro picture shown on post card
toc: true                                # enables hierarchical TOC sidebar
summary: "Custom summary text"           # overrides Hugo's automatic summary
heading: "Custom <title> tag text"       # overrides .Title in <title>
---
```

**Summary** can be defined three ways (in priority order):
1. `summary` field in YAML frontmatter
2. `<!--more-->` summary divider in content (Hugo's manual split)
3. Hugo's automatic first-70-words extraction (controlled by `summaryLength` in config; `hasCJKLanguage: true` recommended for CJK content)

## Series Sidebar Cards

The sidebar series component looks up metadata from `data/series.toml`:

```toml
[[series]]
name = "example series"
img = "images/blog/2018-08/test3.jpg"
summary = "This is the simple summary of the series"
```

If a series is found in this data file, the card shows the custom image and summary. If not found, `static/img/default.jpg` is used as fallback. Series names are matched case-insensitively (lowercased via Hugo's `lower` function).

## Key Configuration

The theme is configured via `config.toml` params. See `exampleSite/config.toml` for a complete reference.

Important params:
- `slidesDirPath` / `slidesDirPathURL` — header carousel images (local filesystem path vs URL path; both required)
- `highlightjs` — toggle syntax highlighting (loads highlight.js pack + github-gist theme)
- `katex` — toggle LaTeX math rendering (loads KaTeX CSS + JS + auto-render)
- `latestpostscount` — number of posts on homepage
- `avatar` / `faviconfile` — site branding images
- `author` / `description` / `welcome_head` / `welcome_word` — homepage hero text
- `bloggroupby` — archive grouping: `"month"` (default) or `"year"`
- `dateform` / `dateformfull` — date display formats
- `noshowreadtime` — hides "X minutes read" when `true`
- `hasCJKLanguage` — set `true` for Chinese/Japanese/Korean content (affects Summary and WordCount)
- `summaryLength` — words in auto-summary (default Hugo: 70)
- Social links (`github`, `twitter`, `linkedin`, `facebook`, `googleplus`, `instagram`, `px500`, `email`) — shown in both homepage header and footer
- `include_rss` — include RSS link in `<head>` and show RSS icon
- `googleAnalytics` — Google Analytics tracking ID
- `forcehttps` — client-side HTTPS redirect script

## Common Commands

```sh
# Serve the example site locally with live reload
hugo server \
  --source exampleSite \
  --themesDir ../.. \
  --config exampleSite/config.toml \
  --buildDrafts

# Build the example site to exampleSite/public/
hugo \
  --source exampleSite \
  --themesDir ../.. \
  --config exampleSite/config.toml \
  --destination public

# Validate no template errors (dry build)
hugo --source exampleSite --themesDir ../.. --config exampleSite/config.toml
```

## Taxonomies

Three built-in taxonomies:
- **tags** — per-post topic labels (displayed inline on post cards and single pages)
- **series** — content series with sidebar recommendation cards (metadata in `data/series.toml`)
- **categories** — broad content categorization (shown with folder icon)

Configured in `config.toml`:
```toml
[taxonomies]
  tag      = "tags"
  series   = "series"
  category = "categories"
```

## Using This Theme in a Hugo Site

```sh
git clone https://github.com/orianna-zzo/AllinOne.git themes/AllinOne
```

Then set `theme = "AllinOne"` in your site's `config.toml`.
