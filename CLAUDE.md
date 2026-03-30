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

---

## Weather Canvas — Spec (not yet built)

Live weather animation on the hero section, driven by Open-Meteo API (Burlington hardcoded, no API key required).

### Status
- CSP updated: `https://api.open-meteo.com` added to `connect-src` ✓
- Rain/drip/storm/snow/sun skeleton code written and in index.html ✓
- **Cloud animations not yet built** — spec below

### Architecture
- Single `<canvas id="weather-canvas">` absolutely positioned over `#hero`, `pointer-events:none`, `z-index:2`
- `<span id="weather-label">` bottom-right of hero: shows `Clear sky · 4°C · Burlington`, fades in after 1s delay
- Canvas resizes on `window.resize`
- Fetch on load → WMO code → `setMode()` → `requestAnimationFrame` loop
- All modes share one canvas, one loop. `setMode()` clears all particle arrays before switching.

### API
```
GET https://api.open-meteo.com/v1/forecast
  ?latitude=43.3255&longitude=-79.7990
  &current=temperature_2m,weathercode,is_day
  &temperature_unit=celsius
```

### WMO Code → Mode mapping
| Codes | Mode |
|---|---|
| 0, 1 | `sun` (day) or `clear` (night) |
| 2, 3 | `cloudy` |
| 51–67, 80–82 | `rain` |
| 71–77, 85–86 | `snow` |
| 95–99 | `storm` |

### Weather modes

**`rain`** — already built
- Drops: fall top→bottom with slight horizontal drift, `rgba(140,160,180)`, drawn as short angled line segments
- Impact ripples: ellipses flattened on Y axis (perspective), expand + fade on drop landing
- Drip trails: vertical streaks that form at impact point, drift down slowly with sine wobble, teardrop bead at bottom, fade out

**`storm`** — already built
- Same as rain but: higher spawn rate, drops angle right (wind), occasional full-canvas white flash (lightning)

**`snow`** — already built
- 90 flakes, varied radius (0.8–3px), slow sine-wobble drift, wrap around edges

**`sun`** — already built
- 8 radial gradient rays from top-right quadrant of canvas, slow opacity pulse

**`cloudy` — NOT YET BUILT**
- 3 parallax layers of clouds, each layer drifts left→right at different speeds
- Layer 1 (back): speed 0.08px/frame, alpha 0.18, large puffs
- Layer 2 (mid): speed 0.18px/frame, alpha 0.22, medium puffs
- Layer 3 (front): speed 0.30px/frame, alpha 0.28, small puffs, slightly darker
- Each cloud = cluster of 4–7 overlapping `ctx.arc` circles, varying radius
- When cloud.x > W + cloud.width, reset to x = -(cloud.width)
- Add `ctx.filter = 'blur(6px)'` on the canvas element via CSS for softness (not in JS — CSS filter is cheaper)
- `partly-cloudy` variant: same clouds at lower opacity (alpha × 0.55) + run the `sun` rays underneath

**`clear` (night)** — NOT YET BUILT
- 60–80 star dots, radius 0.5–1.5px, random positions fixed on canvas
- Each star pulses opacity with a slow sine at individual phase offset
- Very subtle — alpha range 0.08–0.22

### Cloud rendering detail
```
Each cloud object:
  { x, y, puffs: [{cx, cy, r}], width, speed, alpha, layer }

Puff cluster generation (on init):
  - Pick anchor point
  - Add 4–7 circles with: cx = anchor.x + rand(-40,40), cy = anchor.y + rand(-12,12), r = rand(18,52)
  - width = max(cx+r) - min(cx-r) for all puffs

Draw:
  ctx.save()
  ctx.globalAlpha = cloud.alpha
  ctx.fillStyle = 'rgba(255,255,255,1)'  // alpha controlled by globalAlpha
  puffs.forEach → ctx.beginPath(); ctx.arc(cx+cloud.x, cy+cloud.y, r, 0, PI*2); ctx.fill()
  ctx.restore()
```

### Init
- Clouds init with x spread randomly across 0→W so screen isn't empty on load
- 4–6 clouds per layer = 12–18 cloud objects total

### CSS additions needed
```css
#weather-canvas { filter: blur(0px); }  /* default */
.weather-cloudy #weather-canvas,
.weather-partly-cloudy #weather-canvas { filter: blur(5px); }
```
Add class to `#hero` when mode is cloudy/partly-cloudy. Avoids per-frame JS blur.

### What's left to build
1. `cloudy` + `partly-cloudy` mode (cloud init + tick + draw functions)
2. `clear` night mode (star init + tick + draw)
3. Wire `setMode('cloudy')` and `setMode('clear')` into the WMO mapping
4. Add CSS filter classes and `hero.className` toggle in `setMode()`
5. Test all modes — manually override `setMode('rain')` etc. in console to verify each
