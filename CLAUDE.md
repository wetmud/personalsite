# Jason Steltman — Personal Portfolio Site

Static personal portfolio. Vanilla HTML/CSS/JS — zero dependencies, no build tools, multi-page.

**Live:** `jasonsteltman.com` (GitHub Pages via Cloudflare, HTTPS enforced)

---

## Tech Stack

- Vanilla HTML5, CSS3, JavaScript (ES5)
- No frameworks, no package.json, no build process
- Fonts: Cabinet Grotesk (hero name), Zodiak (section titles), Outfit (UI/body) — `@font-face local()` only (broken for other visitors — needs self-hosting)
- Custom base64 cursor on all pages

## File Structure

```
index.html          — Home/landing (all content)
art.html            — Art portfolio (12 Behance projects, JS-rendered)
web.html            — Web projects grid (5 projects with modals)
blog.html           — Blog with tag filtering
cv.html             — CV placeholder
images/             — Screenshots, backgrounds
photoroll/          — Photos for hero slideshow (add images here to update roll)
```

---

## Current Design State

- Beige bg (`#f5f0e8`), near-black text (`#111`)
- Marquee: fixed 30px black bar at top, white text, `z-index:900`
- Circle menu: `top:42px` (below marquee), pill-shaped button, 280px dropdown
- Hero: photo roll (landscape 16/9, CSS crossfade every 4s) + letter spin on name
- Cards (work, art, blog): white bg, `box-shadow:10px 14px 0px` directional (always visible). Hover: `translateY(-4px) translateX(-4px)` only — shadow doesn't animate.
- ASCII breaks: `· · · ·` dot row, `font-size:14px`, `color:rgba(0,0,0,0.55)`
- Cursor picker: bottom-right, panel opens upward, 8 packs, `localStorage` persistence
- Scroll: native wheel only — NO `scroll-behavior:smooth` on container. Nav clicks use JS `scrollToIndex()` via `requestAnimationFrame`.

---

## Known Gotchas

- `scroll-behavior:smooth` on `main.snap-container` causes wheel scroll to freeze — **do not re-add**
- `.tif` images fail silently in browsers — always use `.jpg`/`.png`/`.webp`
- `local()` only `@font-face` works for Jason but fails for all visitors — must self-host `.woff2`
- `body.style.overflow` manipulation in modal open/close is a no-op (body already has `overflow:hidden`)
- Behance CDN image URLs have no fallback — if Behance rotates paths, art cards break silently
- Blog posts: `content` field is array of strings. Use `\'` to escape apostrophes or double-quote the string.
- PHOTO_ROLL array in index.html — add filename to register new photos

---

## Pending Features

- **Self-host fonts** — Cabinet Grotesk + Zodiak `.woff2` from fontshare.com (currently `local()` only)
- **Wire contact form to Formspree** — mailto: works for now; switch when ready
- **Quest page** — `quest.html` doesn't exist; needs to be built from scratch if still wanted
- **Cursor picker submenus** — each of 8 categories needs 5+ cursors; currently only 1 per
- **Downloadable CV/PDF** — link is commented out, re-enable with real PDF href
- Root/tree animation (scrolling SVG/canvas background) — idea noted, not building yet
- Liquid SVG animation (GSAP + inline SVG + 3 layered sine waves) — idea noted, not building yet
