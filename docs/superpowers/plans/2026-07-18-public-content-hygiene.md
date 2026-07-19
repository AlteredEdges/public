# Public site content hygiene Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Fix P0/P1 content and markup bugs on the Altered Edges static public site so App Store Support/Privacy URLs name the correct product, do not link the wrong stores, and validate as well-formed HTML.

**Architecture:** Keep the existing static HTML + shared `css/styles.css` + `js/scripts.js` on GitHub Pages. No framework, no redesign, no Bootstrap removal. Edit HTML in place; delete dead leftover files. Pattern to match for cleaned pages: `memsmith/` (correct product naming, closed footer, real alts where icons are text).

**Tech Stack:** Static HTML, Bootstrap 5.2.3 (CDN JS already linked), GitHub Pages project site at `https://alterededges.github.io/public/`

## Global Constraints

- Repo root: `/Users/jeremybenetz/dev/github/public` (or the worktree created for this branch)
- Branch: create `fix/public-content-hygiene` from current default branch; do not commit on `main`/`master` without explicit user consent
- Do **not** redesign the Freelancer template, replace Bootstrap, introduce a SSG, or change site IA (no studio hub yet)
- Do **not** break existing App Store Connect URLs: keep paths `privacy-policy.html`, `*/privacy-policy.html`, `*/support.html`, `ws/word-slug-2-account-deletion.html` live at the same paths
- Copyright year on touched footers: `2026`
- Word Slug 2 Apple link canonical form: `https://apps.apple.com/app/id6448659885` (no `/be/` locale)
- Word Slug 2 Play link stays: `https://play.google.com/store/apps/details?id=com.alterededges.wordslug2`
- Gym Bingo store URL is unknown in studio-hq — **never** leave Word Slug store links on Gym Bingo pages; use text CTA (Pocket Judge style) until a verified Gym Bingo URL exists
- Privacy copy for Word Slug / Gym Bingo must name **that** app only — never “Mindful Month Bracket”
- Prefer relative asset paths that already work under `…/public/` and subfolders
- Commit after each task; message focuses on why
- Verify with commands in each task (no Jest/pytest — use `rg` + a small Python HTML stack check)

## Out of scope

- Studio hub at `/`
- Dropping Freelancer/Bootstrap or Font Awesome
- Image compression / WebP conversion
- Custom domain
- Eleventy / React / Next
- Rewriting MemSmith / Thump / Pocket Judge pages (already correct enough)

---

### Task 1: Fix Gym Bingo product identity and store CTAs

**Files:**
- Modify: `gb/index.html`
- Modify: `gb/support.html` (favicon path if wrong)
- Modify: `gb/privacy-policy.html` (favicon + title only in this task; privacy body is Task 2)

**Interfaces:**
- Consumes: none
- Produces: Gym Bingo pages no longer claim to be Word Slug 2; no WS2 store URLs on `gb/`

- [ ] **Step 1: Inventory wrong strings on `gb/`**

Run from repo root:

```bash
rg -n 'Word Slug|wordslug2|apps\.apple\.com/be|favicon\.png|Mindful' gb/
```

Expected: hits for title “Word Slug 2”, WS2 App Store/Play links, `./favicon.png`, and Mindful Month in privacy (privacy body fixed in Task 2).

- [ ] **Step 2: Fix `gb/index.html` head + masthead CTAs**

In `gb/index.html`:

1. Set `<title>College Gymnastics Bingo</title>`
2. Set meta description to something like: `College Gymnastics Bingo — Altered Edges, LLC`
3. Change favicon to `../assets/favicon.ico`
4. In the masthead CTA block (the flex row with App Store / Play images linking to Word Slug), **remove** those wrong store links entirely
5. Replace with Pocket Judge–style text (no wrong URLs), for example:

```html
        <div
          class="d-flex flex-column align-items-center justify-content-center mt-4"
        >
          <p class="lead mb-3">Available on the App Store.</p>
          <p class="small mb-0 opacity-75">
            Store badge links will be added when a verified Gym Bingo listing URL
            is confirmed.
          </p>
        </div>
```

6. Remove or leave commented the already-commented About store badges that still contain WS2 links — if those comment blocks still contain live (uncommented) WS2 anchors anywhere in the file, delete those anchors. Prefer deleting dead commented Word Slug about copy if it is pure noise; do not resurrect it.

