# Altered Edges — public site

Static HTML for [Altered Edges, LLC](https://alterededges.github.io/public/), published with [GitHub Pages](https://pages.github.com/). Company presence plus per-app landing, support, and privacy pages used for App Store Connect URLs.

**Live site:** [https://alterededges.github.io/public/](https://alterededges.github.io/public/)

## What’s in this repo

| Path | Purpose |
|------|---------|
| Root (`index.html`, `support.html`, `privacy-policy.html`) | Word Slug 2 landing + root Support / Privacy |
| `original-word-sluggers.html` | Original Word Sluggers marketing page |
| `ws/` | Word Slug 2 pages (incl. `word-slug-2-account-deletion.html`) |
| `gb/` | [College Gymnastics Bingo](https://alterededges.github.io/public/gb/) |
| `pj/` | [Pocket Judge](https://alterededges.github.io/public/pj/) |
| `memsmith/` | [MemSmith](https://alterededges.github.io/public/memsmith/) |
| `thump/` | [Thump](https://alterededges.github.io/public/thump/) |
| `css/`, `js/`, `assets/` | Shared Freelancer theme styles, scripts, images |

Pages use the [Start Bootstrap — Freelancer](https://startbootstrap.com/theme/freelancer) template (Bootstrap + `css/styles.css`).

## Local preview

No build step. Serve from the **repository root** (the directory that contains `index.html`) so asset paths match production:

```bash
cd /path/to/public
python3 -m http.server 8000
```

Open [http://localhost:8000](http://localhost:8000). App folders work the same way (`/memsmith/`, `/thump/`, `/gb/`, `/pj/`, `/ws/`).

## Deploying to GitHub Pages

This is a **project site** (`…/public/`). Deploy from the **`master`** branch, site root `/` (**Settings → Pages** → Deploy from a branch). Push to `master`; GitHub hosts the static files. No build workflow is required.

## Editing tips

- Keep App Store Support / Privacy paths stable: root and each app’s `support.html` and `privacy-policy.html` (plus `ws/word-slug-2-account-deletion.html`).
- Prefer relative links so the site works locally and under `/public/`.
- After changing shared CSS or JS, spot-check root and each app folder (`../css/styles.css`, `../js/scripts.js`).
- Gym Bingo has no verified store listing URL yet — do not put Word Slug store badge links on `gb/` pages.
