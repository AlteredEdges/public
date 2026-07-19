# Controller prompt — public site content hygiene (SDD)

Paste this into a **new Cursor agent chat** with the `public` repo (or multi-root workspace including `/Users/jeremybenetz/dev/github/public`) as context.

---

You are the **controller** for Subagent-Driven Development.

## Required skills (read and follow in order)

1. `superpowers:using-git-worktrees` — create/verify an isolated worktree for branch `fix/public-content-hygiene` from the public site repo default branch
2. `superpowers:subagent-driven-development` — execute the plan task-by-task (fresh implementer per task → task reviewer → fix loop → next task → final whole-branch review → finishing-a-development-branch)

Do **not** implement the HTML yourself in the controller context. Dispatch subagents.

## Plan file (source of truth)

`/Users/jeremybenetz/dev/github/public/docs/superpowers/plans/2026-07-18-public-content-hygiene.md`

Repo: `/Users/jeremybenetz/dev/github/public`  
Live site: https://alterededges.github.io/public/

## Scope

Execute **all five tasks** in the plan (Gym Bingo identity, privacy Mindful Month fix, footer markup, Apple `/be/` + alts, root support + dead assets).

**Out of scope (do not expand):** studio hub redesign, Bootstrap/Freelancer removal, image compression, custom domain, Eleventy/React, rewriting already-good `memsmith/` / `thump/` / `pj/` pages.

## Controller checklist

1. Pre-flight: scan the plan Global Constraints once; if anything conflicts, ask me in one batch before Task 1
2. Create/verify worktree + branch `fix/public-content-hygiene`
3. Init progress ledger at `.superpowers/sdd/progress.md` in the worktree
4. For each Task 1–5:
   - Run `task-brief` from the SDD skill scripts for that task number
   - Dispatch **implementer** with brief path + report path + Global Constraints (mechanical HTML → cheaper model OK; Task 2 privacy copy → standard model)
   - On DONE: `review-package BASE HEAD`, dispatch **task reviewer**
   - Fix Critical/Important findings; re-review until approved
   - Append ledger line; do not stop to ask “continue?”
5. After Task 5: final whole-branch code review on the full branch diff
6. Use `finishing-a-development-branch` — present merge/PR options; **do not push or open a PR unless I explicitly say so**

## Hard rules from the plan

- Never leave Word Slug store links on `gb/`
- Never leave “Mindful Month” in any HTML
- Never break existing Support/Privacy URL paths used by App Store Connect
- Canonical WS2 Apple URL: `https://apps.apple.com/app/id6448659885`
- Commit after each task; no commit on `main`/`master` without consent
