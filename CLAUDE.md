# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static single-page portfolio/resume website for 임요섭 (Lim Yosup), a backend developer. It is deployed via GitHub Pages at `https://xub2.github.io`.

There is no build system, package manager, or framework — the entire site is a single `index.html` file with inline CSS and JavaScript.

## File Structure

```
index.html       — entire site (HTML + CSS + JS in one file)
resume.pdf       — downloadable resume
portfolio.pdf    — downloadable portfolio
assets/
  images/
    xuv.jpg              — profile photo
    thumbnail.png        — OG/Twitter card image
    blog/                — blog post preview thumbnails (288.png, 287.png, …)
    jabaclass/           — project screenshots used in the challenge/arch section
```

## Development

No build step is needed. To preview locally, open `index.html` directly in a browser or serve it with any static file server:

```bash
python3 -m http.server 8080
# or
npx serve .
```

## Architecture of index.html

The file is structured in order:

1. **`<head>`** — meta/OG tags, Google Fonts (`Bebas Neue`, `Noto Sans KR`, `JetBrains Mono`), and all CSS in a single `<style>` block.
2. **CSS custom properties** — defined on `:root` (light) and `[data-theme="dark"]` (dark). All colors reference these variables (`--navy`, `--surface`, `--text-muted`, etc.).
3. **HTML body sections** (in DOM order):
   - `<nav>` — fixed top bar with anchor links
   - `#skills` — 4-column skill cards grid
   - `#blog` — blog stats, story, and post preview cards (thumbnails pulled from `raw.githubusercontent.com`)
   - `#projects` — accordion-style project cards with `.project-challenges` sub-sections and architecture image sliders
   - `#about` — contact info terminal-style block + timeline (education/activities/awards) + certificate image slider
4. **Inline `<script>`** at the bottom handles: dark/light theme toggle (persisted in `localStorage`), `.top-btn` show/hide on scroll, lightbox for architecture images, and scroll-triggered `.fade-up` animations via `IntersectionObserver`.

## Key Conventions

- **Dark mode**: toggled by adding `data-theme="dark"` to `<html>`. Preference is saved to `localStorage` under the key `theme`.
- **Responsive breakpoints**: `768px` (tablet) and `480px` (mobile), both handled with `@media` blocks near the bottom of the `<style>` section.
- **Blog thumbnails**: stored as `assets/images/blog/<post-number>.png` and referenced via the raw GitHub URL. Add new ones by uploading to that folder and creating a new `.blog-post-card` entry.
- **Architecture / project screenshots**: stored under `assets/images/jabaclass/` and used inside `.challenge-arch-imgs` with the lightbox-enabled class.
- **Fonts**: `Bebas Neue` for large display headings, `JetBrains Mono` for labels/tags/code-style text, `Noto Sans KR` for body text.
- **Icons in skill tags**: pulled from `cdn.simpleicons.org` at render time. Each `<span class="tag">` optionally includes an `<img>` from that CDN.