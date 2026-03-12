# Phase 1–3 Redesign Spec
**Date:** 2026-03-11
**Goal:** Land a developer/coder job. Transform the existing multi-page portfolio into a polished, single-page full-screen snap experience.

---

## Decisions

| Decision | Choice |
|---|---|
| Layout | Full-screen snap sections (`scroll-snap-type: y mandatory`) |
| Navigation | Side dot nav — right edge, fills on active section |
| Hero | Split — name left, section index right |
| Section order | Hero → Web → Art → Blog → CV/Contact |
| Implementation | Rewrite `index.html` as the single page. Existing pages stay as backups. |
| Name font | Caprasimo (Google Fonts) |
| Tagline font | Outfit (Google Fonts) |
| Tagline copy | "Creative systems. Deep UX. Built with code." |
| Web projects layout | Horizontal scroll row of cards |
| Kinetic letter spin | Deferred to Phase 5 |

---

## Phase 1 — Remove Background Image

- Remove `background-image: url('images/snapshot2.png')` from all pages (`index.html`, `art.html`, `web.html`, `web2.html`, `blog.html`, `cv.html`)
- Replace with solid background color per page:
  - `index.html`: `#0a0a0a` (near-black, full dark theme)
  - All other pages: keep `#fafaf8` (off-white, existing aesthetic)
- Remove any `background-repeat`, `background-size`, `background-attachment` properties that reference the image

---

## Phase 2 — Single-Page Consolidation

### Global Structure

```
index.html
├── <head> — Google Fonts (Caprasimo, Outfit), existing CSS variables updated
├── <header> — sticky name bar (hidden on load, appears after hero)
├── <nav class="dot-nav"> — fixed right-edge dots
├── <main>
│   ├── <section id="hero">
│   ├── <section id="work">
│   ├── <section id="art">
│   ├── <section id="blog">
│   └── <section id="cv">
└── <script> — IntersectionObserver, modal logic, scroll behavior
```

### CSS Scroll Snap

```css
main {
  height: 100vh;
  overflow-y: scroll;
  scroll-snap-type: y mandatory;
}
section {
  height: 100vh;
  scroll-snap-align: start;
  overflow: hidden; /* outer snap container controls scroll; inner overflow handled by child divs */
}
```

Sections that have more content than fits in 100vh (Art, Blog) use an inner scrollable `<div>` with `overflow-y: auto`. The snap container snaps between sections; the inner div handles overflow within a section. The outer `overflow: hidden` prevents the snap container from intercepting scroll intended for the inner div — inner scroll is consumed first by the browser, and only once the inner div reaches its end does the snap scroll take over.

**Smooth scroll implementation:** Use JS `element.scrollIntoView({ behavior: 'smooth', block: 'start' })` for all anchor/dot clicks. Do NOT use CSS `scroll-behavior: smooth` on the snap container — this conflicts with `scroll-snap-type` in Safari and Firefox.

### Color Palette

| Variable | Value | Usage |
|---|---|---|
| `--bg-dark` | `#0a0a0a` | Hero, Work, Blog sections |
| `--bg-light` | `#fafaf8` | Art section |
| `--ink` | `#111111` | Dark text on light |
| `--ink-light` | `#ffffff` | Light text on dark |
| `--muted` | `#666666` | Secondary text |
| `--rule` | `rgba(255,255,255,0.1)` | Dividers on dark sections |

---

### Section: Hero

**Layout:** Two-column CSS Grid, `1fr 1fr`, full viewport height. Vertical rule between columns.

**Left column:**
- `<h1>` with two `<div>` lines: "Jason" / "Steltman"
- Font: Caprasimo, large (`clamp(56px, 8vw, 120px)`)
- Color: `#ffffff`
- Existing slideUp animation kept (cubic-bezier 0.16 1 0.3 1)
- Below name: tagline in Outfit — *"Creative systems. Deep UX. Built with code."*
- Tagline: `font-size: clamp(13px, 1.5vw, 18px)`, `color: #888`, `letter-spacing: 0.05em`

**Right column:**
- Vertical list of section anchors:
  - `→ Work`
  - `→ Art`
  - `→ Blog`
  - `→ CV`
- Font: Outfit, small (`14px`), uppercase, letter-spaced
- Color: `#555`, hover → `#ffffff` with transition
- Each anchor smooth-scrolls to its section
- FadeIn animation on load (0.6s, 0.8s delay)

**Mobile (≤768px):** Stack vertically. Name full width first, then tagline, then section index links as a horizontal wrapping flex row below. Font size drops to `clamp(40px, 12vw, 72px)` for name.

---

### Section: Work (Web Projects)

**Background:** `#0a0a0a`
**Label:** `WORK` — top-left, `11px`, uppercase, `letter-spacing: 0.2em`, `color: #444`

**Layout:** Horizontal scroll row
- `display: flex; overflow-x: auto; gap: 24px; padding: 0 60px; align-items: center; height: 100%`
- Hide scrollbar (`::-webkit-scrollbar { display: none }`)
- Subtle left/right scroll hint (gradient fade on edges)

**Cards:** Same data as `web.html` — 5 project cards
- Card size: `~340px wide × 420px tall`, fixed
- Same hover effect (translateY -5px, scale 1.01, shadow)
- Click opens existing modal overlay
- Escape closes modal

