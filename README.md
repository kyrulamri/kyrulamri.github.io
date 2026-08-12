# kyrulamri.github.io

Personal site of **kyrulamri**, built with Jekyll and an
Anthropic-inspired design system. Published automatically to
[https://kyrulamri.github.io](https://kyrulamri.github.io) by GitHub
Pages on every push to `main`.

## Editing from the GitHub web UI (no local setup needed)

All content lives in two YAML files. Edit them on github.com, click
**Commit changes**, and the site rebuilds in about a minute.

| What you want to change                    | File                 |
| ------------------------------------------ | -------------------- |
| Name, tagline, about text, social links    | `_data/site.yml`     |
| Project cards (title, blurb, URL, tags)    | `_data/projects.yml` |
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

With Docker (the `MSYS_NO_PATHCONV`/`cygpath` form is for Windows Git
Bash; on macOS/Linux use `docker run --rm -v "$PWD:/srv/jekyll" -p 4000:4000 ...`):

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
