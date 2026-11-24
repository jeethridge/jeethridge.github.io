# Resume handoff: site design

Written 2026-11-24 for a fresh session with no prior context.

## Objective of the next session

Tune the background prototype `prototypes/fruiting-block/index.html` until
the user says it is right, then start the foreground. The user's standing
critique and the ranked ideas are in `docs/background-notes.md`. Read that
file and `docs/design-brief.md` before touching code.

## Branch state

- Repo: `~/projects/jeethridge.github.io`, remote `origin` on GitHub
  (`jeethridge/jeethridge.github.io`).
- `main`: the scaffold commit on top of the Jekyll history. Pushed. The
  remote default branch is still `master`.
- `planning/site-design`: all design work. Pushed, tracking
  `origin/planning/site-design`. Nothing uncommitted.
- Tags, both pushed: `legacy-jekyll-2015` (the retired Jekyll site) and
  `checkpoint-2026-11-24-pockets` (the fruiting block before the smoothing
  pass).
- Published copies: fruiting block
  https://claude.ai/artifact/ABsSSFFQttPcM38vC4qP1S, design canvas
  https://claude.ai/artifact/TRuAqofEhxja5YHQSnxniR. The artifact copy is a
  convenience. The repo file is the source.

## Resume commands

```sh
cd ~/projects/jeethridge.github.io
git checkout planning/site-design
git pull
git status --short --branch
git log --oneline -8
xdg-open prototypes/fruiting-block/index.html
```

`git status` must print a clean tree on `planning/site-design`.

## Editing and checking the prototype

The prototype is one standalone HTML file. Edit it in place. After an edit,
extract the script and syntax-check it:

```sh
node -e 'const fs=require("fs");const s=fs.readFileSync("prototypes/fruiting-block/index.html","utf8");fs.writeFileSync("/tmp/fb.js",s.match(/<script>([\s\S]*?)<\/script>/)[1]);' && node --check /tmp/fb.js
```

Then open the file in a browser and follow "How to verify a change" in
`docs/background-notes.md`. Commit each accepted change as a checkpoint with
a conventional-commit subject. Push the branch at the end of the session
with the user's confirmation.

## Nothing half-finished

Every item below is done or not started. No partial edits exist.

Done:
- Design brief with fixed Dracula roles and recorded decisions.
- Five design canvas artboards exported to `design/canvas/`.
- Background prototype v1 (2-D layers, checkpoint) and v2 (superseded).
- Background prototype v3, the fruiting block, through the smoothing pass.
- Background notes with tunables, critique log, ranked ideas, verification.

Not started:
- Further background tuning against the user's critiques.
- Foreground: navigation, section pages, content, mark as favicon.
- Static site generator choice and scaffold.
- Hosting and DNS. The domain is parked at Namecheap.

## Hard constraints carried forward

- `docs/design-brief.md` holds the decisions. Do not reopen palette, type,
  the roguelite-in-structure call, or the 3-D simulation direction without a
  new recorded decision.
- Facts come from `~/projects/curriculum-vitae`. This repo copies none.
- No push and no PR without the user's confirmation in the session. A push
  of `main` can replace the live GitHub Pages content, if Pages is enabled.
- Prototypes stay single-file HTML with fonts from Google Fonts only.
- Git rules from the user's global instructions: no AI attribution trailers,
  subject-only commit messages.

## Next actions, ordered

1. Open the prototype and ask the user for one word each on: jumble, kinks,
   and whether a pocket's organic-to-circuit transformation reads.
2. Apply the first ranked idea that matches the critique from
   `docs/background-notes.md`. Verify. Commit as a checkpoint.
3. Repeat step 2 until the user says the background is right. Record each
   accepted change in the critique log and the decisions table.
4. Decide open question 1 in the brief (code block or prose) and record it.
5. Decide the static site generator (open question 2) and scaffold it on
   this branch.
6. Build the foreground sections in the order listed at the end of
   `docs/background-notes.md`.
7. On a later push, with the user's confirmation, set the GitHub default
   branch to `main`.
