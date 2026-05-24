# CLAUDE.md

Guidance for AI assistants working in this repository.

## Project overview

Static personal site published via [GitHub Pages](https://pages.github.com/) at `https://onufryk.github.io/`. It is a photo catalog of a Rubik’s-cube collection (“Маркові кубики Рубика”) — one HTML page with a table of images, names, and short descriptions in Ukrainian.

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
- **Tailwind CSS** — loaded via Play CDN (`https://cdn.tailwindcss.com`); no build step
- **Images** — JPEG in `img/`, referenced from table rows

Do not introduce Node, bundlers, or a static-site generator unless the owner explicitly asks for that migration.

## Adding or updating a cube entry

1. Add the photo under `img/`. Use the next sequential number (`014.jpg`) or `NNN-shortname.jpg` if the name helps (see `012-mirror.jpg`, `013-gear.jpg`).
2. In `index.html`, append a `<tr>` inside the table (after existing rows, before `</table>`):

```html
<tr>
  <td class="border border-gray-300 px-2 py-1 align-top"><a href="img/014.jpg"><img src="img/014.jpg" width="400" class="max-w-full h-auto" alt="" /></a></td>
  <td class="border border-gray-300 px-2 py-1 align-top">Product name</td>
  <td class="border border-gray-300 px-2 py-1 align-top">Короткий опис українською</td>
</tr>
```

3. Keep column order: **Фото** | **Назва** | **Подробиці**.
4. Append rows inside `<tbody>`. Use the same cell classes as existing rows. Linked thumbnails open the full image; optional `<b>NEW!!!</b>` for highlights (used sparingly).

Row order in the table is presentation order, not numeric file order (e.g. `005` and `006` rows are swapped relative to filenames).

## Content and language

- UI strings, captions, and descriptions are **Ukrainian**.
- Product names may mix English brand names with Ukrainian text (e.g. `Rubik's Cube`, `Кубик Рубика`).
- `<html lang="en">` is set on pages; do not change language attributes unless aligning them with content is intentional.

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
- **Preserve table layout** — bordered table inside `overflow-x-auto`, shared `border border-gray-300 px-2 py-1 align-top` on body cells, and `<caption class="caption-top mb-4 text-lg font-semibold">`.
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
| Page title / caption | `<title>`, `<caption>` in `index.html` |
| Styling | Tailwind utility classes on table elements; avoid new CSS files unless needed |
| Block directory listing | `img/index.html` |
