# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static marketing website for WebScreen, a hackable, open-source secondary AMOLED display (ESP32-S3). It's a plain HTML/CSS/JavaScript site with no build process, deployed at https://webscreen.cc.

## Tech Stack

- **Frontend Only**: Pure HTML5, CSS3, JavaScript (jQuery)
- **Libraries** (loaded via CDN):
  - Bootstrap 4.5.2 (layout and components — CSS and JS versions must stay aligned)
  - jQuery 2.1.1 (DOM manipulation and event handling)
  - OwlCarousel2 (sliders and carousels; local copy in `assets/js/owl.carousel.min.js`)
  - AOS (Animate On Scroll library)
  - Font Awesome 6.5.2 (icons)
  - CodeMirror 5.65.2 (read-only code blocks on `setup.html`)
  - Google model-viewer (3D/AR model rendering in `3d_render/`)

## Running Locally

Since this is a static site, use a local server:
```bash
python3 -m http.server 8000
# or
http-server
```

## Project Structure

```
/
├── index.html          # Main landing page
├── setup.html          # Setup guide (flashing, config, serial commands, JS API highlights)
├── robots.txt
├── sitemap.xml         # Bump <lastmod> when editing pages
├── 3d_render/          # AR/3D model viewer (uses Google model-viewer)
│   ├── index.html
│   ├── script.js
│   ├── styles.css
│   └── *.glb           # 3D model files
├── assets/
│   ├── css/
│   │   ├── style.css       # Main styles
│   │   ├── responsive.css  # Responsive breakpoints
│   │   └── setup.css       # Setup-page styles
│   ├── js/
│   │   ├── main.js             # Main site functionality
│   │   └── owl.carousel.min.js # Vendored carousel lib
│   ├── images/
│   └── fonts/
```

## Key Architecture

### Pages and Sections
- `index.html` uses anchor links for single-page navigation: `#about`, `#feature`, `#marketplace`, `#faq`, `#contact`. It carries JSON-LD structured data (Product, Organization, WebSite) in the `<head>` — keep it factual (no fabricated ratings or nonexistent pages).
- `setup.html` has Web Flasher and Arduino IDE setup tabs, an Advanced Configuration section (config examples + serial command reference accordions + a "JavaScript API Highlights" subsection), Troubleshooting, and Next Steps. Code blocks (`.code-block`) are turned into read-only CodeMirror editors by inline JS.
- The serial command reference and JS API highlights on `setup.html` must track the firmware docs in the WebScreen-Software repo (`docs/SerialCommands.md`, `docs/API.md`).

### JavaScript Features (`assets/js/main.js`)
- Smooth scroll navigation with 80px offset for sticky header
- Multiple OwlCarousel instances (deal-slider, testimonial-slider, product-slider, twitte-slider)
- Back-to-top button, sticky nav on scroll, scrollspy, preloader fade-out
- AOS scroll animations initialized with `ease-out-back` easing

### External Integrations
- **Crowd Supply**: Primary sales channel (crowdsupply.com/hw-media-lab/webscreen)
- **Flash Tool**: flash.webscreen.cc for firmware flashing
- **Admin Tool**: admin.webscreen.cc for device configuration
- **Serial IDE**: serial.webscreen.cc for development
- **GitHub Org**: github.com/HW-Lab-Hardware-Design-Agency (firmware: WebScreen-Software; examples: WebScreen-Awesome)
- **Community**: Discord (discord.gg/vKT5b3skjF)

## Code Search

Use ken as the first attempt for codebase questions. Prefer ken MCP tools before
broad text search or reading many files:

- Start with `ken_rank` for the current task, or pass a query when the question
  needs a focused search.
- Use `ken_search_files` to find files by intent, feature, behavior, or concept.
- Use `ken_search_symbols` to find functions, classes, methods, APIs, and other
  named code objects.
- Use `ken_file_outline`, `ken_file_symbols`, and `ken_file_snippets` to inspect
  surfaced files precisely before opening larger chunks of code.
- Use `ken_file_neighbors`, `ken_module_graph`, and `ken_find_tests` to follow
  imports, related modules, and source/test pairs.
- Use `ken_changed_context` when working from an existing diff or local edits.
- Use `ken_project_overview` for a compact map of an unfamiliar project area.
- Use `ken_recall` and `ken_findings` for saved project knowledge, and
  `ken_remember` when a durable finding should help future sessions.
- Use `ken_explain_rank` when rankings look surprising or an expected file is
  missing.
- Use `ken_dismiss` when ken surfaces a file that is clearly not relevant, so
  future similar tasks get better results.

After ken narrows the search space, read the relevant files directly. Fall back
to `rg` when ken is insufficient, when an exact literal search is required, or
when verifying a specific string occurrence.
