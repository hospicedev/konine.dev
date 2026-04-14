# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the **Konine** website (konine.dev) — a Jekyll-based static site hosted on GitHub Pages. Konine is the successor to Koseven, which was the successor to Kohana. It is a PHP 8.4+ HMVC framework.

**Koseven was archived on April 14, 2026.** Konine exists as end-of-life support (maintenance and security fixes only). No new features. The goal is to keep the framework running until PHP 9 reaches end of life, or until the maintainers are no longer able to continue.

GitHub: https://github.com/hospicedev/konine.dev

## Build & Development

```bash
# Install dependencies (requires Ruby + Bundler)
bundle install

# Local development server
bundle exec jekyll serve

# Build site
bundle exec jekyll build
```

The site auto-deploys via GitHub Pages on push to `master`.

## Architecture

**Jekyll site structure:**
- `_config.yml` — Site config (title, baseurl, url)
- `_layouts/` — Two layouts: `api.html` (API class pages) and `documentation.html` (guide pages). Both load sidebar nav from a sibling `menu.md` via `{% include_relative menu.md %}`
- `_includes/` — Shared partials: `head.html`, `footer.html`, `breadcrumbs.html`, `promo.html`
- `_data/` — YAML data files driving dynamic content:
  - `modules.yml` — Third-party module directory (name, url, description, category)
  - `categories.yml` — Module categories: database, frontend, misc
  - `contributors.yml` / `showcases.yml`

**Content structure:**
- `documentation/` — All docs, organized by module (kohana, auth, cache, database, image, orm, etc.)
  - `documentation/kohana/` — Core framework docs (install, bootstrap, config, controllers, MVC, etc.)
  - `documentation/api/` — Auto-generated API class reference (one `.md` per class). These contain historical Koseven/Kohana copyright attributions which are correct and should not be changed.
  - Each doc section has its own `menu.md` for sidebar navigation
- `_tools/generatedocs.php` — PHP script to generate API documentation markdown from source

**Top-level pages** (HTML with Jekyll front matter):
- `index.html`, `start.html`, `help.html`, `documentation.html`, `modules.html`, `showcase.html`, `contributors.html`

**Assets:**
- `assets/css/styles.css` — Main theme CSS
- `assets/plugins/` — Bootstrap, Prism (syntax highlighting), jQuery plugins, elegant_font icons

## Key Conventions

- Documentation pages use `layout: documentation` or `layout: api` in front matter
- API docs use `class:` front matter field for the class name
- Sidebar navigation is per-section via `menu.md` files co-located with content
- Module entries in `_data/modules.yml` must use category: `database`, `frontend`, or `misc`
- Third-party module URLs in `_data/modules.yml` point to external repos (many still use "koseven" in their names — this is correct)
- The Kohana::init / KO7::init API calls in documentation reference the underlying framework internals — these class names are part of the framework's PHP API and should not be renamed
