# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Users

Anonymous visitors worldwide who do not know kyrulamri and land on
`kyrulamri.github.io` from any device or browser. Their job is to
learn who kyrulamri is and find their work and ways to reach them.
There is no authenticated or returning-user workflow.

## Product Purpose

A personal site whose single job is to tell the world who kyrulamri
is: a person who does coding, presented with a deliberately crafted,
Anthropic-brand-compliant design. Success means a first-time visitor
leaves with a clear, truthful picture of the person — identity, work,
and contact — regardless of device or color scheme.

## Positioning

The design itself is the position: the site visibly follows
Anthropic's official brand guidelines (warm neutral palette, Poppins
headings / Lora body, one restrained accent) as a deliberate statement
of taste and craft. A generic portfolio could not truthfully claim
that compliance. The person's actual work is the content; the
Anthropic-inspired presentation is the identity signal.

## Operating Context

- Visitors reach the site at `https://kyrulamri.github.io/`; it must
  render correctly on any device, any browser, in light or dark
  color scheme, with English content only.
- The owner edits all content directly in the GitHub web UI, from
  `_data/site.yml` and `_data/projects.yml`; pushes to `main` rebuild
  and republish automatically via GitHub Pages (classic Jekyll build).
- No blog, no custom domain, no résumé download — deliberately out of
  scope.

## Capabilities and Constraints

- Jekyll static site on GitHub Pages; no theme (custom CSS only),
  no build step for the editor, no frameworks — vanilla HTML/CSS/JS.
- Single page: header → hero → about → projects → contact → footer,
  with a branded 404 page. Content is rendered from data files via
  Liquid; editors never touch markup.
- Automatic dark mode via `prefers-color-scheme`; no toggle.
- Google Fonts: Poppins (headings) + Lora (body), Arial/Georgia
  fallbacks.
- Undecided: the actual projects, bio details, and specialization
  fields — the owner confirmed "coding" only. Placeholder content
  remains in place until the owner supplies real material.

## Brand Commitments

- Name: kyrulamri; repo and URL: `kyrulamri.github.io`.
- Binding design constraint: compliance with Anthropic's official
  brand-guidelines — palette `#faf9f5` (light), `#141413` (dark),
  `#b0aea5` mid gray, `#e8e6dc` light gray; accents `#d97757`
  (primary), `#6a9bcc`, `#788c5d`; Poppins headings / Lora body.
- English-language content only.

## Evidence on Hand

None real. The site ships placeholder content (three "Sample Project"
entries in `_data/projects.yml`, placeholder about text in
`_data/site.yml`) until the owner provides actual projects and bio.
Future work must not fabricate projects, credentials, testimonials,
or claims about kyrulamri's work.

## Product Principles

1. **Identity first.** Every visitor should leave knowing who
   kyrulamri is; clarity of the person outranks decoration.
2. **Design is the identity.** The Anthropic-inspired craft is a
   deliberate statement, not a skin — keep the compliance binding.
3. **Zero-friction editing.** All owner-editable content stays in the
   data files; nothing about the site may require touching markup.
4. **Global by default.** The page must work for anonymous visitors
   on any device, screen size, and color scheme.
5. **Truth over claims.** No invented projects or credentials;
   placeholders stay placeholders until the owner supplies real work.

## Accessibility & Inclusion

WCAG AA is a project requirement: AA text contrast in both color
schemes, semantic landmarks, skip link, keyboard focus styles, and
`prefers-reduced-motion` support.
