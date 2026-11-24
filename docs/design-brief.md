# Design brief: jamesethridge.com

**Decision so far:** the site is a terminal-styled personal site in the Dracula
palette, set in JetBrains Mono, with a slowly animated background in which
circuit traces morph into fungal hyphae. The roguelite framing lives in the
information architecture (runs, tech tree, notebook), not in pixel art. The
tone target is soothing and liminal, not threatening.

Status: direction chosen 2026-11-24. Refinement in progress. Open questions at
the end.

## Goal

A living resume plus articles that reads as taste and judgment first, with
some fun. The audience is hiring managers and referrers at Senior and Staff
level who search the name. The site must say "a nerd who likes science and
research" without saying it.

## Concept

- **Runs.** Each role is one run. A run has floors (projects), items
  (technologies picked up), and a boss (the hardest problem). Copy stays
  professional. The frame is the game.
- **Tech tree.** Skills form a graph. Each node records the run and floor
  where it unlocked. An unlocked node sprouts one leaf.
- **Notebook.** Articles are lab entries with a status: hypothesis, result,
  or abandoned. Abandoned entries stay public with a one-paragraph "why it
  stopped".
- **Seed.** A date-based seed drives one generative element per page. The
  seed shows in the footer. A visitor can share a seed URL. "Regrow" picks a
  new seed.
- **Colophon.** A page on fonts, generator, and the seed algorithm.

## Visual direction

### Palette: Dracula, roles fixed

The palette is the user's kitty terminal theme. Each color has one job.

| Token | Hex | Role |
|---|---|---|
| bg | `#282A36` | page ground |
| panel | `#21222C` | terminal panel, cards |
| line | `#44475A` | borders, inactive tabs |
| fg | `#F8F8F2` | primary text |
| secondary text | `#9AA3C9` | labels, captions (Dracula comment `#6272A4` fails contrast on bg, so it is decor only) |
| purple | `#BD93F9` | nav, keywords, circuit trace |
| cyan | `#8BE9FD` | links, italics, tendril (the morph midpoint) |
| green | `#50FA7B` | alive, unlocked, leaf |
| orange | `#FFB86C` | numbers, seed |
| pink | `#FF79C6` | cursor, bud |
| red | `#FF5555` | abandoned |

### Type

JetBrains Mono for everything. The user's terminal font is JetBrains Mono Nerd
Font Mono 14pt. Nerd Font glyphs do not load from Google Fonts, so icons are
inline SVG (Scalable Vector Graphics). Italic is cyan, the Dracula convention.
One italic line per page carries softness. No serif.

### Motif: trace to hypha

A line starts as a PCB (printed circuit board) trace: purple, straight runs,
45° and 90° turns, via pads at the turns. Along its length it becomes cyan and
starts to curve. It ends green as a hypha with a pink bud. No leaves: the
user rejected them in the background on 2026-11-24. The leaf stays only in
the logo mark and the tech tree node. The morph point moves with the seed.
The same motif appears as:

- the background animation,
- a section divider (trace on the left, vine on the right),
- a logo mark (leaf outline with trace veins and a pink pad at the stem),
- tech tree node states (locked pad, dashed researching, unlocked with a leaf).

### Foreground: terminal overlay

Content sits in one translucent terminal panel (panel color at ~0.74 alpha,
backdrop blur) with a kitty-style tab bar: sections as numbered tabs, the
active tab in fg on bg. The home summary is a syntax-highlighted code block.
Assumption: the code block stays on the home page only. Other pages use prose
in the same colors.

### Background: depth and slow motion

Two prototypes exist. Both share the terminal overlay, the parallax, and the
depth-of-field treatment: blurred far and near layers around a sharp focal
plane, pointer tilt by depth, and self-drift with no pointer.

**v1, breathing field** (`prototypes/background/index.html`, checkpoint).
Three layers. Hyphae spawn at edges and in the interior, grow at about 11 to
20 px per second at mid depth, fade from the tail, and the scene breathes
between silicon and mycelium on a ninety-second clock. The clock is global,
so the morph is a function of time.

**v2, substrate network** (`prototypes/substrate/index.html`, current
direction). Four layers. A seeded 3-D value-noise field plus two or three
nutrient hotspots defines nutrient density per layer. Hyphae enter from two
or three edge points per layer and steer up the nutrient gradient. Local
density sets the growth style: sparse ground grows as circuit traces with 45°
turns and vias, rich ground grows organic and branches often. Tips deplete
the field as they pass, so the network spreads. A tip that reaches another
strand fuses with it and stops. When every tip starves or fuses, the network
holds, fades over fourteen seconds, and a new generation grows from a
mutated seed. The morph is a function of place, not time.

Reduced-motion users get one fully grown network, no fade, no parallax.

## Information architecture

| Section | Content |
|---|---|
| home | summary, run log tail, substrate meter |
| run-log | one run per role, floors, items, boss |
| tech-tree | skills graph with unlock provenance |
| notebook | articles with status chips |
| projects | glmerge, the Translational Psychiatry paper, side quests |
| colophon | how the site is built |
| cv.pdf | the current resume PDF from the `curriculum-vitae` repo |

## Decisions recorded

| Date | Decision | Reason |
|---|---|---|
| 2026-11-24 | Terminal direction over warm paper (board A) and pixel atlas (board C) | Pixel art leans too far into RPG. Terminal fonts and Dracula are part of the user's style. |
| 2026-11-24 | Dracula tokens with fixed semantic roles | Coherence across pages and the animation. |
| 2026-11-24 | JetBrains Mono only, no serif | Matches the terminal. One italic line replaces the serif accent. |
| 2026-11-24 | Roguelite in structure, not in visuals | Taste signal for a Senior/Staff audience. |
| 2026-11-24 | Secondary text `#9AA3C9`, not `#6272A4` | Contrast ratio 5.7:1 versus 3.0:1 on bg. |
| 2026-11-24 | No leaves in the background animation | The user did not like them. Buds and pads carry the organic and circuit ends. |
| 2026-11-24 | Growth follows a nutrient field (v2) rather than a time clock (v1) | The user asked for a 3-D substrate, edge-seeded growth along a gradient, and a style set by local density. |
| 2026-11-24 | More depth: four layers, parallax up to 42 px, near layer blurred at 3.6 px | The user asked for more depth and more blur on the nearest strands. |

## Open questions

1. Keep the code-block summary on the home page, or switch to prose?
2. Static site generator: Astro or Eleventy. Both render Markdown from the
   `curriculum-vitae` repo. Undecided.
3. Hosting: GitHub Pages or Cloudflare Pages. The domain is at Namecheap and
   parked. This repo is the GitHub Pages repo for the account. Tag
   `legacy-jekyll-2015` holds the old Jekyll site with its CNAME for
   jamesethridge.com, and its about page holds bio material on embedded and
   music-product work for the projects page.
4. Does the background animation run on article pages, or only on home?
5. Does the mark need a light-on-dark variant for the favicon at 16 px?
