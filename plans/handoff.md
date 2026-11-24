# Resume handoff: site design

Written 2026-11-24 for a fresh session with no prior context.

## Branch state

- Repo: `~/projects/jeethridge.github.io`, remote `origin` on GitHub.
  Nothing from this work is pushed.
- Tag `legacy-jekyll-2015`: the retired Jekyll site, local tag only.
- `main`: one new commit on top of the Jekyll history that removes the old
  site and adds the scaffold (`README.md`, `AGENTS.md`, `.gitignore`). The
  local branch is renamed from `master`. The remote default is still
  `origin/master`.
- `planning/site-design`: the design brief, canvas exports, the background
  prototype, and this handoff. All committed. Nothing uncommitted.

## Resume commands

```sh
cd ~/projects/jeethridge.github.io
git status --short --branch
git log --oneline --all
xdg-open prototypes/background/index.html
```

`git status` must print a clean tree on `planning/site-design`.

## Nothing half-finished

Every item below is done or not started. No partial edits exist.

Done:
- Design brief with fixed Dracula roles and recorded decisions.
- Five design canvas artboards exported to `design/canvas/`.
- Background prototype v1: three parallax canvas layers, trace-to-hypha
  growth, ninety-second substrate cycle, seeded by URL hash or date,
  terminal overlay with kitty tab bar, reduced-motion fallback.

Not started:
- Static site generator choice and scaffold.
- Content pages (run log, tech tree, notebook, projects, colophon).
- Hosting and DNS.

## Hard constraints carried forward

- `docs/design-brief.md` holds the decisions. Do not reopen palette, type,
  or the roguelite-in-structure call without a new recorded decision.
- Facts come from `~/projects/curriculum-vitae`. This repo copies none.
- No push and no PR without the user's confirmation in the session. A push
  of `main` replaces the live GitHub Pages content, if Pages is enabled.

## Next actions, ordered

1. Open the prototype in a browser and collect the user's reactions on four
   axes: growth speed, density, blur strength, overlay opacity.
2. Tune the constants at the top of the script in
   `prototypes/background/index.html` (`LAYERS`, `CYCLE`, `MORPH_SECONDS`)
   from those reactions. Commit as a checkpoint.
3. Decide open question 1 (code block or prose) and record it in the brief.
4. Decide the static site generator (open question 2) and scaffold it in
   this repo on the same branch.
5. On the first push, with the user's confirmation: push `main` and the tag,
   then set the GitHub default branch to `main`.
