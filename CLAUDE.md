# Jason Steltman — Personal Portfolio Site

## Project Overview
Static personal portfolio site for landing a **developer/programmer/coder job**. Built with vanilla HTML, CSS, and JavaScript — zero dependencies, no build tools. Currently multi-page (index.html, art.html, web.html, web2.html, blog.html, cv.html). Goal is to consolidate into a polished single-page portfolio.

## Tech Stack
- Vanilla HTML5, CSS3, JavaScript (ES5)
- No frameworks, no package.json, no build process
- CSS Variables for theming, Grid/Flexbox for layout (scroll-snap being replaced with JS smooth scroll)
- Google Fonts: Bebas Neue (display/headings), Caprasimo (unused, kept), Outfit (UI/body) — loaded via `<link>`
- System fonts fallback: Helvetica Neue, Times New Roman
- Custom base64 cursor on all pages

## File Structure
```
index.html          — Home/landing page (single-page app, all content)
art.html            — Art portfolio (12 Behance projects, JS-rendered)
web.html            — Web projects grid (5 projects with modals)
web2.html           — Web projects list (alternative layout)
blog.html           — Blog with tag filtering (4 posts)
cv.html             — CV placeholder
images/             — Screenshots, backgrounds
photoroll/          — Photos for hero slideshow (add images here to update roll)
writing/            — Legacy .docx files (not web-integrated)
copies/             — Backup HTML files
```

## Key Architecture Notes
- Content (blog posts, art projects) is stored as JS objects/arrays inside each HTML file
- Modals, lightbox, and tag filtering are vanilla JS with DOM manipulation
- Each page has its own `<style>` block — no shared CSS file
- Color scheme: warm beige bg (`#f5f0e8`), near-black text (`#111`)
- Responsive via `clamp()` fluid typography and CSS Grid auto-fill
- Blog posts are added by editing the `POSTS` array in `index.html` — no folder system

## Social Links
- GitHub: [github.com/wetmud](https://github.com/wetmud)
- Behance: [behance.net/jasonsteltman](https://behance.net/jasonsteltman)
- Email: jason.steltman@gmail.com

---

## Current Status (as of 2026-03-13)

**Chunks 1–4 complete. Chunk 5 (circle menu) is next.**

### ✅ Completed
- **Chunk 1:** Background image removed from all 6 pages.
- **Chunk 2:** `index.html` fully rewritten as single-page snap layout (5 sections, modals, lightbox, dot nav, sticky header).
- **Chunk 3 — Design Language:** eszterbial.com-inspired editorial aesthetic:
  - **Bebas Neue** condensed display font (hero name, section headings, MENU button)
  - **6-column vertical grid lines** overlay via `repeating-linear-gradient`
  - **95vh sections** — 5vh bleed shows next section below
  - **Section intros** — large Bebas Neue title + right-aligned metadata + rule line
  - **MENU button** (top-left, fixed) — full-screen dark overlay, staggered nav links, IntersectionObserver light/dark, Escape to close
  - **Scroll reveal**: `.reveal` + `.visible` classes, staggered `setTimeout` delays
  - **Horizontal scroll rows** for Work + Art cards (`340×440px` unified size)
  - **CV section** pinned to bottom with `align-content:end`
- **Chunk 4 — Visual Polish:**
  - **All sections beige** — `#hero`, `#work`, `#blog` switched to `var(--bg-light)` (`#f5f0e8`); all `data-theme="light"` now
  - **Larger modals** — `width: min(1100px, 92vw)`, bottom-right offset shadow (`14px 18px 0px rgba(0,0,0,0.12)`)
  - **Renamed** — "CV & Contact" → "Want to Collaborate?" / nav → "Collaborate" / hero index → "Collab"
  - **ASCII breaks** — `<pre class="ascii-break">` after each `<hr class="section-rule">` (placeholder text; Jason will replace with custom art)

### Current Design State
- All 5 sections use `data-theme="light"` — no dark sections remain
- `.section-intro-title` default color is `var(--ink)` (dark); `.dark-text` class is now redundant
- `.section-intro-count` default is `var(--muted)` dark; `.light` class is no longer needed
- Grid overlay uses `var(--grid-line-light)` (dark lines on beige)
- Work/art scroll wrapper fade gradients use `var(--bg-light)`
- Blog filter buttons: dark pill on beige, active state inverted (dark bg, beige text)

---

## Feature Roadmap (ordered easiest → hardest)

### Chunk 5 — Circle Menu Button (Next Up)
**Goal:** Replace top-left rectangular MENU + full-screen overlay with a top-right circle button + compact dropdown.

- **Circle button (top-right, fixed)** — on click: rotates via CSS `transform:rotate`, dropdown slides down below
- **Dropdown panel** — compact floating panel, not full-screen; backdrop-blur + border
  - Stacked Bebas Neue nav links: Home / Work / Art / Blog / Collaborate
  - Social links row at bottom: GitHub, Behance, email
- **Remove** `#menu-overlay`, `#menu-close`, existing `#menu-btn` entirely
- IntersectionObserver light/dark adaptation carries over to new button
- Keyboard: Escape closes

### Chunk 6 — Hero Photo Roll
**Goal:** Crossfading photo slideshow in the right column of the hero section.

- Photo roll sits centered in upper portion of right column; index links remain pinned below
- Images hardcoded as JS array near top of `<script>` — filenames from `photoroll/` folder
- Auto-advances every ~4s, CSS crossfade (opacity transition, absolute-positioned stacked images)
- Hidden/empty until images are added to array; to add: drop into `photoroll/`, add filename to array

### Chunk 7 — Smooth JS Scroll
**Goal:** Replace `scroll-snap` with JS-driven smooth scroll (eszterbial.com feel).

- Remove `scroll-snap-type: y mandatory` from `.snap-container` and `scroll-snap-align` from sections
- Mousewheel/trackpad: debounced, one section at a time, custom ease-in-out cubic easing
- Click nav → animated `scrollTo`, sections fade `0.85→1` as they enter viewport
- Keyboard arrow keys trigger section scroll

### Chunk 8 — Scrolling Welcome Marquee
- Pure CSS `@keyframes` marquee: "Welcome to Jason Steltman's Website — Developer — Designer — Builder —"
- Thin strip below hero name or between sections

### Chunk 9 — Name Letter Spin
- Each letter of hero name wrapped in `<span>`
- Mouseover → individual letter spins (`rotateY`)
- After 6s idle → all letters auto-rotate at varied speeds
- CSS animations, JS class toggling

### Chunk 10 — Cursor Picker Bubble (Low Priority)
- Floating bubble UI for selecting a custom cursor from a visual library
- Selection stored in `localStorage`, applied site-wide on load

### Chunk 11 — Turtle Loader (Low Priority)
- Spinning turtle SVG page loader, fades out after content loads

### Chunk 12 — Mini Game (Low Priority)
- Small canvas-based browser game, separate page

---

## Style Conventions
- Keep it vanilla — no frameworks unless explicitly discussed
- Inline `<style>` blocks per page (current pattern)
- Prefer CSS animations over JS where possible
- Mobile-first responsive design
- Maintain the minimalist editorial aesthetic — clean, typography-focused
- Just make the changes — no need to double-check obvious decisions mid-task
