# Background notes: the fruiting block

The background is a 3-D simulation of a mycelial network colonizing a voxel
block of nutrient. This note is the working record for tuning it: the knobs,
what each critique changed, what to try next, and how to verify a change.
The visual decisions themselves live in `design-brief.md`.

File: `prototypes/background/index.html` is v1 (checkpoint, 2-D layers).
`prototypes/substrate/index.html` is v2 (superseded). The current prototype
is `prototypes/fruiting-block/index.html`. Published copy:
https://claude.ai/artifact/ABsSSFFQttPcM38vC4qP1S

## The algorithm in one screen

1. Build a 28³ grid of nutrient: a floor of 0.55 plus 3-D value noise, with
   three to five Gaussian pockets of low nutrient carved out.
2. Place six spores on the block's twelve edges. Each starts two tips.
3. Each frame, each tip: reads nutrient `d` and an organic measure
   `o = smoothstep(0.25, 0.8, d)`. Steers by negative autotropism (away from
   the hyphal-density gradient sampled 2.5 voxels ahead), chemotropism (up the
   nutrient gradient), and a slow wander. Extends at a sigmoid rate in `d`.
   Each run is organic (smooth steering) or circuit (a fixed lattice
   direction) with circuit probability (1 − o)². Corners sharpen as `d` falls.
4. Lateral branches leave at 65° ± 14°. The internode distance shortens with
   nutrient. Tips deplete nutrient as they pass. Tips fuse on contact with
   probability 0.35. Tips starve below 0.03 after a grace period.
5. Render: perspective camera, seven depth-of-field canvases blurred by
   distance from the focal plane. Each hypha is a smooth curve through the
   midpoints of its stored points, drawn twice (dark wide, bright narrow).
   Far bins fog toward the background. Width grows with age (cords). The
   substrate is faint dust only.
6. When all tips stop, the block holds eight seconds, fades over fourteen,
   and a new generation grows from a mutated seed.

## Tunables

All constants sit under the `tunables` comment near the top of the script.

| Name | Value | What it changes |
|---|---|---|
| `N` | 28 | voxels per axis; cost grows as the cube |
| `BASE_NUTRIENT` | 0.55 | floor of the rich substrate |
| `POCKETS_MIN`, `POCKETS_MAX` | 3, 5 | low-nutrient pockets per generation |
| `SPORES` | 6 | entry points on the block edges |
| `MAX_TIPS` | 40 | live tip cap; the main density knob |
| `VMAX` | 0.06 | extension rate at full nutrient, world units per second |
| `K_RATE` | 0.35 | half-saturation of the rate sigmoid; higher makes pockets slower |
| `STEP_LEN` | 0.014 | distance between stored points; smaller is smoother and costlier |
| `FUSE_R`, `FUSE_P` | 0.016, 0.35 | anastomosis radius and probability |
| `STARVE` | 0.03 | nutrient below which a tip stops |
| `W_CHEMO` | 1.0 | pull up the nutrient gradient |
| `W_AUTO` | 2.6 | push away from existing hyphae; the spreading knob |
| `W_WANDER` | 0.18 | random walk; higher is wigglier |
| `TURN_RATE` | 1.1 | organic reorientation rate; lower is straighter |
| `INTERNODE_RICH`, `INTERNODE_POOR` | 0.11, 0.42 | branch spacing by nutrient |
| `BRANCH_ANGLE`, `BRANCH_SPREAD` | 65°, 14° | lateral branch angle |
| `GROW_LIMIT`, `HOLD`, `FADE` | 120, 8, 14 s | generation timing |
| `DIST` | 2.25 | camera distance; lower is stronger perspective |
| `FILL` | 0.44 | block half-size as a fraction of the longer window side |
| `IDLE_ORBIT`, `POINTER_ORBIT` | 0.06, 0.24 rad | camera motion |

Rendering constants near the render code: `BIN_EDGE` (depth bin boundaries),
`BIN_ALPHA`, `BIN_FOG`, the CSS blur per bin (`.b0` to `.b6`), the tube
width formula `(1.4 + 0.6 * o) * cord`, and the cord growth `smooth(0, 60, age)`.

## Critique log

| Date | The user said | What changed |
|---|---|---|
| 2026-11-24 | Field looks like it moves, not three-dimensional | v2 layers replaced by a true 3-D block with a perspective camera |
| 2026-11-24 | Do not like the cube edges, liked the dots, perspective should do the work | Outline removed, dust kept, camera moved closer |
| 2026-11-24 | Growth does not look natural, base it on a model | Neighbour-sensing model: autotropism, chemotropism, lateral branching, anastomosis, Monod extension |
| 2026-11-24 | Strands should look more 3-D, like mycelium colonizing a block | Two-pass tubes, fog by depth, cords by age |
| 2026-11-24 | Make the substrate rich with pockets of low density | Field inverted: rich floor with Gaussian pockets |
| 2026-11-24 | Too much of a jumble, a line chasing a dot, kinks, more organic, pockets slower, transformation more apparent | Tips cut to 40, tip dot removed, look-ahead autotropism, midpoint curves, per-run circuit mode with sharpening corners, sigmoid rate |

The user's last word: "a good start". Not yet satisfied.

## Ideas not yet tried, ranked

1. **Translocation and real cords.** Boswell's model moves internal substrate
   along hyphae. Cords would thicken on trunks that carry flow, not by age.
   The current age rule thickens every old hypha equally.
2. **Front legibility.** The colonization front is the most natural cue of
   growth. A slightly brighter last few points per tip, fading with distance
   from the tip, would show the front without a dot.
3. **Pocket transformation.** Add small right-angle stubs and pads along
   traces inside pockets, and let the tube width step down at the first
   circuit run, so the change of substance reads at a glance.
4. **Block placement.** The terminal panel hides the block center. Options:
   shift the block center upward or sideways, or make the panel a little more
   transparent while the colony is young.
5. **Depth ordering inside a bin.** Strokes inside one bin draw in group
   order, not depth order. Painter's order per bin would fix rare overlaps.
6. **Time scale.** Full colonization takes about two minutes. An ambient mode
   after the first colonization could slow growth further and hold longer.
7. **Mobile and reduced motion.** Not yet checked on a phone or with reduced
   motion enabled. The reduced-motion path runs 1200 warm-up ticks on load,
   which may be slow on a phone.
8. **WebGL.** Only if the canvas path cannot hold frame rate at the target
   density. Current guard: render every other frame above 8000 points.

## How to verify a change

1. Open the file in a browser. The first frame must show growth, not an
   empty block (twelve seconds of warm-up run before the first frame).
2. Watch for ninety seconds. Tips, fusions, and hyphae counts in the card
   must rise, and the phase must reach "fully colonized" then "fading".
3. Open the console. There must be no errors.
4. Toggle "orbit" and move the pointer. The block must turn as one body.
5. Run `node --check` on the extracted script if the page shows nothing.
   The assembly step in `plans/handoff.md` does this.

## After the background: the foreground

The foreground is the terminal panel and the six sections. Not started.
Open decision 1 in `design-brief.md` (code block or prose on the home page)
comes first. Then: the kitty tab bar as real navigation, the run-log page
(one run per role, from the `curriculum-vitae` repo), the tech-tree page
(nodes with unlock provenance, the leaf-on-unlock motif from the design
canvas), the notebook (entries with hypothesis, result, abandoned), projects,
colophon, and the mark as favicon. The static site generator choice (open
decision 2) gates all of it.
