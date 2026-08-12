---
name: kyrulamri.github.io
description: Personal site of kyrulamri — The Clay Atelier, an Anthropic-inspired identity that is crafted, bookish, and unhurried.
colors:
  clay: "#d97757"
  chambray: "#6a9bcc"
  moss: "#788c5d"
  cream-paper: "#faf9f5"
  book-cloth: "#141413"
  wool: "#e8e6dc"
  pewter: "#b0aea5"
  muted-ink: "#5f5e57"
  terracotta-ink: "#b04e2f"
  hairline: "#dcd9cd"
  charcoal-surface: "#1e1d1c"
typography:
  display:
    fontFamily: "Poppins, Arial, sans-serif"
    fontSize: "clamp(2.75rem, 7vw, 4.75rem)"
    fontWeight: 600
    lineHeight: 1.15
    letterSpacing: "-0.03em"
  headline:
    fontFamily: "Poppins, Arial, sans-serif"
    fontSize: "clamp(1.5rem, 3vw, 2rem)"
    fontWeight: 600
    lineHeight: 1.15
    letterSpacing: "-0.02em"
  title:
    fontFamily: "Poppins, Arial, sans-serif"
    fontSize: "1.15rem"
    fontWeight: 600
    lineHeight: 1.15
    letterSpacing: "-0.01em"
  body:
    fontFamily: "Lora, Georgia, serif"
    fontSize: "1.0625rem"
    fontWeight: 400
    lineHeight: 1.7
    letterSpacing: "normal"
  label:
    fontFamily: "Poppins, Arial, sans-serif"
    fontSize: "0.8rem"
    fontWeight: 600
    letterSpacing: "0.16em"
rounded:
  sm: "8px"
  md: "14px"
  lg: "24px"
  pill: "999px"
spacing:
  xs: "0.5rem"
  sm: "0.75rem"
  md: "1.25rem"
  lg: "1.75rem"
  xl: "2.5rem"
components:
  button-primary:
    backgroundColor: "{colors.clay}"
    textColor: "{colors.book-cloth}"
    rounded: "{rounded.pill}"
    padding: "0.85rem 1.6rem"
  button-ghost:
    backgroundColor: "transparent"
    textColor: "{colors.book-cloth}"
    rounded: "{rounded.pill}"
    padding: "0.85rem 1.6rem"
  tag:
    backgroundColor: "{colors.cream-paper}"
    textColor: "{colors.book-cloth}"
    rounded: "{rounded.pill}"
    padding: "0.3rem 0.7rem"
  project-card:
    backgroundColor: "{colors.wool}"
    rounded: "{rounded.lg}"
    padding: "1.75rem"
  nav-link:
    textColor: "{colors.pewter}"
---

# Design System: kyrulamri.github.io

## Overview

**Creative North Star: "The Clay Atelier"**

This is a maker's workshop with the warmth of a well-loved studio: cream-paper walls, one clay accent that appears sparingly and means it, and type set the way a book is set. The site introduces a person who codes, and it does so with the unhurried confidence of something hand-finished — crafted, bookish, quiet. Nothing shouts; the identity is carried by material and rhythm rather than decoration.

The layout breathes: a single column of prose measured at a book-like length, sections separated by hairline rules, generous air around every element. The interface recedes so the person leads. At night the same workshop turns its lights down — the palette remaps automatically to a dark canvas, and the clay accent stays warm in the dark, like a lamp left on. Density is low and editorial everywhere; there is no UI clutter, no gratuitous motion, no ornament that isn't doing identity work.

**Key Characteristics:**
- One loud accent (Clay) on a dominant cream canvas; rarity is the point
- Bookish serif body (Lora) with geometric sans headings (Poppins) — never inverted
- Flat at rest; soft shadows only as a state response; dark mode uses tonal layering
- Generous corners on interactive cards (24px), fine controls smaller (8px), full pills for buttons and tags
- Automatic dark mode remaps the same tokens — no toggle, no separate dark design

## Colors

A warm, low-chroma palette built from Anthropic's official brand neutrals, with one terracotta accent that carries the entire emotional payload. The same tokens drive both color schemes; dark mode only remaps usage roles.