7. Replace every `alt="..."` in this file with meaningful alts (e.g. `College Gymnastics Bingo`, `Download on the App Store` only if badge remains — after this task badges should be gone).

- [ ] **Step 3: Fix favicon on `gb/support.html` and `gb/privacy-policy.html`**

Replace `href="./favicon.png"` with `href="../assets/favicon.ico"` in both files. Fix `<title>` if it still says Word Slug 2 (use `College Gymnastics Bingo — Support` / `College Gymnastics Bingo — Privacy Policy`).

- [ ] **Step 4: Verify**

```bash
rg -n 'Word Slug|wordslug2|apps\.apple\.com|play\.google|favicon\.png' gb/
```

Expected: **no** Word Slug / wordslug2 / apple/play store URLs / favicon.png under `gb/`. Mindful Month may still appear in privacy until Task 2.

- [ ] **Step 5: Commit**

```bash
git add gb/index.html gb/support.html gb/privacy-policy.html
git commit -m "$(cat <<'EOF'
fix(gb): correct Gym Bingo identity and remove Word Slug store links

EOF
)"
```

---

### Task 2: Fix privacy policies that still name Mindful Month

**Files:**
- Modify: `privacy-policy.html` (root Word Slug 2 privacy)
- Modify: `ws/privacy-policy.html`
- Modify: `gb/privacy-policy.html`
- Delete: `privacy_policy.html` (underscore — Mindful Month leftover; not linked from current footers that matter; confirm with `rg` before delete)

**Interfaces:**
- Consumes: Task 1 favicon/title fixes on `gb/privacy-policy.html`
- Produces: every remaining privacy page defines Application as the correct product

**Gold-standard tone (do not copy MemSmith’s local-only claims onto WS2):** use MemSmith structure for clarity, but content must match the app:

- Root + `ws/privacy-policy.html`: Application = **Word Slug 2**; company Altered Edges, LLC; contact `alterededges@gmail.com`; keep account/deletion language if already present elsewhere for WS2 (account deletion page stays at `ws/word-slug-2-account-deletion.html`)
- `gb/privacy-policy.html`: Application = **College Gymnastics Bingo** (or “Gym Bingo”); no Mindful Month; no Word Slug

- [ ] **Step 1: Confirm Mindful Month hits and who links to underscore file**

```bash
rg -n 'Mindful Month' --glob '*.html'
rg -n 'privacy_policy\.html' --glob '*.html'
```

Expected: Mindful in `privacy-policy.html`, `ws/privacy-policy.html`, `gb/privacy-policy.html`, and the underscore file. Underscore filename should have **zero** HTML links (or only self); if something links it, retarget that link to the hyphenated policy for the correct app before delete.

- [ ] **Step 2: Patch Application definition in the three hyphenated policies**

In each of `privacy-policy.html`, `ws/privacy-policy.html`, `gb/privacy-policy.html`, find the definition list / paragraph that says Application refers to Mindful Month Bracket and replace with the correct product name for that file. Also fix `<title>` and any H1 that still say Word Slug 2 on Gym Bingo privacy, or Mindful Month anywhere.

Minimum required string replacements (exact product names):

| File | Application name |
|------|------------------|
| `privacy-policy.html` | Word Slug 2 |
| `ws/privacy-policy.html` | Word Slug 2 |
| `gb/privacy-policy.html` | College Gymnastics Bingo |

Scan each file for other Mindful Month / wrong-app leftovers (meta description, headings) and fix them in the same edit.

- [ ] **Step 3: Delete the underscore leftover**

```bash
rm privacy_policy.html
```

- [ ] **Step 4: Verify**

```bash
rg -n 'Mindful Month' --glob '*.html'
test ! -f privacy_policy.html && echo 'underscore gone'
```

Expected: zero Mindful Month hits; underscore file gone.

- [ ] **Step 5: Commit**

```bash
git add privacy-policy.html ws/privacy-policy.html gb/privacy-policy.html
git add -u privacy_policy.html
git commit -m "$(cat <<'EOF'
fix: name the correct app in privacy policies; drop Mindful Month leftover

EOF
)"
```

---

### Task 3: Close broken copyright footers on older pages

**Files (all currently leave an unclosed outer `div` before `</body>`):**
- Modify: `index.html`
- Modify: `privacy-policy.html`
- Modify: `original-word-sluggers.html`
- Modify: `gb/index.html`
- Modify: `gb/privacy-policy.html`
- Modify: `gb/support.html`
- Modify: `ws/index.html`
- Modify: `ws/privacy-policy.html`
- Modify: `ws/support.html`
- Modify: `ws/original-word-sluggers.html`