**Content source:** Copy JS `PROJECTS` array and modal JS verbatim from `web.html` into `index.html`. Do not rewrite or adapt the modal logic — use it as-is.

---

### Section: Art

**Background:** `#fafaf8`
**Label:** `ART` — top-left, same style as WORK label but `color: #bbb`
**Layout:** CSS Grid, `auto-fill, minmax(200px, 1fr)`, gap `16px`, inside a scrollable inner div
- Inner div: `height: calc(100vh - 80px); overflow-y: auto; padding: 0 60px 40px`
- Outer section: `overflow: hidden` (snap boundary)

**Content source:** Copy JS `PROJECTS` array and modal/lightbox JS verbatim from `art.html`

---

### Section: Blog

**Background:** `#0a0a0a`
**Label:** `BLOG`
**Layout:** Tag filter row at top (~48px), posts below in a 2-column card grid inside a scrollable inner div
- Inner div: `height: calc(100vh - 48px - 80px); overflow-y: auto; padding: 0 60px 40px`
- Outer section: `overflow: hidden`

**Content source:** Copy `POSTS` array and tag filter JS verbatim from `blog.html`

---

### Section: CV / Contact

**Background:** `#fafaf8`
**Label:** `CV`

**Layout:** Two-column grid, `1fr 1fr`
- **Left — CV info:**
  - Name: Jason Steltman
  - Title: Developer · Designer · Artist
  - Skills list: HTML/CSS, JavaScript, Python, UI/UX, Prompt Engineering
  - Links: GitHub (github.com/wetmud), Behance (behance.net/jasonsteltman)
  - Download link: "Download CV →" (placeholder `href="#"` until PDF is ready)
- **Right — Contact form:** Name, Email, Message fields + Send button. Extracted verbatim from the existing modal in `index.html`. No modal wrapper needed — placed inline in the section.

---

## Phase 3 — Sticky Name Banner

**Element:** `<header id="site-header">` — fixed top, full width, `height: 48px`

**Behavior:**
- On load: `opacity: 0; transform: translateY(-48px)` — invisible above viewport
- IntersectionObserver on `#hero` with threshold `0` (fires as soon as any part of hero leaves view)
- When hero is NOT intersecting: transition to `opacity: 1; transform: translateY(0)`
- When hero IS intersecting: hide again (`opacity: 0; transform: translateY(-48px)`)
- With `scroll-snap-type: y mandatory`, snapping is binary — the hero is either fully in view or fully out. The observer fires cleanly at snap boundaries.

**Contents:**
- Left: `JASON STELTMAN` — Caprasimo, `16px`, `color: #fff` (or `#111` on light sections)
- Right: `Work · Art · Blog · CV` — Outfit, `12px`, same smooth-scroll anchors as hero index
- Clicking name scrolls to `#hero`

**Background:** `rgba(10, 10, 10, 0.85)` with `backdrop-filter: blur(12px)` — glass effect over any section

**Transition:** `0.35s cubic-bezier(0.16, 1, 0.3, 1)` — same easing as existing animations

---

## Side Dot Nav

**Element:** `<nav class="dot-nav">` — fixed, right edge, vertically centered

**5 dots** — one per section (hero, work, art, blog, cv)

**States on dark sections (hero, work, blog):**
- Default: `8px` hollow circle, `border: 1.5px solid rgba(255,255,255,0.3)`, transparent fill
- Active: `background: #ffffff`
- Hover label: white text

**States on light sections (art, cv):**
- Default: `border: 1.5px solid rgba(0,0,0,0.25)`, transparent fill
- Active: `background: #111111`
- Hover label: `#111` text

The dot nav element itself has no background — dots inherit visibility from whatever section is behind them. The active section is determined by IntersectionObserver and a `data-theme="light"|"dark"` attribute on each `<section>`, which the JS reads to toggle dot color class.

**Hover label:** `<span class="dot-label">` positioned absolute, right of dot, `opacity 0 → 1` + `translateX(6px → 0)` on parent hover. `font-family: Outfit; font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase`.

**IntersectionObserver:** threshold `0.5` — marks dot active when section is ≥50% in view.

**Click:** `scrollIntoView({ behavior: 'smooth', block: 'start' })`

---

## Fonts

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Caprasimo&family=Outfit:wght@300;400;600&display=swap" rel="stylesheet">
```

- `Caprasimo, serif` — name only (h1). Falls back to serif if Google Fonts unavailable.
- `Outfit, sans-serif` — tagline, nav links, labels, body text. Falls back to sans-serif.
- `Times New Roman, serif` — blog post body text (existing character, no external load needed)

## Mobile Layout (≤768px)

| Section | Mobile behavior |
|---|---|
| Hero | Stack vertically: name → tagline → anchor links (horizontal flex wrap) |
| Work | Horizontal scroll row persists — native touch scroll works well |
| Art | Single-column grid (`minmax(140px, 1fr)`) |
| Blog | Single-column card stack |
| CV/Contact | Stack vertically: CV info above, contact form below |
| Dot nav | Hide on mobile (≤768px) — hero anchor links serve as primary mobile nav |
| Sticky header | Show on mobile — same behavior, provides nav after hero scrolls away |

---

## Out of Scope (This Phase)

- Kinetic letter animations (Phase 5)
- Scrolling welcome banner (Phase 4)
- Cursor picker (Phase 4)
- Turtle loading screen (Phase 5)
- Mini game (Phase 5)
- Lotus video scroll (Phase 5)
