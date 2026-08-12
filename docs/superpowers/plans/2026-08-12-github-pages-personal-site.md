# Personal GitHub Pages Site (kyrulamri.github.io) Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a beautiful single-page personal site for `kyrulamri` that follows Anthropic's official brand guidelines, ships as a static Jekyll site, publishes automatically at `https://kyrulamri.github.io/`, and lets the owner edit all content directly from the GitHub web UI.

**Architecture:** A zero-build Jekyll site on GitHub Pages. All user-editable content lives in two YAML data files (`_data/site.yml`, `_data/projects.yml`); a single `index.html` renders it with Liquid loops. GitHub Pages builds and publishes the site automatically on every push to `main` — no local toolchain is required to edit content. Design is a fully custom stylesheet (`assets/css/main.css`) implementing Anthropic's official brand palette and typography; one small vanilla-JS file handles mobile nav, scroll reveal, and the dynamic year. Theme is disabled (`theme: null` in `_config.yml`, `layout: null` in page front matter) so the default minima theme never wraps or styles our pages.

**Tech Stack:** Jekyll (as pinned by the `github-pages` gem), GitHub Pages (auto-build), Liquid templating, vanilla HTML/CSS/JS, Google Fonts (Poppins + Lora), Docker (`jekyll/jekyll:pages` image) for local verification.

## Global Constraints

> These are project-wide requirements. Every task below implicitly includes them.

- Publish target: `https://kyrulamri.github.io/`, from the **public** repo `kyrulamri.github.io`, default branch `main`, GitHub Pages source = main branch root.
- GitHub username (all URLs, remote, and JSON-LD): **`kyrulamri`**.
- Must be a **static site** that publishes to GitHub Pages with **no deploy step** — pushing to `main` is the deploy.
- Must be **easily editable directly from the GitHub repo**: all content in `_data/*.yml` and Markdown, editable in the github.com web UI; the editor never needs to touch HTML/CSS or run a build.
- Must comply with **Anthropic's official brand-guidelines skill** (anthropics/skills, `skills/brand-guidelines/SKILL.md`), adapted for the web:
  - Palette (verbatim): Dark `#141413`, Light `#faf9f5`, Mid Gray `#b0aea5`, Light Gray `#e8e6dc`; accents Orange `#d97757` (primary), Blue `#6a9bcc` (secondary), Green `#788c5d` (tertiary).
  - Typography (verbatim): Headings **Poppins** with **Arial** fallback; body **Lora** with **Georgia** fallback.
  - Accent colors cycle orange → blue → green across project cards (per the skill's "cycles through orange, blue, and green accents").
- Must be **beautiful**: editorial layout inspired by anthropic.com — warm cream canvas, generous whitespace, one restrained accent, rounded cards, subtle hairlines.
- **Accessibility:** WCAG AA text contrast (4.5:1), semantic landmarks, skip link, visible focus styles, `prefers-reduced-motion` support.
- Content defaults approved by the owner: placeholder about text and **3 sample projects** (replace-me content is expected).
- No CSS/JS frameworks, no additional gems/plugins, no external JS. Fonts come from Google Fonts (fallbacks Arial/Georgia must be declared).
- Placeholder-content rule: sample projects and about text are fine, but must be visibly marked "replace me" in the data files.
- Theme disabled: `_config.yml` sets `theme: null`; every page's front matter sets `layout: null`. This keeps minima (GitHub Pages' default theme) from wrapping our pages or copying its assets into `_site/`.
- **Local build command used by every task's tests** (Docker, Windows Git Bash — copy exactly):

```bash
MSYS_NO_PATHCONV=1 docker run --rm -v "$(cygpath -w "$PWD"):/srv/jekyll" jekyll/jekyll:pages jekyll build
```

  On macOS/Linux the plain form works: `MSYS_NO_PATHCONV=1 docker run --rm -v "$(cygpath -w "$PWD"):/srv/jekyll" jekyll/jekyll:pages jekyll build`. Add `-p 4000:4000` and replace `build` with `serve --livereload` for local preview.

---

## File Structure

```
kyrulamri.github.io/
├── index.html            # The whole page: header, hero, about, projects, contact, footer (Liquid)
├── _config.yml           # Site title, description, url, baseurl, exclude list
├── Gemfile               # Pins local builds to the github-pages gem
├── .gitignore            # Excludes _site/ and Jekyll caches from git
├── README.md             # "How to edit from GitHub" guide (Task 9)
├── 404.html              # Branded 404 page (Task 8)
├── _data/
│   ├── site.yml          # name, tagline, about (Markdown), socials — ALL site content the owner edits
│   └── projects.yml      # project cards: title, description, url, tags — the other editable file
├── assets/
│   ├── css/main.css      # Full Anthropic-inspired design system (tokens + components)
│   └── js/main.js        # Mobile nav toggle, scroll reveal, dynamic year
└── _site/                # Jekyll build output — gitignored, never committed
```

- `index.html` is the single template. It has front matter (so Jekyll processes it) but **no `layout:`** — it renders as-is with Liquid evaluated. We deliberately use no theme, so minima's CSS never loads.
- `_data/site.yml` and `_data/projects.yml` are the ONLY files the site owner edits day-to-day. README documents them.
- `assets/css/main.css` is the design system: design tokens at the top (brand colors/type as CSS variables), then component rules appended by later tasks.
- `assets/js/main.js` is the only script. Vanilla, dependency-free.

---

### Task 1: Project Scaffold and Jekyll Configuration

**Files:**
- Create: `.gitignore`
- Create: `Gemfile`
- Create: `_config.yml`
- Create: `index.html` (minimal skeleton with front matter — replaced/expanded in Tasks 3–6)
- Test: `_site/index.html` (build output)

**Interfaces:**
- Consumes: nothing (bootstrap task).
- Produces: the git repo; the build command used by every later task's tests; `site.title`, `site.description`, `site.url` (from `_config.yml`) used by `index.html` head in Tasks 3 and 8.

- [ ] **Step 1: Initialize the git repository and configure identity**

```bash
git init
git branch -M main
git config --get user.email || echo "NOTE: run the next two commands with your GitHub email/name if you want to commit locally"
# git config user.email "you@example.com"
# git config user.name "kyrulamri"
```

- [ ] **Step 2: Create `.gitignore`**

```gitignore
_site/
.sass-cache/
.jekyll-cache/
.jekyll-metadata
vendor/
Gemfile.lock
.DS_Store
```

- [ ] **Step 3: Create `Gemfile`**

