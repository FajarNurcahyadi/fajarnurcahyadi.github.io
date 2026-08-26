# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page personal portfolio site (Muhamad Fajar Nurcahyadi — Fullstack Engineer & UI/UX Designer), meant to be hosted on GitHub Pages (repo name `fajarnurcahyadi.github.io-index.html`). There is no build system, no package manager, and no test suite — the entire site is one static HTML file plus image/PDF assets.

## Running / previewing

There are no npm scripts or build commands. To preview changes:

- Open [index.html](index.html) directly in a browser, or
- Serve it with any static file server and view at `http://localhost:8080` — `.vscode/launch.json` is preconfigured to launch a Chrome debug session against that URL, so running a local static server on port 8080 (e.g. the VS Code "Live Server" extension) lets you use VS Code's built-in "Launch Chrome against localhost" debug config.

There is no lint or test command — verify changes visually in the browser.

## Architecture

Everything lives in [index.html](index.html): structure, all CSS (in a single `<style>` block in `<head>`), and all JS (in a single `<script>` block before `</body>`). There are no separate `.css`/`.js` files and no framework/bundler — edits go directly into these inline blocks.

Key structural/behavioral points:

- **CSS custom properties** are defined once on `:root` (colors, spacing radius) and reused throughout — change the palette by editing these variables rather than hardcoding colors inline.
- **Sections are anchor-linked**: nav links, the mobile menu, and the hero CTA all target section IDs (`#about`, `#skills`, `#services`, `#resume`, `#portfolio`, `#contact`). Adding/renaming a section means updating both the `<section id="...">` and every nav/menu link pointing at it.
- **Scroll-reveal animation**: any element with class `reveal` starts hidden/offset and is switched to `.active` by the `reveal()` function on scroll (`window.addEventListener("scroll", reveal)`), based on its position relative to the viewport. New sections/blocks that should animate in need the `reveal` class added.
- **Portfolio filtering** is client-side only: each `.porto-card` carries a `data-category` attribute, and `filterPortfolio(cat, btnEl)` toggles card visibility by comparing against the clicked filter button. Adding a new portfolio category requires adding both a filter `<button>` (with matching `onclick="filterPortfolio('newcat', this)"`) and giving cards that `data-category`.
- **Project modal**: clicking a portfolio card's zoom icon calls `openModal(title, cat, desc, img)`, which populates the single shared `#modal` markup (image, title, category, description) rather than each card having its own modal.
- **Image fallbacks**: profile photo and project images use inline `onerror` handlers — `foto.jpg` falls back to a placeholder icon block, and portfolio images fall back to a generated `https://placehold.co/...` image. This means broken/missing image files fail visually gracefully instead of showing broken-image icons.
- **Mobile nav** is a separate full-screen `.mobile-menu` overlay (not a collapsed dropdown), toggled via `toggleMenu()` and shown/hidden with the `.open` class; the hamburger button only appears under the `max-width: 768px` media query.

## Known inconsistencies to be aware of

- `index.html` references `project8.jpg` (last UI/UX portfolio card), but no `project8.jpg` file exists in the repo — it currently falls through to the placehold.co `onerror` fallback and has a placeholder title (`"NAMA PROJECT UIUX 2"`). If asked to "finish" or "fill in" the portfolio, this is the card to complete.
- `bg.jpg` exists in the repo but is not referenced anywhere in `index.html` — likely leftover/unused.
- Copy is a mix of English and Indonesian (e.g. the Services section descriptions are in Indonesian while most other copy is English) — match the existing language of whatever section you're editing rather than normalizing it unprompted.
