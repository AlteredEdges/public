# Altered Edges — public site

Static HTML site for [Altered Edges, LLC](https://alterededges.github.io/public/). It is published with [GitHub Pages](https://pages.github.com/) and serves as the company’s basic web presence, including landing pages for apps and legal/support pages (privacy policy, support).

**Live site:** [https://alterededges.github.io/public/](https://alterededges.github.io/public/)

## What’s in this repo

| Area | Purpose |
|------|--------|
| Root (`index.html`, `support.html`, `privacy-policy.html`, …) | Main landing and shared legal/support pages |
| `gb/` | [College Gymnastics Bingo](https://alterededges.github.io/public/gb/) product pages |
| `pj/` | [Pocket Judge](https://alterededges.github.io/public/pj/) product pages |
| `ws/` | Alternate copy of Word Slug–related pages (same template family as root) |
| `css/`, `js/` | Shared styles and scripts |
| `assets/` | Images and other static assets |

The pages are based on the [Start Bootstrap — Freelancer](https://startbootstrap.com/theme/freelancer) template, with Bootstrap and shared CSS in `css/styles.css`.

## Local preview

There is no build step. Serve the folder over HTTP so asset paths behave like production.

**Important:** run the commands below from the **repository root** (the directory that contains `index.html`, `pj/`, `gb/`, and `css/`). If you start the server from another folder (for example a different project), paths like `/pj/` will 404.

```bash
cd /path/to/public   # your clone of this repo

# Python 3 — default port is 8000
python3 -m http.server

# Or pick a port explicitly (e.g. 8080)
python3 -m http.server 8080
```

Then open the URL that matches the port you used—for example [http://localhost:8000](http://localhost:8000) (default) or [http://localhost:8080](http://localhost:8080). Subpaths such as `/gb/`, `/pj/`, and `/ws/` work the same way (e.g. [http://localhost:8000/pj/](http://localhost:8000/pj/)).

## Deploying to GitHub Pages

This repository is set up as a **project site** (URL includes the repo name: `…/public/`).

1. Push changes to the branch GitHub Pages is configured to use (often `main`).
2. In the GitHub repo: **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to **Deploy from a branch** and choose the branch and folder (**/** root, unless you use `/docs`).

GitHub builds and hosts the static files automatically; no workflow file is required for a plain static site.

## Editing tips

- Prefer relative links between pages so the site works locally and on Pages.
- After changing shared CSS or JS, spot-check root, `gb/`, `pj/`, and `ws/` because they reuse `../css/styles.css` and `../js/scripts.js` from subfolders.