**Reference (correct pattern from `memsmith/index.html`):**

```html
    <div class="copyright py-4 text-center text-white">
      <div
        class="container d-flex flex-column flex-md-row justify-content-between align-items-center gap-2"
      >
        <small>Copyright &copy; Altered Edges LLC 2026</small>
        <div>
          <a class="text-white me-3" href="./support.html"
            ><small>Support</small></a
          >
          <a class="text-white" href="./privacy-policy.html"
            ><small>Privacy Policy</small></a
          >
        </div>
      </div>
    </div>
```

Adjust relative hrefs per folder (`./` vs `../` vs `./ws/…`). Keep account-deletion link on Word Slug pages that already have it.

- [ ] **Step 1: Write a small stack checker and confirm failures**

Save as `/tmp/check_html_stack.py` (or `scripts/check_html_stack.py` if you prefer committing a tiny helper — optional; `/tmp` is fine):

```python
from html.parser import HTMLParser
from pathlib import Path

TRACK = {"div", "section", "nav", "header", "footer", "ul", "li", "a", "body", "html"}

class C(HTMLParser):
    def __init__(self):
        super().__init__()
        self.stack = []
        self.issues = []

    def handle_starttag(self, tag, attrs):
        if tag in TRACK:
            self.stack.append(tag)

    def handle_endtag(self, tag):
        if tag not in TRACK:
            return
        if not self.stack or self.stack[-1] != tag:
            self.issues.append(f"unexpected </{tag}> stack={self.stack[-5:]}")
        else:
            self.stack.pop()

files = [
    "index.html",
    "privacy-policy.html",
    "original-word-sluggers.html",
    "gb/index.html",
    "gb/privacy-policy.html",
    "gb/support.html",
    "ws/index.html",
    "ws/privacy-policy.html",
    "ws/support.html",
    "ws/original-word-sluggers.html",
]
for rel in files:
    p = Path(rel)
    c = C()
    c.feed(p.read_text(errors="replace"))
    status = "OK" if not c.stack and not c.issues else f"BAD stack={c.stack} issues={c.issues[:2]}"
    print(f"{rel}: {status}")
```

Run:

```bash
python3 /tmp/check_html_stack.py
```

Expected before fix: listed files report `BAD` with leftover `div`.

- [ ] **Step 2: Close footers**

For each BAD file, ensure the copyright block closes both the inner container `div` and the outer `copyright` `div` before Bootstrap script tags. Update copyright year to `2026` on those footers. Do not change MemSmith/Thump/PJ footers.

- [ ] **Step 3: Re-run stack checker**

```bash
python3 /tmp/check_html_stack.py
```

Expected: every listed file prints `OK`.

- [ ] **Step 4: Commit**

```bash
git add index.html privacy-policy.html original-word-sluggers.html \
  gb/index.html gb/privacy-policy.html gb/support.html \
  ws/index.html ws/privacy-policy.html ws/support.html ws/original-word-sluggers.html
git commit -m "$(cat <<'EOF'
fix: close copyright footers on legacy Freelancer pages

EOF
)"
```

---

### Task 4: Canonical Word Slug Apple links + meaningful image alts on WS pages

**Files:**
- Modify: `index.html`
- Modify: `ws/index.html`
- Modify: `original-word-sluggers.html`
- Modify: `ws/original-word-sluggers.html`
- Modify any other HTML under repo that still contains `apps.apple.com/be/`

**Interfaces:**
- Consumes: Task 3 footers already closed
- Produces: no `/be/` Apple links; Word Slug marketing pages have real `alt` text

- [ ] **Step 1: Find remaining `/be/` links**

```bash
rg -n 'apps\.apple\.com/be/' --glob '*.html'
```

- [ ] **Step 2: Replace all with canonical URL**

Replace every `https://apps.apple.com/be/app/word-slug-2/id6448659885` with:

`https://apps.apple.com/app/id6448659885`

- [ ] **Step 3: Fix `alt="..."` on Word Slug marketing pages**

On `index.html`, `ws/index.html`, `original-word-sluggers.html`, `ws/original-word-sluggers.html` (and privacy/support in those trees if they still have `alt="..."`):

