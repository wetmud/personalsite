# Jason Steltman — Personal Portfolio Site

## Project Overview
Static personal portfolio site for landing a **developer/programmer/coder job**. Built with vanilla HTML, CSS, and JavaScript — zero dependencies, no build tools. Currently multi-page (index.html, art.html, web.html, web2.html, blog.html, cv.html). Goal is to consolidate into a polished single-page portfolio.

**Custom domain (purchased):** `jasonsteltman.com` — bought on GoDaddy. To connect: add site in Cloudflare (free), point GoDaddy nameservers to Cloudflare NS, then add a CNAME or A record pointing to GitHub Pages (`<username>.github.io`), and add the custom domain in the GitHub Pages settings for this repo.

## Tech Stack
- Vanilla HTML5, CSS3, JavaScript (ES5)
- No frameworks, no package.json, no build process
- CSS Variables for theming, Grid/Flexbox for layout
- Fonts: Cabinet Grotesk (hero name, CV name), Zodiak (section titles/headers), Outfit (UI/body) — loaded via `@font-face local()` only for now
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
- Blog post `content` field is an array of strings — one string = one paragraph. Use `\'` to escape apostrophes, or wrap strings in double quotes.
- PHOTO_ROLL array in index.html — add filename to register new photos. `.tif` files do NOT work in browsers — convert to `.jpg` first.

