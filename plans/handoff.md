# Resume handoff: site design

Written 2026-11-25 for a fresh session with no prior context.

## Objective of the next session

The background is at a checkpoint the user likes: v4,
`prototypes/colonization/index.html`. Take further background critiques if
the user has any, then start the foreground. Read `docs/design-brief.md` and
`docs/background-notes.md` before touching code.

## Branch state

- Repo: `~/projects/jeethridge.github.io`, remote `origin` on GitHub
  (`jeethridge/jeethridge.github.io`).
- `main`: the scaffold commit on top of the Jekyll history. Pushed. The
  remote default branch is still `master`.
- `planning/site-design`: all design work. Pushed, tracking
  `origin/planning/site-design`. Nothing uncommitted.
- Tags, all pushed: `legacy-jekyll-2015` (the retired Jekyll site),
  `checkpoint-2026-11-24-pockets` (v3 before the smoothing pass), and
  `checkpoint-2026-11-25-colonization` (v4 at this handoff).
- Harness: the repo is a target of the `hjp` workspace
  (`~/harnesses/hjp`). Its `.harness` link and `CLAUDE.local.md` stay out of
  this repo's history. The reference model lives in that workspace at
  `initiatives/personal-site/docs/design/mycelium-colonization-3d.html`.
- Published copies: v3 fruiting block
  https://claude.ai/artifact/ABsSSFFQttPcM38vC4qP1S, design canvas
  https://claude.ai/artifact/TRuAqofEhxja5YHQSnxniR. v4 has no published
  copy. The repo file is the source.

## Resume commands

```sh
cd ~/projects/jeethridge.github.io
git checkout planning/site-design
git pull
git status --short --branch
git log --oneline -8
xdg-open prototypes/colonization/index.html
```

`git status` must print a clean tree on `planning/site-design`.

## Editing and checking the prototype

The prototype is one standalone HTML file. Edit it in place. After an edit,
run these three checks:

1. Model unchanged. This `diff` must print nothing:
   ```sh
   diff <(sed -n '/SIM-BEGIN/,/SIM-END/p' ~/harnesses/hjp/initiatives/personal-site/docs/design/mycelium-colonization-3d.html) \
        <(sed -n '/SIM-BEGIN/,/SIM-END/p' prototypes/colonization/index.html)
   ```
2. Syntax:
   ```sh
   node -e 'const fs=require("fs");const s=fs.readFileSync("prototypes/colonization/index.html","utf8");fs.writeFileSync("/tmp/v4.js",s.match(/<script>([\s\S]*?)<\/script>/)[1]);' && node --check /tmp/v4.js
   ```
3. Render: open the file in a browser and follow "How to verify a change" in
   `docs/background-notes.md`. For a headless check, Playwright's cached
   Chromium is at `~/.cache/ms-playwright/chromium-1234/chrome-linux64/chrome`.
   Launch it with `--use-angle=swiftshader` for WebGL 2.

Commit each accepted change as a checkpoint with a conventional-commit
subject. Push at the end of the session with the user's confirmation.

## Nothing half-finished

Every item below is done or not started. No partial edits exist.

Done:
- Design brief with fixed Dracula roles and recorded decisions.
- Five design canvas artboards exported to `design/canvas/`.
- Background prototypes v1 to v3 (checkpoints).
- Background v4: the reference model in a Dracula WebGL 2 renderer, the
  `7 substrate` controls tab, top-face spawn, cover-fit zoom, blur from
  depth in the substrate, and live hyphal heads for smooth growth.

Not started:
- Moving the model into a Web Worker. Do it only if the user sees hitches
  late in a generation. One model step costs up to about 5 ms.
- Foreground: navigation, section pages, content, mark as favicon.
- Static site generator choice and scaffold.
- Hosting and DNS. The domain is parked at Namecheap.

## Hard constraints carried forward

- `docs/design-brief.md` holds the decisions. Do not reopen palette, type,
  the roguelite-in-structure call, the 3-D simulation direction, or the
  reference model without a new recorded decision.
- The model code between `// SIM-BEGIN` and `// SIM-END` stays
  byte-identical to the reference. Visual changes go in the renderer.
- Facts come from `~/projects/curriculum-vitae`. This repo copies none.
- No push and no PR without the user's confirmation in the session. A push
  of `main` can replace the live GitHub Pages content, if Pages is enabled.
- Prototypes stay single-file HTML with fonts from Google Fonts only.
- Git rules from the user's global instructions: no AI attribution trailers,
  subject-only commit messages.

## Next actions, ordered

1. Ask the user whether the v4 background needs more tuning. Known open
   items: the controls panel overflows a 900 px-tall window, and a mature
   colony is bright behind the home panel.
2. Decide open question 1 in the brief (code block or prose) and record it.
3. Decide the static site generator (open question 2) and scaffold it on
   this branch.
4. Build the foreground sections in the order listed at the end of
   `docs/background-notes.md`.
5. On a later push, with the user's confirmation, set the GitHub default
   branch to `main`.
