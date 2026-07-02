# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

**AllinOne** is a Hugo theme for blogging and personal websites. It is used as a subdirectory (`themes/AllinOne/`) inside a Hugo site, not as a standalone project.

- **Author**: ZHENG Zi'ou (Orianna)
- **License**: MIT
- **Min Hugo version**: v0.46
- **Demo**: https://orianna-zzo.github.io/AllinOne-html/

## Key Features

- Full image carousel on homepage
- Hierarchical TOC with Scrollspy (h1~h4)
- Katex for LaTeX math rendering
- Syntax highlighting via highlight.js
- Google Analytics integration
- Font Awesome icons
- Tags, Series, and Categories taxonomies
- Post card list with summary and intro picture
- Previous/Next post navigation
- Pagination for sections
- CJK language support

## Project Structure

| Path | Description |
|------|-------------|
| `layouts/` | Hugo Go templates (baseof, single page, list pages, partials) |
| `layouts/_default/baseof.html` | Base HTML skeleton shared by all pages |
| `layouts/partials/` | Reusable template fragments (nav, footer, sidebar, head, etc.) |
| `layouts/_default/` | Core page types: `single.html` (post), `list.html` (archive), `taxonomy.html`/`terms.html` |
| `layouts/shortcodes/` | Custom Hugo shortcodes (`center`, `img-no-border`) |
| `static/` | Static assets served as-is: CSS, JS, fonts, images, vendor libraries |
| `static/js/vendors/` | Third-party JS (jQuery, Bootstrap 4, MDB, highlight.js, Katex, holder.js, popper) |
| `static/css/vendors/` | Third-party CSS (Bootstrap 4, MDB, highlight.js) |
| `static/img/` | Theme images (profile, header slides, SVG icons, masks) |
| `static/fonts/fontawesome/` | Font Awesome icon font files |
| `i18n/` | Translation files (`en.yaml`, `zh-Hans.yaml`) |
| `archetypes/` | Default content template (`default.md`) |
| `data/` | Site data files (`series.toml` for series metadata) |
| `exampleSite/` | Example Hugo site with sample content and configuration |
| `images/` | Theme screenshots (for README display) |

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

# Build the theme (validate no template errors)
hugo --source exampleSite --themesDir ../.. --config exampleSite/config.toml
```

## Using This Theme in a Hugo Site

Clone the theme into your Hugo site's `themes/` directory:

```sh
git clone https://github.com/orianna-zzo/AllinOne.git themes/AllinOne
```

Then set `theme = "AllinOne"` in your site's `config.toml`.

## Key Configuration

The theme is configured via `config.toml` params. See `exampleSite/config.toml` for a complete reference.

Important params:
- `slidesDirPath` / `slidesDirPathURL` — header carousel images
- `highlightjs` — toggle syntax highlighting
- `katex` — toggle LaTeX math rendering
- `latestpostscount` — number of posts on homepage
- `avatar` / `faviconfile` — site branding images
- Social links (github, twitter, etc.) — shown in footer

## Taxonomies

Three built-in taxonomies:
- **tags** — per-post topic labels
- **series** — content series with sidebar recommendation cards (metadata in `data/series.toml`)
- **categories** — broad content categorization
