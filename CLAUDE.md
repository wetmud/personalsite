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
- Color scheme: off-white bg (#fafaf8), near-black text (#111), blue links
- Responsive via `clamp()` fluid typography and CSS Grid auto-fill

## Current Features
- Hero animation (slideUp, fadeIn with cubic-bezier easing)
- Card hover effects (translateY, scale, shadow)
- Modal system (overlay blur, scale animation, Escape to close)
- Lightbox for image zoom
- Blog tag filtering
- Contact form modal (index.html#contact)

---

## Current Status (as of 2026-03-13)

**Phase 1–4 redesign is COMPLETE and live on GitHub Pages.**

### ✅ Completed
- **Chunk 1 (Phase 1):** Background image removed from all 6 pages.
- **Chunk 2 (Phase 2–3):** `index.html` fully rewritten as single-page snap layout (5 sections, modals, lightbox, dot nav, sticky header).
- **Chunk 3 (Phase 4 — Design Language):** eszterbial.com-inspired editorial aesthetic:
  - **Bebas Neue** added as condensed display font (hero name, section headings, MENU button)
  - **6-column vertical grid lines** overlay via `repeating-linear-gradient` (editorial feel)
  - **95vh sections** with `display:flex flex-direction:column` — 5vh bleed shows next section below
  - **Section intros** replaced tiny labels: large Bebas Neue title + right-aligned metadata + rule line
  - **MENU button** (top-left, fixed) replaces header + dot nav:
    - Full-screen dark overlay with staggered Bebas Neue nav links (Home/Work/Art/Blog/Contact)
    - Menu button adapts light/dark via `IntersectionObserver` section theme tracking
    - Keyboard: Escape closes menu
  - **Scroll reveal system**: `.reveal` + `.visible` classes with staggered `setTimeout` delays
  - **Art section**: changed from CSS grid → horizontal scroll row (matches Work layout)
  - **Unified card size**: Work + Art cards both `340×440px`, same radius and shadow
  - **CV section**: `align-content:end` pins content to bottom of section
  - **Copyright** line at very bottom of CV spanning both columns
  - **Warm beige** bg-light: `#f5f0e8`
  - Grid lines z-index fix: `::before` at `z-index:0`, sections at `z-index:1`

### Tech additions
- Google Fonts: Caprasimo (name/h1), Outfit (tagline/nav/labels)
- `scroll-snap-type: y mandatory` on `main.snap-container`
- `IntersectionObserver` for dot nav active state and sticky header
- JS DOM rendering with IIFEs for closure-safe event handlers (no innerHTML for data)

---

## Feature Roadmap (ordered easiest → hardest)

All completed phases are done. Below is the active + future work, easiest first.

### Chunk 4 — Navigation Redesign (Next Up)
**Goal:** Replace the current top-left full-screen MENU overlay with a polished circle button (top-right) that opens a compact dropdown panel.

- **Circle menu button (top-right, fixed)**
  - Circular button replaces current rectangular MENU button
  - On click: circle rotates (CSS `transform: rotate`), dropdown slides down below it
  - Button still adapts light/dark via `IntersectionObserver` (existing system)
  - Keyboard: Escape closes
- **Dropdown panel (not full-screen)**
  - Compact floating panel below the circle, slides/fades in
  - Nav links: Home / Work / Art / Blog / Contact — stacked, Bebas Neue
  - Social links row at bottom: Behance, GitHub, Email
  - Panel has subtle backdrop-blur + border, matches editorial aesthetic
- **Remove** full-screen `#menu-overlay` entirely

### Chunk 5 — Hero Photo Roll
**Goal:** Add a crossfading photo slideshow to the right column of the hero section.

- Right column layout: `flex-direction:column` — photo roll fills `flex:1` (top), index links pinned at bottom
- Slideshow pulls from `photoroll/` folder — images hardcoded as a JS array at top of `<script>` (update array when adding photos; starts empty/hidden until images are added)
- Auto-advances every ~4s with a smooth CSS crossfade (opacity transition, absolute-positioned images stacked)
- Centered within the column with tasteful padding, slight border-radius, no heavy borders
- To add new photos: drop image into `photoroll/`, add filename to the JS array near top of `<script>`

### Chunk 6 — Smooth JS Scroll
**Goal:** Replace `scroll-snap` with JS-driven smooth section scrolling that feels like eszterbial.com.

- Remove `scroll-snap-type: y mandatory` from `.snap-container` and `scroll-snap-align` from sections
- JS handles scroll: click nav → `scrollTo` with custom easing (e.g., ease-in-out cubic)
- Mousewheel/trackpad: debounced wheel listener, one section at a time, smooth animated scroll
- Sections fade in slightly as they enter viewport (opacity 0.85 → 1 during scroll)
- Keyboard arrow keys also trigger section scroll

### Chunk 7 — Scrolling Welcome Marquee
**Goal:** CSS marquee banner across a section or the top of hero.

- Text: "Welcome to Jason Steltman's Website — Developer — Designer — Builder —" (repeating)
- Pure CSS `@keyframes` animation, no JS
- Positioned either just below hero name or as a thin strip between sections

### Chunk 8 — Name Letter Spin
**Goal:** Hero name letters each wrapped in `<span>`, interactive + idle animation.

- Mouseover individual letter → it spins (CSS `rotateY` or `rotate`)
- After 6s idle → all letters auto-rotate at different speeds (varied `animation-duration`)
- Pure CSS animations triggered by JS class toggling

### Chunk 9 — Cursor Picker Bubble (Low Priority)
- Floating bubble UI for selecting a custom cursor from a visual library
- Selection stored in `localStorage`, applied site-wide on load

### Chunk 10 — Turtle Loader (Low Priority)
- Spinning turtle SVG as page loader, fades out after content loads

### Chunk 11 — Mini Game (Low Priority)
- Small canvas-based browser game, separate page

---

## Style Conventions
- Keep it vanilla — no frameworks unless explicitly discussed
- Inline `<style>` blocks per page (current pattern)
- Prefer CSS animations over JS where possible
- Mobile-first responsive design
- Maintain the minimalist aesthetic — clean, typography-focused
