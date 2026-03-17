# Jason Steltman — Personal Portfolio

A hand-built portfolio site. No frameworks, no build step, no excuses.

**Live:** [wetmud.github.io/personalsite](https://wetmud.github.io/personalsite/)

---

## What's in here

- **Hero photo roll** — landscape slideshow, CSS crossfade every 4 seconds
- **Letter spin** — hover the name and watch it do its thing
- **Circle menu** — top-right nav that spins open, links to everything
- **Cursor picker** — 8 cursor packs, bottom-right corner, persists across sessions
- **Project modals** — click any card for a full-screen detail view
- **Art portfolio** — 12 Behance projects pulled in via JS
- **Blog** — tag filtering, no CMS, posts live as JS objects in the file
- **CV section** — scrolling marquee banner, contact form, downloadable resume (soon)
- **Top marquee bar** — fixed 30px black bar, white text, always scrolling

---

## Stack

Vanilla HTML5, CSS3, JavaScript. That's it.

- No React, no Vue, no build pipeline
- No `package.json`, no `node_modules`
- CSS Variables for theming, Grid + Flexbox for layout
- Fonts: Cabinet Grotesk + Zodiak (via `@font-face`)
- Animations: CSS `@keyframes` only — no GSAP, no libraries

---

## Run locally

```
open index.html
```

Seriously, that's it. No server required.

---

## Pages

| File | What it is |
|------|------------|
| `index.html` | Main portfolio — everything lives here |
| `art.html` | Art portfolio (12 Behance projects) |
| `web.html` | Web projects grid with modals |
| `blog.html` | Blog with tag filtering |
| `cv.html` | CV / resume |

---

## License

MIT — see [LICENSE](LICENSE)
