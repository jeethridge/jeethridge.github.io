# Resume handoff: site design

Written 2026-11-24 for a fresh session with no prior context.

## Branch state

- Repo: `~/projects/jeethridge.github.io`, remote `origin` on GitHub.
  Nothing from this work is pushed.
- Tag `legacy-jekyll-2015`: the retired Jekyll site, local tag only.
- Tag `checkpoint-2026-11-24-pockets`: the fruiting block before the
  smoothing pass (per-run circuit mode, midpoint curves, no tip dot).
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
xdg-open prototypes/fruiting-block/index.html
```

`git status` must print a clean tree on `planning/site-design`.

## Nothing half-finished

Every item below is done or not started. No partial edits exist.

Done:
- Design brief with fixed Dracula roles and recorded decisions.
- Five design canvas artboards exported to `design/canvas/`.
- Background prototype v1 (checkpoint): three parallax canvas layers,
  trace-to-hypha growth, ninety-second substrate cycle, seeded by URL hash or
  date, terminal overlay with kitty tab bar, reduced-motion fallback. No
  leaves. Published copy: https://claude.ai/artifact/NE4pH9APV1cd47k8koXMno
- Background prototype v2 (superseded): four flat layers over a 2-D field.
  Published copy: https://claude.ai/artifact/WPS56o5hPdxdiN6k5xHMsh
- Background prototype v3 (current direction): one 3-D voxel block, spores
  on the block edges, neighbour-sensing growth (autotropism, chemotropism,
  lateral branching, anastomosis, Monod extension), density-driven
  circuit-or-organic style, perspective camera into seven depth-of-field
  canvases, two-pass tube rendering with fog and cords, dust-only substrate,
  hold-fade-regrow generations.
  Published copy: https://claude.ai/artifact/ABsSSFFQttPcM38vC4qP1S
- Design canvas: https://claude.ai/artifact/TRuAqofEhxja5YHQSnxniR

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

1. Open `prototypes/fruiting-block/index.html`, let one generation finish
   (about ninety seconds), and collect the user's reactions on: whether the
   block reads as a volume, growth speed, strand visibility, the
   circuit-versus-organic split, camera motion (idle orbit on or off), and
   fade timing.
2. Tune the constants under `tunables` in that file (`N`, `SPORES`,
   `MAX_TIPS`, `VMAX`, `K_NUTRIENT`, `W_CHEMO`, `W_AUTO`, `W_WANDER`,
   `TURN_RATE`, `INTERNODE_RICH`, `INTERNODE_POOR`, `BRANCH_ANGLE`, `FUSE_R`,
   `FUSE_P`, `STARVE`, `GROW_LIMIT`, `HOLD`, `FADE`, `DIST`, `FILL`,
   `IDLE_ORBIT`, `POINTER_ORBIT`) from those reactions. Commit as a checkpoint.
   The substrate is rich with low-nutrient pockets (`BASE_NUTRIENT`,
   `POCKETS_MIN`, `POCKETS_MAX`). If the pockets never fill, raise the floor
   of the extension rate or lower `STARVE`.
3. Decide open question 1 (code block or prose) and record it in the brief.
4. Decide the static site generator (open question 2) and scaffold it in
   this repo on the same branch.
5. On the first push, with the user's confirmation: push `main` and the tag,
   then set the GitHub default branch to `main`.
