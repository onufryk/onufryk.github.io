# CLAUDE.md

Guidance for AI assistants working in this repository.

## Project overview

Static personal site published via [GitHub Pages](https://pages.github.com/) at `https://onufryk.github.io/`. It is a photo catalog of a Rubik’s-cube collection (“Маркові кубики Рубика”) — one HTML page with stacked cards (image, name, description) in Ukrainian.

There is no build step, package manager, framework source, or test suite. Changes are plain HTML/CSS assets committed to `main` and deployed automatically by GitHub Pages.

## Repository layout

```
.
├── index.html          # Main catalog page (only user-facing page)
├── img/
│   ├── index.html      # Empty placeholder (discourages bare directory listing)
│   ├── 001.jpg …       # Cube photos (~1–1.4 MB each)
│   ├── 012-mirror.jpg  # Descriptive suffix when numbering alone is unclear
│   └── 013-gear.jpg
├── .gitignore          # .DS_Store only
└── CLAUDE.md
```

## Tech stack

- **HTML5** — single page, hand-edited
- **Tailwind CSS** — Play CDN with inline `tailwind.config` (cube palette, Fredoka/Nunito via Google Fonts); no build step
- **Images** — JPEG in `img/`, referenced from table rows

Do not introduce Node, bundlers, or a static-site generator unless the owner explicitly asks for that migration.

## Adding or updating a cube entry

1. Add the photo under `img/`. Use the next sequential number (`014.jpg`) or `NNN-shortname.jpg` if the name helps (see `012-mirror.jpg`, `013-gear.jpg`).
2. In `index.html`, append an `<article>` inside `<main class="space-y-6">` (copy an existing card and adjust paths/text). Cycle `ring-cube-*` accent colors (red → blue → green → yellow → orange).

```html
<article class="group flex flex-col gap-5 overflow-hidden rounded-3xl border-2 border-white/80 bg-white/90 p-5 shadow-lg shadow-violet-200/50 backdrop-blur-sm transition hover:-translate-y-1 hover:shadow-xl hover:shadow-violet-300/40 sm:flex-row sm:items-start sm:p-6">
  <a href="img/014.jpg" class="block shrink-0 overflow-hidden rounded-2xl ring-4 ring-cube-red/30 transition group-hover:ring-cube-red/60">
    <img src="img/014.jpg" width="400" class="max-w-full h-auto transition duration-300 group-hover:scale-105" alt="Product name" />
  </a>
  <div class="min-w-0 flex-1">
    <h2 class="font-display text-2xl font-semibold text-slate-900">Product name</h2>
    <p class="mt-2 text-slate-600 leading-relaxed">Короткий опис українською</p>
  </div>
</article>
```

3. For new highlights, use the amber card variant and a `<span class="...">NEW</span>` badge (see Mirror Cube / Gear Cube entries).
4. Card order is presentation order, not numeric file order (e.g. `005` and `006` are swapped relative to filenames).

## Content and language

- UI strings, captions, and descriptions are **Ukrainian**.
- Product names may mix English brand names with Ukrainian text (e.g. `Rubik's Cube`, `Кубик Рубика`).
- `<html lang="uk">` on the main page; do not change language attributes without reason.

## Images

- Large JPEGs are committed to the repo; optimize only when asked — avoid re-encoding all assets in drive-by changes.
- `img/index.html` is intentionally minimal; leave it unless replacing with a redirect or explicit index page is requested.

## Deployment

- Remote: `https://github.com/onufryk/onufryk.github.io.git`
- Branch: `main` → GitHub Pages user site root
- No GitHub Actions workflow in-repo; publishing is the default Pages behavior on push to `main`.

To preview locally, open `index.html` in a browser or serve the repo root with any static file server.

## Conventions for edits

- **Minimize scope** — this is a one-page site; prefer editing `index.html` and adding one image over new abstractions.
- **Preserve card layout** — copy an existing `<article>` block; keep playful styling (rounded cards, cube-colored rings, hover lift) consistent with neighbors.
- **Tailwind CDN** — styling is utility classes in HTML; no separate CSS file unless the owner adds a build pipeline.
- **Commits** — only create git commits when the user explicitly asks.
- **No README** — there is no project README; do not add one unless requested.

## What to avoid

- Adding React, Vue, build pipelines, or npm without explicit direction.
- Splitting into multiple pages or components unless requested.
- English-only rewrites of Ukrainian copy without being asked.
- Force-pushing `main` or amending pushed commits unless the user explicitly requests it.

## Quick reference

| Task | Where to change |
|------|-----------------|
| New cube | `img/*.jpg` + new `<tr>` in `index.html` |
| Page title / heading | `<title>`, `<h1>` in `index.html` |
| Styling | Tailwind utilities + inline `tailwind.config`; minimal extra CSS in `<style>` only when needed |
| Block directory listing | `img/index.html` |