## Social Links
- GitHub: [github.com/wetmud](https://github.com/wetmud)
- Behance: [behance.net/jasonsteltman](https://behance.net/jasonsteltman)
- Email: jason.steltman@gmail.com

---

## Current Status (as of 2026-03-18)

**Chunks 1–11 complete. Domain live. Site is in polish/refinement phase.**

### ✅ Completed
- **Chunks 1–3:** Full redesign — beige bg, Bebas Neue, grid lines, scroll reveal, horizontal card rows
- **Chunk 4:** All sections beige, larger modals w/ bottom-right shadow, ASCII breaks, CV renamed
- **Chunk 5:** Circle menu — top-right fixed, 60px circle, 360° spin, compact dropdown, social links, Escape closes
- **Chunk 6:** Hero photo roll — landscape 16/9, full right-column width, CSS crossfade every 4s, `photoroll/` folder
- **Chunk 7:** Smooth natural scroll — removed snap, `overflow-y:auto`, no wheel hijacking
- **Chunk 8:** Marquee — fixed 30px black bar at top of page, white text, CSS `@keyframes marquee`
- **Chunk 9:** Letter spin — each letter `<span class="letter">`, continuous spin on hover (3s), idle after 6s (5–9s random)
- **Chunk 10:** Cursor picker — bottom-right fixed, 8 packs from `cursor icons/`, `localStorage` persistence
- **Chunk 11:** "Want to Collaborate?" — scrolling black marquee banner at top of CV section (28s speed)
- **Session 2026-03-15:** Card centering fixed (min-height + justify-content:center + 90px top padding on #work/#art); photo roll expanded to 10 images; hero tagline updated; bio paragraph added to CV; French Press blog post added; CV banner slowed to 28s; scroll fixed (removed `scroll-behavior:smooth` from CSS — was causing freeze on native wheel scroll); fonts switched to Cabinet Grotesk + Zodiak
- **Session 2026-03-17:** Domain live — `jasonsteltman.com` on GitHub Pages via Cloudflare, HTTPS enforced. Mobile audit — circle menu overlap fixed, blog tag row overflow fixed, tap targets increased, 480px breakpoint added, ASCII break overflow fixed, overflow-x protection added. CSP header added. Removed from Netlify.
- **Session 2026-03-18:** SEO/favicon added (meta description, OG/Twitter cards, inline SVG favicon). Banner text changed to "Hey! Welcome to my website :) !". Subtitle added under hero name ("I can help with your website..."). Work cards + blog cards flipped from dark (#1a1a1a) to white/black text (now match art cards). Circle menu button restyled as pill (border-radius:40px, padding, border). Dropdown widened from 220px to 280px. Photo roll: clicking active image opens lightbox. Quest page investigated — no file exists in repo (never built; task is pre-work).
- **Session 2026-03-19:** Full security + code quality audit applied (see audit-2026-03-19.md). Email obfuscated, rel="noopener noreferrer" added site-wide, CSP + SEO meta added to all subpages, og:image added, dead CV link removed, contact form validation added, lazy loading + onerror fallbacks on art/work images, skip-to-content link, duplicate .cv-inner rule removed.

### Current Design State
- Sections: natural height, `75px` `.section-gap` divs between them, `var(--bg-light)` background
- Work/Art sections: `min-height:calc(100vh - 30px)`, `justify-content:center`, `padding-top:90px`, `padding-bottom:60px`; card rows use `padding:24px 80px 32px`; cards `420×520px`
- All cards (work, art, blog) use static `box-shadow:10px 14px 0px` (directional, always visible — not animated on hover). Hover only animates `transform:translateY(-4px) translateX(-4px)`. Shadow no longer transitions — GPU-composited only.
- ASCII breaks: single dot row `· · · ·` only, `font-size:14px`, `color:rgba(0,0,0,0.55)`
- Modal overlay: `top:30px` to center below marquee bar
- Sections renamed: "Selected Work" → "Web Projects", "Photography" → "Art"
- Hero: photo roll landscape above nav links in right column; letter spin on name; tagline "Creative systems. Thoughtful UX. Actionable Data."
- Top marquee: `position:fixed`, `height:30px`, `z-index:900`, black/white
- Circle menu: `top:42px` (below marquee), right-aligned dropdown with nav + social links
- Cursor picker: bottom-right, panel opens upward
- Scroll: native wheel scroll only — NO `scroll-behavior:smooth` on container. Nav clicks use JS `scrollToIndex()` via `requestAnimationFrame`.
- **Performance fixes applied (2026-03-16):** Removed all `backdrop-filter:blur()` from circle-btn, circle-dropdown, cursor-panel, and modal-overlay (biggest jank source). Photo roll interval cached + cleared on pagehide. Scroll reveal observer unobserves after first trigger. Blog filter now hide/show via CSS — no DOM rebuild on tag click. Letter spin uses single parent `mouseover` instead of per-letter listeners. Google Fonts loaded non-blocking via `media="print"` trick.
- `.dark-text` and `.light` classes on section intros are now redundant (safe to leave)

---

## Feature Roadmap

### Pending / Polish
- **Quest page drag** — no quest.html exists in repo at all; was never built. Needs to be created from scratch if still wanted.
- **Wire contact form to Formspree** → mailto: works for now with validation; switch to Formspree for proper server-side handling when ready
- **Self-host fonts** — Cabinet Grotesk + Zodiak `.woff2` from fontshare.com/fonts/cabinet-grotesk and fontshare.com/fonts/zodiak (currently `local()` only — broken for all visitors who haven't installed them)
- **Unify card shadows** — work cards still `0 2px 12px rgba(0,0,0,0.4)`, should match `0 2px 8px rgba(0,0,0,0.3)`; post card hover should use `10px 14px 0px` directional not vertical lift
- **Mobile art scroll row** — missing `@media` padding override; fade gradients 80px wide on mobile (too wide for 375px viewport)
- **Cursor picker submenus** — each of 8 categories needs 5+ cursors; currently only 1 per category
- **Add downloadable CV/resume PDF** — link is commented out, re-enable with real PDF href when ready
- Replace placeholder ASCII dot breaks with custom ASCII art (Jason's art goes in `<pre class="ascii-break">`)
- Fix duplicate `scrollToSection` declaration in JS (harmless but confusing)
- Fix duplicate `cv-inner` CSS rule (~lines 33 and 109)

### Low Priority
- Chunk 12 — Mini Game: small canvas-based browser game, separate page
- Turtle Loader: spinning turtle SVG page loader, fades out after content loads

---

## Known Gotchas
- `scroll-behavior:smooth` on `main.snap-container` causes native wheel scroll to feel frozen/sluggish — **do not re-add**
- `.tif` images silently fail in browsers — always use `.jpg`/`.png`/`.webp` in photoroll
- `local()` only `@font-face` works for Jason locally but fails for all other visitors — must self-host `.woff2`
- `body.style.overflow` manipulation in modal open/close is a no-op — body already has `overflow:hidden` in CSS
- Behance CDN image URLs have no fallback — if Behance rotates paths, all art cards break silently
- Form labels in CV section have no `for`/`id` association — accessibility issue

## Style Conventions
- Keep it vanilla — no frameworks unless explicitly discussed
- Inline `<style>` blocks per page (current pattern)
- Prefer CSS animations over JS where possible
- Mobile-first responsive design
- Maintain the minimalist editorial aesthetic — clean, typography-focused
- Just make the changes — no need to double-check obvious decisions mid-task
