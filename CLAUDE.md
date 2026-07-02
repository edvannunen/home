# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Context

This is **Home**, the landing page for `bier-en-brood.nl`. It's one of three sibling projects deployed on the same Hetzner + Coolify server — see `../Fietsen/CLAUDE.md` for the full family overview (De Sprong, Fietsen, Home) and shared decisions (path-based routing, local dev conventions, the planned shared-login gate in Part 3).

## What this is

A single static page, no build step, no framework: `index.html` + `style.css`, plain HTML/CSS. It shows the "Bier & Brood" banner and two large tiles that link out to the other two apps:
- "De Sprong" → `/de-sprong`
- "Fietsen" → `/fietsen`

Design assets live in `img/`. Only `banner.png`, `fietsen_trans.png`, and `sprong_klein_trans.png` are used by the page and tracked in git — `img/Backup/`, `img/[Originals]/`, `banner_groot.png`, and the standalone Strange Brew logo are working/source files, gitignored.

## Local dev

`npx serve .` from this folder. The tile links point at the **production** relative paths (`/de-sprong`, `/fietsen`), so they'll 404 locally unless De Sprong/Fietsen happen to be reachable at those exact paths — same trade-off Fietsen's Part 1 documents. To test a link locally, temporarily point it at whatever port that app's dev server is using (e.g. `http://localhost:5173/de-sprong` for De Sprong's `vite dev`).

## Deploy (Coolify) — done, live at `bier-en-brood.nl`

- Application: Build Pack `Static`, Base/Publish directory `.` (repo root — no subpath, so none of Fietsen's Nginx alias/`<base>` workarounds are needed)
- Repo is public, connected via GitHub App (no deploy key needed, unlike Fietsen's private repo)
- Domain: `https://bier-en-brood.nl` (bare, no path prefix)

**Gotcha hit during setup**: the domain root was briefly owned by the De Sprong app (its Domains field had no `/de-sprong` path suffix), so it caught every unmatched request including `/`. Fixed by scoping De Sprong's domain to `https://bier-en-brood.nl/de-sprong` first, freeing the root for Home. See `../Fietsen/CLAUDE.md` for the full story, including the Coolify "Strip Prefixes" bug that briefly broke De Sprong after that change.
