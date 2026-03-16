# Chunk 5 — Circle Menu Button Design Spec
**Date:** 2026-03-14
**Status:** Approved

---

## Overview

Replace the current top-left rectangular `#menu-btn` + full-screen `#menu-overlay` with a top-right fixed circle button that opens a compact dropdown panel on click.

---

## What Gets Removed

- `#menu-btn` (rectangular, top-left, Bebas Neue "MENU" text)
- `#menu-overlay` (full-screen dark overlay)
- `#menu-close` (close button inside overlay)
- `#menu-footer` (bottom metadata inside overlay)
- All associated CSS blocks for the above
- `menuOpen` variable
- `toggleMenu()` function
- The second `keydown` listener (lines ~489–491) that calls `toggleMenu()` — merge Escape handling into one listener (see Behaviour section)

---

## HTML Structure

```html
<div id="circle-menu">
  <button id="circle-btn" onclick="toggleCircleMenu()">+</button>
  <div id="circle-dropdown">
    <nav class="circle-nav">
      <a onclick="scrollToSection('hero');toggleCircleMenu()">Home</a>
      <a onclick="scrollToSection('work');toggleCircleMenu()">Work</a>
      <a onclick="scrollToSection('art');toggleCircleMenu()">Art</a>
      <a onclick="scrollToSection('blog');toggleCircleMenu()">Blog</a>
      <a onclick="scrollToSection('cv');toggleCircleMenu()">Collaborate</a>
    </nav>
    <div class="circle-social">
      <a href="https://github.com/wetmud" target="_blank">GitHub</a>
      <a href="https://behance.net/jasonsteltman" target="_blank">Behance</a>
      <a href="mailto:jason.steltman@gmail.com">Email</a>
    </div>
  </div>
</div>
```

Both `#circle-btn` and `#circle-dropdown` are children of `#circle-menu`. This ensures the click-outside handler (`!e.target.closest('#circle-menu')`) correctly identifies both button and panel as "inside" and won't close on interaction with either.

---

## The Button (`#circle-btn`)

- `position: fixed` via parent `#circle-menu` wrapper (see below)
- `width: 60px; height: 60px; border-radius: 50%`
- `display: flex; align-items: center; justify-content: center` — centers `+` precisely
- Font: Bebas Neue, `28px`, `line-height: 1`
- Default state (dark sections): `color: #fff; background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.2)`
- `.light-mode` state: `color: #111; background: rgba(0,0,0,0.06); border: 1px solid rgba(0,0,0,0.15)`
- Since all sections are currently `data-theme="light"`, button will always render in `.light-mode` in practice — but both states defined for future-proofing
- `backdrop-filter: blur(10px); -webkit-backdrop-filter: blur(10px)`
- Browser resets: `border: none` overridden by above; `outline: none` on focus (add `focus-visible` outline for a11y: `outline: 2px solid var(--ink); outline-offset: 3px`); `cursor: pointer; appearance: none; -webkit-appearance: none`
- Transition: `transform 0.35s cubic-bezier(0.16, 1, 0.3, 1), background 0.2s, border-color 0.2s`
- When `#circle-menu.open #circle-btn`: `transform: rotate(135deg)` — turns `+` into `×`

### Wrapper (`#circle-menu`)

- `position: fixed; top: 24px; right: 32px; z-index: 600`
- `display: flex; flex-direction: column; align-items: flex-end`
- No background, no dimensions — purely a positioning container

---

## The Dropdown Panel (`#circle-dropdown`)

- Sibling to `#circle-btn` inside `#circle-menu`
- `width: 220px`
- `max-height: 0; overflow: hidden; opacity: 0; pointer-events: none`
- `transform: translateY(-8px)`
- Transition: `max-height 0.38s cubic-bezier(0.16,1,0.3,1), opacity 0.28s ease, transform 0.28s cubic-bezier(0.16,1,0.3,1)`
- `border-radius: 16px`
- `margin-top: 10px` (gap between button and panel)

When `#circle-menu.open #circle-dropdown`:
- `max-height: 400px; opacity: 1; pointer-events: all; transform: translateY(0)`

Background + border:
- `background: rgba(245, 240, 232, 0.96)`
- `backdrop-filter: blur(16px); -webkit-backdrop-filter: blur(16px)`
- `border: 1px solid rgba(0,0,0,0.1)`
- `box-shadow: 0 8px 32px rgba(0,0,0,0.12)`

---

## Nav Links (`.circle-nav`)

- Container padding: `24px 28px 16px`
- `display: flex; flex-direction: column; gap: 2px`
- Each `<a>`:
  - Font: Bebas Neue, `32px`, `letter-spacing: 0.02em`, `color: var(--ink)`
  - `display: block; text-decoration: none; cursor: pointer`
  - `transition: color 0.15s, transform 0.15s`
  - Hover: `color: var(--muted); transform: translateX(4px)`

---

## Social Links Row (`.circle-social`)

- `border-top: 1px solid rgba(0,0,0,0.08)`
- `padding: 12px 28px 20px`
- `display: flex; gap: 16px`
- Each `<a>`:
  - Font: Outfit, `10px`, `letter-spacing: 0.14em`, `text-transform: uppercase`
  - `color: var(--muted); text-decoration: none`
  - Hover: `color: var(--ink)`

---

## Behaviour & JS

**New JS variable + function:**
```js
var circleMenuOpen = false;
function toggleCircleMenu() {
  circleMenuOpen = !circleMenuOpen;
  document.getElementById('circle-menu').classList.toggle('open', circleMenuOpen);
}
```

**Click outside to close:**
```js
document.addEventListener('click', function(e) {
  if (circleMenuOpen && !e.target.closest('#circle-menu')) {
    toggleCircleMenu();
  }
});
```

**Escape key — merge into the single existing `keydown` listener (the one that handles modals + lightbox):**
- Remove the second standalone `keydown` listener that calls `toggleMenu()`
- In the existing listener, replace `if(!menuOpen)document.body.style.overflow=''` with just `document.body.style.overflow=''` (no menu lock needed)
- Add `if(circleMenuOpen) toggleCircleMenu();` to the Escape block

**`themeObserver` update:**
- Change `document.getElementById('menu-btn')` reference to `document.getElementById('circle-btn')`
- Toggle `.light-mode` on `#circle-btn` as before

**No `body.overflow` locking** — the compact dropdown doesn't need it. Modal open/close continues to manage `overflow` independently via `openModal()` / `closeModal()`.

---

## Mobile (`@media (max-width: 768px)`)

- Remove old refs: `#menu-btn`, `#menu-overlay`, `#menu-footer` rules
- `#circle-menu`: `top: 20px; right: 24px`
- `#circle-dropdown`: `width: min(220px, calc(100vw - 48px))`
- `.circle-nav a`: font-size `26px`

---

## Implementation Checklist

1. Remove old HTML: `#menu-btn`, `#menu-overlay`, `#menu-close`, `#menu-footer`
2. Remove old CSS blocks for those elements
3. Add new HTML: `#circle-menu` wrapper with `#circle-btn` + `#circle-dropdown`
4. Add new CSS rules (all in existing `<style>` block)
5. Remove `menuOpen` var + `toggleMenu()` fn from JS
6. Add `circleMenuOpen` var + `toggleCircleMenu()` fn
7. Add document click-outside listener
8. Merge Escape handling into single `keydown` listener; fix the `menuOpen` reference on the overflow line
9. Update `themeObserver` to reference `#circle-btn`
10. Update mobile `@media` block