| Image | Suggested alt |
|-------|----------------|
| Word Slug logo | `Word Slug 2 logo` |
| Icon-512 / masthead icon | `Word Slug 2 app icon` |
| App Store badge | `Download on the App Store` |
| Google Play badge | `Get it on Google Play` |
| Gameplay screenshot | `Word Slug 2 gameplay screenshot` |
| Main menu screenshot | `Word Slug 2 main menu screenshot` |

- [ ] **Step 4: Verify**

```bash
rg -n 'apps\.apple\.com/be/' --glob '*.html'
rg -n 'alt="\.\.\."' index.html ws/index.html original-word-sluggers.html ws/original-word-sluggers.html
```

Expected: zero `/be/` hits; zero `alt="..."` on those four files.

- [ ] **Step 5: Commit**

```bash
git add index.html ws/index.html original-word-sluggers.html ws/original-word-sluggers.html
# include any other files you changed for /be/ or alt
git commit -m "$(cat <<'EOF'
fix: use locale-neutral App Store links and real image alts

EOF
)"
```

---

### Task 5: Align root `support.html` + delete unused Freelancer stock art

**Files:**
- Modify: `support.html` (root — currently bare unstyled HTML)
- Delete unused template images under `assets/img/portfolio/` that are **not** referenced by any HTML: `cabin.png`, `cake.png`, `circus.png`, `game.png`, `safe.png`, `submarine.png` (keep the two WordSlug2 screenshot PNGs)
- Optional cleanup: `assets/favicon-old.ico`, `assets/img/avataaars.svg` if `rg` shows zero references

**Interfaces:**
- Consumes: none from prior tasks beyond stable privacy/support paths
- Produces: root support page themed like `memsmith/support.html` but for Word Slug 2 account deletion / contact; smaller unused assets

- [ ] **Step 1: Confirm which portfolio assets are referenced**

```bash
rg -n 'cabin\.png|cake\.png|circus\.png|game\.png|safe\.png|submarine\.png|avataaars|favicon-old' --glob '*.html'
```

Expected: no HTML references (or only commented). Safe to delete unreferenced files.

- [ ] **Step 2: Rewrite `support.html`**

Replace bare HTML with a Freelancer-themed page matching `memsmith/support.html` structure:

- `lang="en"`, viewport, meta description for Word Slug 2 support
- Shared CSS/JS/fonts/FA kit paths relative to repo root (`css/styles.css`, `js/scripts.js`, `assets/favicon.ico`)
- Nav brand → `index.html` (“Word Slug 2”)
- Body sections: Account deletion (point to `ws/word-slug-2-account-deletion.html` and/or mailto `alterededges@gmail.com` with subject guidance), Additional support mailto
- Footer with Privacy → `privacy-policy.html`, Home → `index.html`, © 2026

Keep content concise; do not invent new legal claims.

- [ ] **Step 3: Delete unreferenced stock assets**

```bash
rm -f assets/img/portfolio/cabin.png assets/img/portfolio/cake.png \
  assets/img/portfolio/circus.png assets/img/portfolio/game.png \
  assets/img/portfolio/safe.png assets/img/portfolio/submarine.png
# only if Step 1 showed zero refs:
rm -f assets/favicon-old.ico assets/img/avataaars.svg
```

- [ ] **Step 4: Final verification gate**

```bash
rg -n 'Mindful Month|apps\.apple\.com/be/|wordslug2' gb/ --glob '*.html' || true
rg -n 'Mindful Month' --glob '*.html'
rg -n 'apps\.apple\.com/be/' --glob '*.html'
rg -n 'alt="\.\.\."' index.html ws/index.html gb/index.html original-word-sluggers.html ws/original-word-sluggers.html
python3 /tmp/check_html_stack.py
test ! -f privacy_policy.html && echo OK_underscore
ls assets/img/portfolio/
```

Expected:

- No Mindful Month anywhere
- No `/be/` Apple links
- No `alt="..."` on the listed marketing pages
- HTML stack OK for Task 3 file list
- Underscore privacy gone
- Portfolio folder only has the two WordSlug2 screenshot PNGs

- [ ] **Step 5: Commit**

```bash
git add support.html
git add -u assets/
git commit -m "$(cat <<'EOF'
fix: theme root support page and remove unused Freelancer stock art

EOF
)"
```

---

## Done when

All five tasks committed on `fix/public-content-hygiene`, final verification gate green, ready for human review / PR to default branch. Do not push unless the user asks.
