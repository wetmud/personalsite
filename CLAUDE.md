# Jason Steltman — Personal Portfolio Site

## Project Overview
Static personal portfolio site for landing a **developer/programmer/coder job**. Built with vanilla HTML, CSS, and JavaScript — zero dependencies, no build tools. Currently multi-page (index.html, art.html, web.html, web2.html, blog.html, cv.html). Goal is to consolidate into a polished single-page portfolio.

## Tech Stack
- Vanilla HTML5, CSS3, JavaScript (ES5)
- No frameworks, no package.json, no build process
- CSS Variables for theming, Grid/Flexbox for layout, scroll-snap for sections
- Google Fonts: Caprasimo (display), Outfit (UI) — loaded via `<link>`
- System fonts fallback: Helvetica Neue, Times New Roman
- Custom base64 cursor on all pages

## File Structure
```
index.html          — Home/landing page
art.html            — Art portfolio (12 Behance projects, JS-rendered)
web.html            — Web projects grid (5 projects with modals)
web2.html           — Web projects list (alternative layout)
blog.html           — Blog with tag filtering (4 posts)
cv.html             — CV placeholder
images/             — Screenshots, backgrounds
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

## Current Status (as of 2026-03-12)

**Phase 1–3 redesign is COMPLETE and live on GitHub Pages.**

### ✅ Completed
- **Chunk 1 (Phase 1):** Background image removed from all 6 pages. `snapshot2.png`, `body::before` overlay, and `body>* z-index` rules cleaned up.
- **Chunk 2 (Phase 2–3):** `index.html` fully rewritten as single-page snap layout:
  - 5 full-screen `scroll-snap-type: y mandatory` sections (Hero, Work, Art, Blog, CV)
  - Dark hero with Caprasimo name + Outfit tagline: "Creative systems. Deep UX. Built with code."
  - Horizontal scroll work cards (5 projects with modals + lightbox)
  - Art grid (12 Behance photography cards with lightbox)
  - Blog section with tag filter (4 posts with modal full text)
  - CV with skills, links, contact form
  - Dot nav (right edge, 5 dots, dark/light theme flip)
  - Sticky header with IntersectionObserver (slides in after hero)
  - Shared JS: scrollToSection, openModal/closeModal, openLightbox, Escape handler
- `Githubrepo2 .png` renamed to `Githubrepo2_.png` (space in filename fix)

### Tech additions
- Google Fonts: Caprasimo (name/h1), Outfit (tagline/nav/labels)
- `scroll-snap-type: y mandatory` on `main.snap-container`
- `IntersectionObserver` for dot nav active state and sticky header
- JS DOM rendering with IIFEs for closure-safe event handlers (no innerHTML for data)

### Next steps (optional)
- Phase 4: Letter spin on hover + idle animation on hero name
- Phase 4: Scrolling welcome marquee banner
- Phase 5: Cursor picker, turtle loader, mini game

---

## Feature Roadmap (ordered easiest → hardest)

### Phase 1 — Quick Wins (High Impact, Easy)
1. **Remove background image** — Delete the repeating `snapshot2.png` background from all pages. Use clean solid/gradient bg instead.

### Phase 2 — Core Restructure (Highest Impact)
2. **Single-page consolidation** — Merge art, web, blog, and CV content into index.html as scroll sections. Add smooth scroll navigation. Add scroll-triggered animations (IntersectionObserver). Remove separate page files once consolidated.
3. **Sticky "Jason Steltman" header on scroll** — When user scrolls past hero, shrink name into a sticky top bar. Clicking it scrolls back to top.

### Phase 3 — Navigation & Polish
4. **Menu bubble, dropdown, text animation** — Redesign nav as a floating bubble/pill. Add dropdown with smooth open/close animation. Animated text transitions on menu items.
5. **Name letter spin on hover + idle animation** — Wrap each letter of "Jason Steltman" in a `<span>`. On mouseover, spin that letter. After 6s idle, all letters auto-rotate at different speeds via CSS animations with varied `animation-duration`.

### Phase 4 — Personality & Flair
6. **Scrolling welcome banner** — CSS marquee/keyframe banner: "Welcome! This is Jason's Website!" across the top.
7. **Welcome/landing page with cursor picker** — Splash screen: "Hey, I'm Jason Steltman. Welcome to my Official Website. Pick a cursor and poke around!" User selects a cursor from a visual library, then proceeds to main site.
8. **Cursor picker bubble** — Floating bubble UI for browsing/selecting custom cursors. Store selection in localStorage.

### Phase 5 — Fun Extras (Low Priority)
9. **Terrapin turtle loading screen** — Spinning turtle SVG/animation as page loader. Sits in corner on top of everything after load.
10. **Mini game page** — Small browser game (e.g., simple canvas game). Showcases JS skills but not critical for job hunting.
11. **Lotus video on scroll** — Embed video that plays/scrubs on scroll. Requires video asset.
12. **Plane guy banner animation** — Animated character flying a plane pulling the welcome banner. Requires sprite/SVG animation work.

---

## Style Conventions
- Keep it vanilla — no frameworks unless explicitly discussed
- Inline `<style>` blocks per page (current pattern)
- Prefer CSS animations over JS where possible
- Mobile-first responsive design
- Maintain the minimalist aesthetic — clean, typography-focused
