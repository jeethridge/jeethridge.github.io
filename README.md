# jamesethridge.com

Design workspace for the personal site at jamesethridge.com: a living resume, a
career run log, a skills tech tree, a lab notebook of articles, and a projects
page. The repo holds design decisions, design canvas exports, and motion
prototypes. It holds no site build yet. Tag `legacy-jekyll-2015` holds the
retired Jekyll site, including the old about page.

## Layout

| Path | Contents |
|---|---|
| `AGENTS.md` | Entry point for agent sessions: work mode, rules, where things live |
| `docs/background-notes.md` | Tunables, critique log, ranked ideas, and the verification routine for the background |
| `docs/design-brief.md` | The visual direction, color roles, type, motifs, information architecture, open questions |
| `design/canvas/` | Exports of the design canvas artboards (`.dc.html`) and their index |
| `prototypes/` | Standalone HTML motion prototypes: `background/` (v1), `substrate/` (v2, superseded), `fruiting-block/` (current) |
| `plans/handoff.md` | Resume handoff for the next session |

## Prototype

Open `prototypes/fruiting-block/index.html` in a browser. It needs no build step.
The page loads JetBrains Mono from Google Fonts and runs on a 2D canvas. A seed
in the URL hash (`index.html#7f3a`) replays one garden. With no hash, the seed
is today's date.

## Content source

Resume and CV (curriculum vitae) facts live in the `curriculum-vitae` repo. This
repo copies no facts. Placeholders in mockups are marked in square brackets.