### Primary
- **Clay** (#d97757): The single loud color. Fills — primary buttons, selection highlight, focus rings, the 10px accent dot on project cards. Never body text on cream; on dark it may serve as accent text because it passes AA there (5.9:1).

### Secondary
- **Chambray** (#6a9bcc): Second voice in the project-card accent cycle. Used only as card dots and 15% tag tints — never as fills or text.

### Tertiary
- **Moss** (#788c5d): Third voice in the accent cycle. Same discipline as Chambray: dots and tints only.

### Neutral
- **Cream Paper** (#faf9f5): The canvas. Dominates every screen in light mode; also the text color on dark.
- **Book Cloth** (#141413): Ink — primary text, dark canvas, and text on Clay fills.
- **Wool** (#e8e6dc): Subtle surfaces — project-card backgrounds (light mode).
- **Pewter** (#b0aea5): Secondary elements — muted text on dark, brand mid-gray.
- **Muted Ink** (#5f5e57): AA-compliant secondary text on cream (4.5:1+), derived from Pewter.
- **Terracotta Ink** (#b04e2f): AA-compliant accent-colored text on cream (5.3:1) — eyebrows, prose links.
- **Hairline** (#dcd9cd): Borders and rules on cream — section dividers, ghost-button strokes, card edges on hover.
- **Charcoal Surface** (#1e1d1c): Card background in dark mode, lifted just off the dark canvas.

### Named Rules
**The One Voice Rule.** Clay is the only loud color, and it is used on a small fraction of any screen. Its rarity is the point; two Clay elements within one viewport need a reason.

**The Accent Cycle Rule.** Project cards cycle Clay → Chambray → Moss by list index. Accents appear as a 10px dot and a 15% tint behind tags — never as full-strength fills or text.

**The Ink Discipline Rule.** Accent-colored text uses Terracotta Ink on cream and Clay on dark. Full-strength accents are for fills and decoration, never for body or small text.

## Typography

**Display Font:** Poppins (with Arial)
**Body Font:** Lora (with Georgia)
**Label/Mono Font:** none — one sans, one serif, deliberately no third voice

**Character:** A geometric sans holding the structure, a warm serif doing the reading. Poppins gives headings a precise, almost architectural weight; Lora keeps body copy bookish and unhurried, like a well-set page. The pairing is Anthropic's own brand formula and is treated as a binding constraint.

### Hierarchy
- **Display** (Poppins 600, clamp(2.75rem, 7vw, 4.75rem), line-height 1.15, -0.03em): The hero name only. The single largest type on the page, and the only place letterspacing goes negative enough to matter.
- **Headline** (Poppins 600, clamp(1.5rem, 3vw, 2rem), 1.15, -0.02em): Section titles (About, Projects, Contact), extended by a hairline rule.
- **Title** (Poppins 600, 1.15rem, 1.15, -0.01em): Project card titles and the header brand mark (1.05rem).
- **Body** (Lora 400, 1.0625rem, 1.7): All running prose, measured at 40rem max (≈65–70ch). Taglines and descriptions may use muted ink at the same size.
- **Label** (Poppins 600, 0.8rem, 0.16em uppercase): The hero eyebrow. Tags are the same voice at 0.72rem / 0.02em; nav links at 0.9rem / 500 weight. All-caps is reserved for this class.

### Named Rules
**The Serif–Sans Rule.** Headings are always Poppins, body always Lora. Never inverted, never mixed within a phrase. The formula is the brand.

**The Uppercase Whisper Rule.** Uppercase is reserved for tiny labels — the eyebrow, tags, and nothing else. It never appears in sentences, headings, or body copy.

## Layout

A 72rem container with fluid gutters (`clamp(1.25rem, 4vw, 2.5rem)`), centering a single column of content on large screens. The hero is capped at 46rem and given generous vertical padding (`clamp(5rem, 12vw, 8.5rem)` top). Prose is measured at 40rem. Sections stack with `clamp(4rem, 9vw, 7rem)` vertical rhythm, each opened by a 1px hairline rule, and their titles extend into a hairline rule that carries the eye across the page.

Project cards form a responsive grid: `repeat(auto-fill, minmax(17rem, 1fr))` with 1.5rem gaps — three across on desktop, one column on phones. The header is a sticky 4rem bar with a translucent canvas scrim and backdrop blur. Below 720px the nav collapses to a Menu toggle; everything else flows to a single column. Content is English-only and reads LTR.

## Elevation & Depth

Flat by default. Surfaces are flat at rest; depth appears only as a response to state — buttons and cards lift 2–4px and gain a soft shadow on hover or focus. There is no ambient elevation, no layered composition, no permanent shadow vocabulary.

In dark mode shadows are effectively invisible, so depth is conveyed by tonal layering instead: cards lift off the canvas with a slightly lighter surface (Charcoal Surface on Book Cloth) plus a hairline border that appears on hover. The two schemes achieve the same hierarchy through different materials.

### Shadow Vocabulary
- **Rest** (`0 1px 2px rgba(20, 20, 19, 0.06)`): The quiet shadow under primary buttons and resting cards. Barely there.
- **Hover** (`0 6px 24px rgba(20, 20, 19, 0.09)`): The lifted shadow on hover/focus for buttons and cards.

### Named Rules
**The Flat-By-Default Rule.** Surfaces are flat at rest. Shadows appear only as a response to state — hover, focus, elevation. If a new surface needs depth at rest, use tonal layering, not a shadow.

## Shapes

A warm, generous form language. The most interactive surfaces are the roundest: project cards at 24px, buttons and tags as full pills (999px). Smaller, less prominent controls sit at 8px, with 14px as an intermediate. The accent dot is a 10px circle. Borders are 1px hairlines (1.5px on ghost buttons, which are the only elements that visibly carry a stroke at rest).

### Named Rules
**The Generous Corner Rule.** Interactive containers are the roundest shapes on the page (24px cards, full pills). Corners shrink as prominence falls; the smallest controls (8px) are never rounder than the cards they belong to.

## Components

### Buttons
- **Shape:** Full pill (999px).
- **Primary:** Clay background, Book Cloth text, Rest shadow, padding 0.85rem × 1.6rem, Poppins 600 at 0.95rem.
- **Hover / Focus:** Lifts 2px, shadow moves Rest → Hover. Focus-visible draws a 3px Clay outline offset 3px.
- **Ghost:** Transparent background, Book Cloth text, 1.5px Hairline stroke. Hover fills with the hover surface and drops the stroke.

### Chips (tags)
- **Style:** Full pill, Poppins 500 at 0.72rem uppercase, 0.02em tracking, padding 0.3rem × 0.7rem. Background is the canvas tinted 15% with the card's accent (color-mix); text stays full-contrast ink — the tint is decorative, the text is always AA-safe.
- **State:** Static; tags are metadata, not controls.

### Cards / Containers
- **Corner Style:** 24px (the roundest shape in the system).
- **Background:** Wool in light mode, Charcoal Surface in dark mode.
- **Shadow Strategy:** Rest shadow at rest; Hover shadow + 1px hairline border on hover (see Elevation & Depth).
- **Border:** None at rest; hairline appears on hover to define the edge.
- **Internal Padding:** 1.75rem, with 1rem gaps between title, description, and tags.
- **Signature detail:** a 10px accent dot (Clay / Chambray / Moss by index) above the title, and tags tinted to match.

### Navigation
- **Style:** Sticky 4rem bar, translucent canvas scrim (85%) with 10px backdrop blur, hairline bottom border.
- **Typography:** Brand mark Poppins 600 at 1.05rem; links Poppins 500 at 0.9rem in muted ink, darkening to full ink on hover.
- **Mobile:** Below 720px the menu collapses behind a Menu/Close toggle (Poppins 0.85rem, hairline stroke); links become full-width rows with hairline separators. The skip link precedes the header for keyboard users.

## Do's and Don'ts

### Do:
- **Do** start every new surface from the tokens in the frontmatter; they are normative.
- **Do** let Cream Paper dominate; Clay earns its meaning from scarcity.
- **Do** cycle project accents Clay → Chambray → Moss by index, as dots and 15% tints.
- **Do** use Terracotta Ink for accent-colored text on cream and Clay for accent text on dark.
- **Do** keep headings in Poppins and body in Lora — the pairing is binding.
- **Do** honor automatic dark mode by remapping usage tokens, never by forking the design.
- **Do** keep prose at 40rem measure and section rhythm at `clamp(4rem, 9vw, 7rem)`.
- **Do** keep surfaces flat at rest and let shadows appear only on hover and focus.

### Don't:
- **Don't** use full-strength Clay (or Chambray/Moss) as body or small text on cream — it fails AA (2.7:1).
- **Don't** add a manual theme toggle; dark mode follows the OS, by design.
- **Don't** introduce a third font family or a monospace voice.
- **Don't** use pure white (#ffffff) or pure black (#000000) anywhere — the palette's warm near-whites and near-blacks are the brand.
- **Don't** use shadows as ambient depth, and don't create new palette colors outside the frontmatter.