```ruby
# Pins local builds to the exact Jekyll stack GitHub Pages uses.
source "https://rubygems.org"
gem "github-pages", group: :jekyll_plugins
```

- [ ] **Step 4: Create `_config.yml`**

```yaml
# Site configuration for kyrulamri.github.io
# GitHub Pages builds this site automatically on every push to `main`.
# NOTE: theme: null disables minima (GitHub Pages' default theme) so our
# custom design in assets/css/main.css is the only styling applied.

title: kyrulamri
description: Personal site of kyrulamri — software and design with an Anthropic-inspired aesthetic.
url: "https://kyrulamri.github.io"
baseurl: ""
theme: null

# Keep source/readme/plan files out of the published _site/
exclude:
  - README.md
  - Gemfile
  - Gemfile.lock
  - docs
  - vendor
```

- [ ] **Step 5: Create the minimal `index.html` skeleton**

```html
---
title: kyrulamri
layout: null
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>{{ site.title }}</title>
</head>
<body>
  <p>Hello, {{ site.title }}.</p>
</body>
</html>
```

- [ ] **Step 6: Run the build to verify the scaffold works (this task's test is build success — nothing exists to fail yet)**

Prerequisite: Docker (recommended) or Ruby ≥ 3.0. The `jekyll/jekyll:pages` image is built from GitHub's official `pages-gem`, so its output matches what GitHub Pages publishes. First run pulls the image (~400 MB); later runs are fast.

```bash
MSYS_NO_PATHCONV=1 docker run --rm -v "$(cygpath -w "$PWD"):/srv/jekyll" jekyll/jekyll:pages jekyll build
test -f _site/index.html \
  && echo "PASS: Jekyll build produces _site/index.html" \
  || { echo "FAIL: build did not produce _site/index.html"; exit 1; }
```

Expected: image pulls, build completes, `_site/index.html` exists and contains `Hello, kyrulamri.`

Notes:
- On Windows Git Bash, the `MSYS_NO_PATHCONV=1` prefix + `$(cygpath -w "$PWD")` form is REQUIRED — plain `$PWD` gets mangled into `C:/Program Files/Git/...` and the mount silently fails. On macOS/Linux use plain `"$PWD:/srv/jekyll"`. On Windows PowerShell use `"${PWD}:/srv/jekyll"`.
- No Docker? Use `bundle install && bundle exec jekyll build` (needs Ruby). All test commands in this plan assume the Docker form; substitute the `bundle exec` form if you use Ruby.
- `theme: null` disables minima entirely, so the build produces only our files — no default theme CSS, no layout wrapping. Every page front matter must set `layout: null` (this also prevents the `jekyll-seo-tag` "No repo name found" error that otherwise fires in local builds that lack the `PAGES_REPO_NWO` env var GitHub Pages sets in production).

- [ ] **Step 7: Commit**

```bash
git add .gitignore Gemfile _config.yml index.html
git commit -m "chore: scaffold Jekyll site for GitHub Pages"
```

---

### Task 2: Anthropic Design Tokens and Base Stylesheet

**Files:**
- Create: `assets/css/main.css` (tokens, reset, typography, layout, skip link, buttons)
- Test: `_site/assets/css/main.css` (built output)

**Interfaces:**
- Consumes: build command from Task 1.
- Produces: CSS custom properties used by every later task — `--dark`, `--light`, `--mid-gray`, `--light-gray`, `--accent-orange`, `--accent-blue`, `--accent-green`, `--accent-ink`, `--text`, `--text-muted`, `--canvas`, `--surface`, `--border`, `--font-heading`, `--font-body`, `--radius-sm/md/lg`, `--shadow-sm/md`, `--container`, `--gutter`, `--section-space`, `--ease`. Component classes `.container`, `.btn`, `.btn-primary`, `.btn-ghost`, `.skip-link` are consumed by Tasks 3–6. Later tasks **append** to this file.

- [ ] **Step 1: Write the failing test**

```bash
MSYS_NO_PATHCONV=1 docker run --rm -v "$(cygpath -w "$PWD"):/srv/jekyll" jekyll/jekyll:pages jekyll build
grep -q -- "--accent-orange: #d97757" _site/assets/css/main.css \
  && echo "PASS: brand accent token present" \
  || { echo "FAIL: accent token missing"; exit 1; }
```

- [ ] **Step 2: Run it to verify it fails**

Run the command above.
Expected: FAIL — `assets/css/main.css` doesn't exist yet, so `_site/assets/css/main.css` isn't found.

- [ ] **Step 3: Create `assets/css/main.css` with the full base system**

```css
/* ============================================================
   kyrulamri.github.io — Anthropic-inspired design system
   Palette & typography: anthropics/skills brand-guidelines
   ============================================================ */

/* ---------- Design tokens ---------- */
:root {
  /* Brand palette (verbatim from brand-guidelines SKILL.md) */
  --dark: #141413;          /* primary text + dark backgrounds */
  --light: #faf9f5;         /* light backgrounds + text on dark */
  --mid-gray: #b0aea5;      /* secondary elements */
  --light-gray: #e8e6dc;    /* subtle backgrounds */
  --accent-orange: #d97757; /* primary accent */
  --accent-blue: #6a9bcc;   /* secondary accent */
  --accent-green: #788c5d;  /* tertiary accent */

  /* Usage tokens (derived from brand palette) */
  --text: var(--dark);
  --text-muted: #5f5e57;        /* AA-compliant muted ink (4.5:1 on cream), derived from Mid Gray */
  --canvas: var(--light);
  --surface: var(--light-gray); /* card + well backgrounds */
  --border: #dcd9cd;            /* hairline borders on the cream canvas */
  --focus: var(--accent-orange);
  --accent-ink: #b04e2f;        /* AA-compliant accent for TEXT (brand Orange #d97757 is for fills) */

  /* Typography (brand-guidelines: Poppins headings / Lora body) */
  --font-heading: "Poppins", Arial, sans-serif;
  --font-body: "Lora", Georgia, serif;

  /* Radii + elevation */
  --radius-sm: 8px;
  --radius-md: 14px;
  --radius-lg: 24px;
  --shadow-sm: 0 1px 2px rgba(20, 20, 19, 0.06);
  --shadow-md: 0 6px 24px rgba(20, 20, 19, 0.09);

  /* Layout */
  --container: 72rem;
  --gutter: clamp(1.25rem, 4vw, 2.5rem);
  --section-space: clamp(4rem, 9vw, 7rem);

  /* Motion */
  --ease: cubic-bezier(0.22, 1, 0.36, 1);
}

/* ---------- Reset & base ---------- */
*, *::before, *::after { box-sizing: border-box; }

html { scroll-behavior: smooth; }

body {
  margin: 0;
  font-family: var(--font-body);
  font-size: 1.0625rem;
  line-height: 1.7;
  color: var(--text);
  background: var(--canvas);
  -webkit-font-smoothing: antialiased;
}

h1, h2, h3, h4 { font-family: var(--font-heading); font-weight: 600; line-height: 1.15; margin: 0; }
p { margin: 0; }
ul, ol { margin: 0; padding: 0; }
a { color: inherit; }
img { max-width: 100%; display: block; }

::selection { background: var(--accent-orange); color: var(--dark); }

:focus-visible { outline: 3px solid var(--focus); outline-offset: 3px; border-radius: 2px; }

.container {
  width: 100%;
  max-width: var(--container);
  margin-inline: auto;
  padding-inline: var(--gutter);
}

/* ---------- Skip link ---------- */
.skip-link {
  position: absolute;
  top: -100%;
  left: 1rem;
  z-index: 100;
  padding: 0.75rem 1.25rem;
  background: var(--dark);
  color: var(--light);
  border-radius: var(--radius-sm);
  text-decoration: none;
  font-family: var(--font-heading);
  font-size: 0.9rem;
}
.skip-link:focus { top: 1rem; }

/* ---------- Buttons ---------- */
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  padding: 0.85rem 1.6rem;
  border-radius: 999px;
  font-family: var(--font-heading);
  font-size: 0.95rem;
  font-weight: 600;
  text-decoration: none;
  cursor: pointer;
  transition: transform 0.2s var(--ease), box-shadow 0.2s var(--ease), background-color 0.2s var(--ease);
}
.btn:hover { transform: translateY(-2px); }
.btn-primary { background: var(--accent-orange); color: var(--dark); box-shadow: var(--shadow-sm); }
.btn-primary:hover { box-shadow: var(--shadow-md); }
.btn-ghost { background: transparent; color: var(--text); border: 1.5px solid var(--border); }
.btn-ghost:hover { background: var(--light-gray); border-color: transparent; }
```

- [ ] **Step 4: Run the test to verify it passes**

Run the Step 1 command.
Expected: PASS — `--accent-orange: #d97757` found in `_site/assets/css/main.css`.

- [ ] **Step 5: Commit**

```bash
git add assets/css/main.css
git commit -m "feat: add Anthropic-inspired design tokens and base styles"
```

---

### Task 3: Header, Navigation, and Hero

**Files:**
- Create: `_data/site.yml`
- Modify: `index.html` (replace the whole body with the full page skeleton below)
- Modify: `assets/css/main.css` (append header/nav/hero styles)
- Test: `_site/index.html`

**Interfaces:**
- Consumes: `.container`, `.btn`, `.btn-primary`, `.btn-ghost` (Task 2); `site.title`, `site.description` (Task 1).
- Produces: `_data/site.yml` keys used by Tasks 4–8 — `site.data.site.name`, `.tagline`, `.about`, `.socials` (each social: `label`, `url`). HTML anchors `#top`, `#about`, `#projects`, `#contact`; classes `.site-header`, `.nav`, `.brand`, `.nav-toggle`, `.nav-menu`, `.hero`, `.eyebrow`, `.hero-name`, `.hero-tagline`, `.hero-actions`. Marker comments `<!-- ABOUT_SECTION -->`, `<!-- PROJECTS_SECTION -->`, `<!-- CONTACT_SECTION -->` are replaced one-for-one in Tasks 4, 5, and 6.

- [ ] **Step 1: Write the failing test**

```bash
MSYS_NO_PATHCONV=1 docker run --rm -v "$(cygpath -w "$PWD"):/srv/jekyll" jekyll/jekyll:pages jekyll build
grep -q "Software and design with an Anthropic-inspired aesthetic" _site/index.html \
  && echo "PASS: hero tagline rendered" \
  || { echo "FAIL: hero tagline missing"; exit 1; }
```

- [ ] **Step 2: Run it to verify it fails**

Expected: FAIL — no hero exists yet; `_data/site.yml` doesn't exist.

- [ ] **Step 3: Create `_data/site.yml`**

```yaml
# ── EDIT ME from the GitHub web UI ─────────────────────────────
# This file controls the site's identity, hero, about, and contact.
# See README.md for how to edit safely.

name: kyrulamri
tagline: "Software and design with an Anthropic-inspired aesthetic."
about: |
  I'm **kyrulamri** — a developer who cares about clean design and
  durable software. This page follows [Anthropic's brand
  guidelines](https://github.com/anthropics/skills/tree/main/skills/brand-guidelines):
  warm neutrals, one restrained accent, and type that reads like a
  book.

  *Replace this paragraph with a short description of what you do,
  what you're building, or what you care about. Plain Markdown works
  here — links, bold, and lists are all fine.*
socials:
  - label: GitHub
    url: "https://github.com/kyrulamri"
  - label: LinkedIn
    url: "https://www.linkedin.com/in/your-handle"
  - label: Email
    url: "mailto:you@example.com"
```

- [ ] **Step 4: Replace the body of `index.html` with the full page skeleton**

Keep the existing front matter (`title` + `layout: null`, per Task 1) and `<head>`, then replace everything from `<body>` down with:

```html
<body>
  <a class="skip-link" href="#main">Skip to content</a>

  <header class="site-header">
    <nav class="nav container" aria-label="Primary">
      <a class="brand" href="#top">{{ site.data.site.name }}</a>
      <button class="nav-toggle" type="button" aria-expanded="false" aria-controls="nav-menu">Menu</button>
      <ul class="nav-menu" id="nav-menu">
        <li><a href="#about">About</a></li>
        <li><a href="#projects">Projects</a></li>
        <li><a href="#contact">Contact</a></li>
      </ul>
    </nav>
  </header>

  <main id="main">
    <div class="container">
      <section class="hero" id="top">
        <p class="eyebrow">Hi, I'm</p>
        <h1 class="hero-name">{{ site.data.site.name }}</h1>
        <p class="hero-tagline">{{ site.data.site.tagline }}</p>
        <div class="hero-actions">
          <a class="btn btn-primary" href="#projects">See my work</a>
          <a class="btn btn-ghost" href="#contact">Get in touch</a>
        </div>
      </section>

      <!-- ABOUT_SECTION -->

      <!-- PROJECTS_SECTION -->

      <!-- CONTACT_SECTION -->
    </div>
  </main>

  <footer class="site-footer">
    <div class="container">
      <p>© <span id="year"></span> {{ site.data.site.name }}</p>
      <p>An Anthropic-inspired design</p>
    </div>
  </footer>

  <script src="{{ '/assets/js/main.js' | relative_url }}"></script>
</body>
</html>
```

- [ ] **Step 5: Append the header/nav and hero styles to `assets/css/main.css`**

```css
/* ---------- Header & nav ---------- */
.site-header {
  position: sticky;
  top: 0;
  z-index: 50;
  background: rgba(250, 249, 245, 0.85);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border-bottom: 1px solid var(--border);
}
.nav { display: flex; align-items: center; justify-content: space-between; height: 4rem; }
.brand { font-family: var(--font-heading); font-size: 1.05rem; font-weight: 600; letter-spacing: -0.01em; text-decoration: none; }
.nav-menu { display: flex; align-items: center; gap: 2rem; list-style: none; }
.nav-menu a {
  font-family: var(--font-heading);
  font-size: 0.9rem;
  font-weight: 500;
  text-decoration: none;
  color: var(--text-muted);
  transition: color 0.2s var(--ease);
}
.nav-menu a:hover { color: var(--text); }
.nav-toggle { display: none; }

/* ---------- Hero ---------- */
.hero { padding: clamp(5rem, 12vw, 8.5rem) 0 clamp(3rem, 8vw, 5rem); max-width: 46rem; }
.eyebrow {
  font-family: var(--font-heading);
  font-size: 0.8rem;
  font-weight: 600;
  letter-spacing: 0.16em;
  text-transform: uppercase;
  color: var(--accent-ink);
  margin-bottom: 1.25rem;
}
.hero-name { font-size: clamp(2.75rem, 7vw, 4.75rem); font-weight: 600; letter-spacing: -0.03em; }
.hero-tagline { margin-top: 1.5rem; font-size: clamp(1.2rem, 2.6vw, 1.6rem); line-height: 1.5; color: var(--text-muted); }
.hero-actions { margin-top: 2.5rem; display: flex; flex-wrap: wrap; gap: 1rem; }

/* Entrance animation */
.hero > * { animation: rise 0.8s var(--ease) both; }
.hero > *:nth-child(2) { animation-delay: 0.08s; }
.hero > *:nth-child(3) { animation-delay: 0.16s; }
.hero > *:nth-child(4) { animation-delay: 0.24s; }
@keyframes rise {
  from { opacity: 0; transform: translateY(14px); }
  to   { opacity: 1; transform: none; }
}

/* ---------- Responsive: header/nav ---------- */
@media (max-width: 720px) {
  .nav-toggle {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.5rem 0.9rem;
    background: var(--canvas);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    font-family: var(--font-heading);
    font-size: 0.85rem;
    color: var(--text);
    cursor: pointer;
  }
  .nav-menu {
    position: absolute;
    top: 4rem;
    left: 0;
    right: 0;
    display: none;
    flex-direction: column;
    align-items: stretch;
    gap: 0;
    padding: 0.5rem var(--gutter) 1.25rem;
    background: var(--canvas);
    border-bottom: 1px solid var(--border);
  }
  .nav-menu.is-open { display: flex; }
  .nav-menu a { display: block; padding: 0.9rem 0; font-size: 1rem; }
  .nav-menu li + li { border-top: 1px solid var(--border); }
}
```

- [ ] **Step 6: Run the test to verify it passes**

Run the Step 1 command.
Expected: PASS — tagline text rendered into `_site/index.html`.

- [ ] **Step 7: Visual check**

```bash
MSYS_NO_PATHCONV=1 docker run --rm -v "$(cygpath -w "$PWD"):/srv/jekyll" -p 4000:4000 jekyll/jekyll:pages jekyll serve --livereload
```

Open `http://localhost:4000`. Verify: cream background `#faf9f5`; hero name renders in Poppins; tagline in Lora muted gray; orange "See my work" pill button; ghost button with hairline border; sticky header with blur on scroll; entrance animation on load. Stop the server with Ctrl+C.

- [ ] **Step 8: Commit**

```bash
git add _data/site.yml index.html assets/css/main.css
git commit -m "feat: add header nav and hero section"
```

---

### Task 4: About Section

**Files:**
- Modify: `index.html` (replace the `<!-- ABOUT_SECTION -->` marker)
- Modify: `assets/css/main.css` (append section + prose styles)
- Test: `_site/index.html`

**Interfaces:**
- Consumes: `site.data.site.about` (a Markdown string, from Task 3); `.reveal` class (styling arrives in Task 7 — harmless before then, no rule matches).
- Produces: anchor `#about`; classes `.section`, `.section-title`, `.prose` (consumed visually by Task 5's section title styling via `.section-title`).

- [ ] **Step 1: Write the failing test**

```bash
MSYS_NO_PATHCONV=1 docker run --rm -v "$(cygpath -w "$PWD"):/srv/jekyll" jekyll/jekyll:pages jekyll build
grep -q 'id="about"' _site/index.html \
  && echo "PASS: about section present" \
  || { echo "FAIL: about section missing"; exit 1; }
grep -q "clean design and" _site/index.html \
  && echo "PASS: about content rendered" \
  || { echo "FAIL: about content missing"; exit 1; }
```

- [ ] **Step 2: Run it to verify it fails**

Expected: FAIL — the About section marker is still an HTML comment.

- [ ] **Step 3: In `index.html`, replace the exact line `<!-- ABOUT_SECTION -->` with:**

```html
      <section class="section reveal" id="about">
        <h2 class="section-title">About</h2>
        <div class="prose">{{ site.data.site.about | markdownify }}</div>
      </section>
```

- [ ] **Step 4: Append the section and prose styles to `assets/css/main.css`**

```css
/* ---------- Sections ---------- */
.section { padding-block: var(--section-space); border-top: 1px solid var(--border); }
.section-title {
  display: flex;
  align-items: baseline;
  gap: 1rem;
  font-size: clamp(1.5rem, 3vw, 2rem);
  letter-spacing: -0.02em;
}
.section-title::after { content: ""; flex: 1; height: 1px; background: var(--border); }

/* ---------- Prose (rendered Markdown) ---------- */
.prose { max-width: 40rem; margin-top: 2rem; }
.prose p + p { margin-top: 1.1rem; }
.prose a { color: var(--accent-ink); text-decoration: underline; text-decoration-color: rgba(176, 78, 47, 0.4); text-underline-offset: 3px; }
.prose a:hover { text-decoration-color: var(--accent-ink); }
.prose strong { font-weight: 600; }
```

- [ ] **Step 5: Run the test to verify it passes**

Run the Step 1 command.
Expected: PASS on both greps — the `markdownify` filter turned the `about:` Markdown into HTML (`<p>I'm <strong>kyrulamri</strong> …`).

- [ ] **Step 6: Visual check**

`jekyll serve` (same command as Task 3) and verify: "About" title with hairline rule extending right; body text at 40rem measure, justified left; the Markdown link to Anthropic's guidelines renders in dark terracotta with an underline. Stop the server.

- [ ] **Step 7: Commit**

```bash
git add index.html assets/css/main.css
git commit -m "feat: add about section with editable content"
```

---

### Task 5: Projects Section

**Files:**
- Create: `_data/projects.yml`
- Modify: `index.html` (replace the `<!-- PROJECTS_SECTION -->` marker)
- Modify: `assets/css/main.css` (append project card + tag styles)
- Test: `_site/index.html`

**Interfaces:**
- Consumes: `site.data.projects` — each item has `title` (string), `description` (string), `url` (string), `tags` (list of strings). Shape is defined by this task and relied on by the README (Task 9).
- Produces: anchor `#projects`; classes `.project-grid`, `.project-card`, `.project-card-top`, `.project-dot`, `.project-title`, `.project-description`, `.tag-list`, `.tag`, `.accent-orange`, `.accent-blue`, `.accent-green`; Liquid variable `accent_var` (one of `orange|blue|green`).

- [ ] **Step 1: Write the failing test**

```bash
MSYS_NO_PATHCONV=1 docker run --rm -v "$(cygpath -w "$PWD"):/srv/jekyll" jekyll/jekyll:pages jekyll build
grep -q "Sample Project One" _site/index.html \
  && echo "PASS: project one rendered" \
  || { echo "FAIL: project one missing"; exit 1; }
grep -q "Sample Project Three" _site/index.html \
  && echo "PASS: project three rendered" \
  || { echo "FAIL: project three missing"; exit 1; }
```

- [ ] **Step 2: Run it to verify it fails**

Expected: FAIL — no projects data or markup yet.

- [ ] **Step 3: Create `_data/projects.yml`**

```yaml
# ── EDIT ME from the GitHub web UI ─────────────────────────────
# Each item below becomes one card in the Projects section.
# Replace the three samples with your own projects.
# Rules: 2-space indent, quote values containing spaces, https:// URLs.

- title: "Sample Project One"
  description: "One or two sentences: what it does, the problem it solves, and what you learned. Replace me."
  url: "https://github.com/kyrulamri"
  tags: ["TypeScript", "Web"]

- title: "Sample Project Two"
  description: "One or two sentences about this project. Replace me."
  url: "https://github.com/kyrulamri"
  tags: ["Python", "CLI"]

- title: "Sample Project Three"
  description: "One or two sentences about this project. Replace me."
  url: "https://github.com/kyrulamri"
  tags: ["Design", "Accessibility"]
```

- [ ] **Step 4: In `index.html`, replace the exact line `<!-- PROJECTS_SECTION -->` with:**

```html
      <section class="section reveal" id="projects">
        <h2 class="section-title">Projects</h2>
        <div class="project-grid">
          {% for project in site.data.projects %}
            {% assign accent_key = forloop.index0 | modulo: 3 %}
            {% case accent_key %}
              {% when 0 %}{% assign accent_var = "orange" %}
              {% when 1 %}{% assign accent_var = "blue" %}
              {% else %}{% assign accent_var = "green" %}
            {% endcase %}
            <a class="project-card reveal accent-{{ accent_var }}"
               style="transition-delay: {{ forloop.index0 | times: 60 }}ms"
               href="{{ project.url }}" target="_blank" rel="noopener">
              <div class="project-card-top">
                <span class="project-dot" aria-hidden="true"></span>
                <h3 class="project-title">{{ project.title }}</h3>
              </div>
              <p class="project-description">{{ project.description }}</p>
              <ul class="tag-list">
                {% for tag in project.tags %}<li class="tag">{{ tag }}</li>{% endfor %}
              </ul>
            </a>
          {% endfor %}
        </div>
      </section>
```

- [ ] **Step 5: Append the projects styles to `assets/css/main.css`**

```css
/* ---------- Projects ---------- */
.project-grid { margin-top: 2.5rem; display: grid; grid-template-columns: repeat(auto-fill, minmax(17rem, 1fr)); gap: 1.5rem; }
.project-card {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  padding: 1.75rem;
  background: var(--surface);
  border: 1px solid transparent;
  border-radius: var(--radius-lg);
  text-decoration: none;
  box-shadow: var(--shadow-sm);
  transition: transform 0.25s var(--ease), box-shadow 0.25s var(--ease), border-color 0.25s var(--ease), opacity 0.6s var(--ease);
}
.project-card:hover { transform: translateY(-4px); box-shadow: var(--shadow-md); border-color: var(--border); }
.project-card-top { display: flex; align-items: center; gap: 0.75rem; }
.project-dot { width: 10px; height: 10px; border-radius: 50%; background: var(--card-accent, var(--accent-orange)); flex: none; }
.project-title { font-size: 1.15rem; letter-spacing: -0.01em; }
.project-description { margin: 0; color: var(--text-muted); font-size: 0.98rem; flex: 1; }
.tag-list { display: flex; flex-wrap: wrap; gap: 0.5rem; list-style: none; }
.tag {
  font-family: var(--font-heading);
  font-size: 0.72rem;
  font-weight: 500;
  letter-spacing: 0.02em;
  text-transform: uppercase;
  padding: 0.3rem 0.7rem;
  border: 1px solid var(--border);
  border-radius: 999px;
  background: var(--canvas);
  color: var(--text);
}
/* Tag accent is a tinted fill (decorative), so tag text stays AA-compliant */
.tag {
  background: color-mix(in srgb, var(--tag-accent, var(--accent-orange)) 15%, var(--canvas));
}
/* Accent cycling — Liquid assigns one class per card */
.accent-orange { --card-accent: var(--accent-orange); --tag-accent: var(--accent-orange); }
.accent-blue   { --card-accent: var(--accent-blue);   --tag-accent: var(--accent-blue); }
.accent-green  { --card-accent: var(--accent-green);  --tag-accent: var(--accent-green); }
```

- [ ] **Step 6: Run the test to verify it passes**

Run the Step 1 command.
Expected: PASS — three project cards rendered from `_data/projects.yml`.

- [ ] **Step 7: Visual check**

`jekyll serve` and verify: cards on light-gray wells with 24px rounded corners; dot and tag tints cycle orange → blue → green; hover lifts the card with a soft shadow; grid reflows to one column on a narrow window. Stop the server.

- [ ] **Step 8: Commit**

```bash
git add _data/projects.yml index.html assets/css/main.css
git commit -m "feat: add projects grid driven by _data/projects.yml"
```

---

### Task 6: Contact Section and Footer Styles

**Files:**
- Modify: `index.html` (replace the `<!-- CONTACT_SECTION -->` marker)
- Modify: `assets/css/main.css` (append contact + footer styles)
- Test: `_site/index.html`

**Interfaces:**
- Consumes: `site.data.site.socials` — each item has `label` (string) and `url` (string, https:// or mailto:) from Task 3.
- Produces: anchor `#contact`; classes `.contact-blurb`, `.contact-links`. The footer markup already exists (Task 3 skeleton); this task adds its styles and relies on `#year` being filled by JS in Task 7.

- [ ] **Step 1: Write the failing test**

```bash
MSYS_NO_PATHCONV=1 docker run --rm -v "$(cygpath -w "$PWD"):/srv/jekyll" jekyll/jekyll:pages jekyll build
grep -q 'id="contact"' _site/index.html \
  && echo "PASS: contact section present" \
  || { echo "FAIL: contact section missing"; exit 1; }
```

- [ ] **Step 2: Run it to verify it fails**

Expected: FAIL — the contact marker is still an HTML comment.

- [ ] **Step 3: In `index.html`, replace the exact line `<!-- CONTACT_SECTION -->` with:**

```html
      <section class="section reveal" id="contact">
        <h2 class="section-title">Contact</h2>
        <p class="contact-blurb">
          Thanks for stopping by — I'm always up for a conversation about
          software, design, or anything in between. Reach me anywhere below.
        </p>
        <div class="contact-links">
          {% for social in site.data.site.socials %}
            <a class="btn btn-ghost" href="{{ social.url }}">{{ social.label }}</a>
          {% endfor %}
        </div>
      </section>
```

- [ ] **Step 4: Append the contact and footer styles to `assets/css/main.css`**

```css
/* ---------- Contact ---------- */
.contact-blurb { max-width: 34rem; margin-top: 1.25rem; color: var(--text-muted); }
.contact-links { margin-top: 2rem; display: flex; flex-wrap: wrap; gap: 1rem; }

/* ---------- Footer ---------- */
.site-footer { padding: 2.25rem 0 3rem; border-top: 1px solid var(--border); color: var(--text-muted); font-size: 0.9rem; }
.site-footer .container { display: flex; flex-wrap: wrap; justify-content: space-between; gap: 0.5rem; }
```

- [ ] **Step 5: Run the test to verify it passes**

Run the Step 1 command.
Expected: PASS.

- [ ] **Step 6: Visual check**

`jekyll serve` and verify: three ghost buttons (GitHub, LinkedIn, Email) wrap nicely; footer is muted with a top hairline and sits at the page bottom. Stop the server.

- [ ] **Step 7: Commit**

```bash
git add index.html assets/css/main.css
git commit -m "feat: add contact section and footer"
```

---

### Task 7: Interactivity — Mobile Nav, Scroll Reveal, Dynamic Year

**Files:**
- Create: `assets/js/main.js`
- Modify: `assets/css/main.css` (append reveal + reduced-motion styles)
- Test: `_site/assets/js/main.js` (build output), plus a browser check

**Interfaces:**
- Consumes: DOM hooks from Tasks 3–6 — `.nav-toggle`, `.nav-menu`, `#year`, `.reveal`.
- Produces: `is-visible` class (added to `.reveal` elements when scrolled into view); `aria-expanded` toggling on `.nav-toggle`; `#year` text set to the current year. Reduced-motion media query that disables the hero entrance and reveal transitions.

- [ ] **Step 1: Write the failing test**

```bash
MSYS_NO_PATHCONV=1 docker run --rm -v "$(cygpath -w "$PWD"):/srv/jekyll" jekyll/jekyll:pages jekyll build
test -f _site/assets/js/main.js \
  && echo "PASS: main.js is built and referenced" \
  || { echo "FAIL: main.js missing"; exit 1; }
```

- [ ] **Step 2: Run it to verify it fails**

Expected: FAIL — `assets/js/main.js` doesn't exist.

- [ ] **Step 3: Create `assets/js/main.js`**

```js
(function () {
  'use strict';

  // Mobile nav toggle
  var toggle = document.querySelector('.nav-toggle');
  var menu = document.querySelector('.nav-menu');
  if (toggle && menu) {
    toggle.addEventListener('click', function () {
      var open = menu.classList.toggle('is-open');
      toggle.setAttribute('aria-expanded', String(open));
      toggle.textContent = open ? 'Close' : 'Menu';
    });
    menu.querySelectorAll('a').forEach(function (link) {
      link.addEventListener('click', function () {
        menu.classList.remove('is-open');
        toggle.setAttribute('aria-expanded', 'false');
        toggle.textContent = 'Menu';
      });
    });
  }

  // Current year in the footer
  var year = document.getElementById('year');
  if (year) {
    year.textContent = String(new Date().getFullYear());
  }

  // Scroll reveal
  var revealEls = document.querySelectorAll('.reveal');
  if ('IntersectionObserver' in window) {
    var observer = new IntersectionObserver(function (entries) {
      entries.forEach(function (entry) {
        if (entry.isIntersecting) {
          entry.target.classList.add('is-visible');
          observer.unobserve(entry.target);
        }
      });
    }, { threshold: 0.12 });
    revealEls.forEach(function (el) { observer.observe(el); });
  } else {
    revealEls.forEach(function (el) { el.classList.add('is-visible'); });
  }
})();
```

- [ ] **Step 4: Append the reveal and reduced-motion styles to `assets/css/main.css`**

```css
/* ---------- Scroll reveal ---------- */
.reveal { opacity: 0; transform: translateY(16px); transition: opacity 0.6s var(--ease), transform 0.6s var(--ease); }
.reveal.is-visible { opacity: 1; transform: none; }

/* ---------- Reduced motion ---------- */
@media (prefers-reduced-motion: reduce) {
  html { scroll-behavior: auto; }
  .hero > * { animation: none; }
  .reveal { opacity: 1; transform: none; transition: none; }
  .btn:hover, .project-card:hover { transform: none; }
}
```

- [ ] **Step 5: Run the test to verify it passes**

Run the Step 1 command.
Expected: PASS — `main.js` copied into `_site/assets/js/` and already referenced by `index.html` from Task 3.

- [ ] **Step 6: Browser checks**

`jekyll serve`, then:
1. Desktop: scroll slowly — About, Projects, and Contact sections fade up as they enter the viewport; project cards stagger in.
2. Narrow the window below 720px: the "Menu" button appears; clicking it opens the dropdown and sets `aria-expanded="true"`; clicking a link closes it.
3. Footer year shows the current year (e.g., 2026).
4. Turn on OS-level "reduce motion": sections appear instantly, no animations.
Stop the server.

- [ ] **Step 7: Commit**

```bash
git add assets/js/main.js assets/css/main.css
git commit -m "feat: add mobile nav, scroll reveal, and dynamic year"
```

---

### Task 8: Meta Tags, Favicon, 404 Page, and Accessibility Pass

**Files:**
- Modify: `index.html` (head: add social meta, favicon, JSON-LD)
- Create: `404.html`
- Test: `_site/index.html`, `_site/404.html`

**Interfaces:**
- Consumes: `site.title`, `site.description`, `site.url` (Task 1); `site.data.site.name` (Task 3).
- Produces: Open Graph/Twitter meta (consumed by link previews), inline SVG favicon, JSON-LD `Person` block, branded 404 page served by GitHub Pages for unknown paths.

- [ ] **Step 1: Write the failing test**

```bash
MSYS_NO_PATHCONV=1 docker run --rm -v "$(cygpath -w "$PWD"):/srv/jekyll" jekyll/jekyll:pages jekyll build
grep -q 'property="og:title"' _site/index.html \
  && echo "PASS: Open Graph meta present" \
  || { echo "FAIL: Open Graph meta missing"; exit 1; }
grep -q '<link rel="icon"' _site/index.html \
  && echo "PASS: favicon present" \
  || { echo "FAIL: favicon missing"; exit 1; }
test -f _site/404.html \
  && echo "PASS: 404 page built" \
  || { echo "FAIL: 404 page missing"; exit 1; }
```

- [ ] **Step 2: Run it to verify it fails**

Expected: FAIL on all three — none exist yet.

- [ ] **Step 3: In the `<head>` of `index.html`, replace the `<meta name="description" ...>` line with:**

```html
  <meta name="description" content="{{ site.description }}">
  <meta property="og:type" content="website">
  <meta property="og:title" content="{{ site.title }}">
  <meta property="og:description" content="{{ site.description }}">
  <meta property="og:url" content="{{ site.url }}/">
  <meta name="twitter:card" content="summary">
  <link rel="icon" href='data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><rect width="100" height="100" rx="24" fill="%23d97757"/><text x="50" y="68" font-size="56" text-anchor="middle" fill="%23141413" font-family="Georgia,serif" font-weight="bold">k</text></svg>'>
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "Person",
    "name": "{{ site.data.site.name }}",
    "url": "{{ site.url }}/"
  }
  </script>
```

- [ ] **Step 4: Create `404.html`**

```html
---
permalink: /404.html
layout: null
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>404 — Page not found · {{ site.title }}</title>
  <link rel="stylesheet" href="{{ '/assets/css/main.css' | relative_url }}">
</head>
<body>
  <main class="container" style="padding-block: 6rem;">
    <p class="eyebrow">404</p>
    <h1 class="hero-name">Page not found</h1>
    <p class="hero-tagline">The page you're looking for doesn't exist or has moved.</p>
    <div class="hero-actions">
      <a class="btn btn-primary" href="/">Back home</a>
    </div>
  </main>
</body>
</html>
```

- [ ] **Step 5: Run the test to verify it passes**

Run the Step 1 command.
Expected: PASS on all three checks.

- [ ] **Step 6: Accessibility audit (manual, browser open on the served site)**

1. Tab through the page — the skip link appears first ("Skip to content"), every link/button gets a visible orange focus ring.
2. Keyboard-navigate the mobile menu — it opens and closes with Enter/Space.
3. Check landmarks in DevTools → Elements: exactly one `<header>`, one `<main>`, one `<footer>`, one `<nav aria-label="Primary">`.
4. Heading order: one `h1` (hero name), then `h2` section titles, then `h3` project titles.
5. DevTools → Lighthouse → Accessibility → Run: aim for a score of 100 (all checks above already satisfy the common failures).
6. Contrast spot-check: `--text-muted` (#5f5e57) on cream (#faf9f5) ≈ 6.1:1; `--accent-ink` (#b04e2f) on cream ≈ 5.3:1 — both pass AA. The brand Orange (#d97757) is used only for fills/decoration (buttons carry dark text, dots/tags are tinted fills).

- [ ] **Step 7: Commit**

```bash
git add index.html 404.html
git commit -m "feat: add meta tags, favicon, 404 page, and a11y polish"
```

---

### Task 9: README — The Owner's Editing Guide

**Files:**
- Create: `README.md`
- Test: `README.md` (file presence; content is the deliverable)

**Interfaces:**
- Consumes: the exact `_data` shapes from Tasks 3 and 5 (`site.yml` keys, `projects.yml` item shape); the build/test commands established in Tasks 1–2.
- Produces: the document the site owner reads to edit the site from GitHub.

- [ ] **Step 1: Write the failing test**

```bash
test -f README.md && echo "PASS: README exists" || { echo "FAIL: README missing"; exit 1; }
```

- [ ] **Step 2: Run it to verify it fails**

Expected: FAIL — README.md doesn't exist yet.

- [ ] **Step 3: Create `README.md`**

```markdown
# kyrulamri.github.io

Personal site of **kyrulamri**, built with Jekyll and an
Anthropic-inspired design system. Published automatically to
[https://kyrulamri.github.io](https://kyrulamri.github.io) by GitHub
Pages on every push to `main`.

## Editing from the GitHub web UI (no local setup needed)

All content lives in two YAML files. Edit them on github.com, click
**Commit changes**, and the site rebuilds in about a minute.

| What you want to change                    | File                |
| ------------------------------------------ | ------------------- |
| Name, tagline, about text, social links    | `_data/site.yml`    |
| Project cards (title, blurb, URL, tags)    | `_data/projects.yml`|
| Browser-tab title & description            | `index.html` (front matter + `<title>`) |

### Editing `_data/site.yml`

- `name` — your name as shown in the header and hero.
- `tagline` — the one-line description under your name.
- `about` — a longer bio; **Markdown is allowed** (bold, links, lists).
- `socials` — one block per link: `label` (shown on the button) and
  `url` (must start with `https://` or `mailto:`).

### Editing `_data/projects.yml` — adding a project

Open the file, copy the most recent block, and edit the four fields:

```yaml
- title: "My New Project"
  description: "One or two sentences about it."
  url: "https://github.com/kyrulamri/my-project"
  tags: ["Web", "Go"]
```

Rules that keep the build green:

- Indentation is significant: list items start with `- ` and their
  fields are indented by exactly two spaces.
- Put values containing spaces inside double quotes (`"..."`).
- URLs must start with `https://`.
- A broken YAML file fails the build — GitHub Pages shows the error on
  the repo's **Settings → Pages** tab, so it's easy to find and fix.

### Changing the design

The palette and typography are defined once, as CSS variables at the
top of `assets/css/main.css` — Anthropic brand colors (`#faf9f5`
canvas, `#141413` ink, `#d97757` accent, Poppins/Lora type). Change
the *tokens*, not the components.

## Previewing locally (optional)

With Docker (the `MSYS_NO_PATHCONV`/`cygpath` form is for Windows Git Bash; on macOS/Linux use `docker run --rm -v "$PWD:/srv/jekyll" -p 4000:4000 ...`):

```bash
MSYS_NO_PATHCONV=1 docker run --rm -v "$(cygpath -w "$PWD"):/srv/jekyll" -p 4000:4000 jekyll/jekyll:pages jekyll serve --livereload
# open http://localhost:4000
```

With Ruby instead:

```bash
bundle install
bundle exec jekyll serve
```

## Verifying the build

```bash
MSYS_NO_PATHCONV=1 docker run --rm -v "$(cygpath -w "$PWD"):/srv/jekyll" jekyll/jekyll:pages jekyll build
# _site/ now contains exactly what GitHub Pages will publish
```

## Deployment

There is no deploy step. Push to `main` and GitHub Pages builds and
publishes automatically.
```

- [ ] **Step 4: Run the test to verify it passes**

Run the Step 1 command. Expected: PASS.

- [ ] **Step 5: Sanity check the guide end-to-end**

Rebuild (`jekyll build`), then confirm the README's claims: `_data/site.yml` and `_data/projects.yml` are the only content files (grep `index.html` — every user-visible string comes from `site.data.*`), and `_config.yml` excludes `README.md` from `_site/` (so the guide isn't published):

```bash
MSYS_NO_PATHCONV=1 docker run --rm -v "$(cygpath -w "$PWD"):/srv/jekyll" jekyll/jekyll:pages jekyll build
test ! -f _site/README.html && echo "PASS: README excluded from build" || echo "NOTE: README.html found in _site"
```

- [ ] **Step 6: Commit**

```bash
git add README.md
git commit -m "docs: add editing guide to README"
```

---

### Task 10: Publish to GitHub Pages

**Files:**
- Modify: none (delivery task — remote, push, verification)
- Test: the live URL `https://kyrulamri.github.io/`

**Interfaces:**
- Consumes: the repo from Task 1 (branch `main`); `_config.yml` `url` from Task 1.
- Produces: the public `kyrulamri/kyrulamri.github.io` repository and the live site.

Prerequisites: the `gh` CLI installed and authenticated (`gh auth status`; if not, run `gh auth login`). The repo **must be public** — GitHub Pages for user sites is not served from private repos on free plans.

- [ ] **Step 1: Check whether the repo already exists**

```bash
gh repo view kyrulamri/kyrulamri.github.io >/dev/null 2>&1 \
  && echo "REPO EXISTS" || echo "REPO MISSING"
```

- [ ] **Step 2a: If REPO MISSING — create it and push**

```bash
gh repo create kyrulamri.github.io --public --source=. --remote=origin --push
```

- [ ] **Step 2b: If REPO EXISTS — add the remote and push**

```bash
git remote add origin https://github.com/kyrulamri/kyrulamri.github.io.git
git push -u origin main
```

(Browser fallback for either: create an empty public repo named `kyrulamri.github.io` at github.com/new, then run the `git remote add` + `git push` lines.)

- [ ] **Step 3: Wait for the Pages build and verify the live site**

GitHub Pages builds user sites automatically on push; allow up to ~2 minutes on the first build.

```bash
for i in $(seq 1 12); do
  code=$(curl -s -o /dev/null -w "%{http_code}" https://kyrulamri.github.io/)
  echo "attempt $i: HTTP $code"
  [ "$code" = "200" ] && break
  sleep 10
done
curl -s https://kyrulamri.github.io/ | grep -c "kyrulamri"
```

Expected: the loop reaches `HTTP 200`, and the content grep returns a positive count (the hero name, brand, and JSON-LD all contain `kyrulamri`).

- [ ] **Step 4: Confirm the source setting (only if the site doesn't come up)**

Check github.com → `kyrulamri.github.io` → **Settings → Pages**. For `<user>.github.io` repos the source defaults to "Deploy from a branch → main / (root)" and is enabled automatically; if it's unset, select that option and save.

- [ ] **Step 5: Final acceptance**

1. `https://kyrulamri.github.io/` returns 200 and shows the hero, about, three project cards, contact buttons, and footer.
2. Edit `_data/projects.yml` from the GitHub web UI (change one project title), commit, wait ~1 minute, and confirm the change appears live — this proves the "editable directly from the repo" requirement end-to-end.
3. Visit an unknown path (e.g., `https://kyrulamri.github.io/nope`) — the branded 404 page renders.

---

## Self-Review

Run this checklist once, silently, before handing the plan over; fix any failures inline:

1. **Spec coverage** — every requirement from the conversation maps to a task:
   - "beautiful" → Tasks 2, 3, 5 (design tokens, hero, editorial layout, card grid)
   - "comply with anthropic design plan" → Global Constraints + Tasks 2–5 (verbatim palette, Poppins/Lora, cycling accents)
   - "able to be published at github.io" → Tasks 1, 10
   - "easily editable direct from the github repo" → Tasks 3–6 (data-driven content), Task 9 (README guide), Task 10 step 5 (end-to-end edit proof)
   - Placeholder content approved → Tasks 3, 5 sample data
2. **Placeholder scan** — every code step contains complete, runnable content. No "TBD"/"similar to Task N" anywhere; the only markers are the three `<!-- *_SECTION -->` comments, each replaced by an exact block in its own task.
3. **Type consistency** — one pass through names: `site.data.site.{name,tagline,about,socials}` and `site.data.projects.{title,description,url,tags}` are defined in Tasks 3/5 and consumed identically in Tasks 4–9; CSS classes and JS hooks (`#year`, `.nav-toggle`, `.nav-menu`, `.reveal`, `.is-visible`) are created before they're consumed; `--accent-ink` is defined in Task 2 before first use in Task 3.
