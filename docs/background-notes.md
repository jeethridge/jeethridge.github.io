# Background notes: the fruiting block

The background is a 3-D simulation of a mycelial network colonizing a voxel
block of nutrient. This note is the working record for tuning it: the knobs,
what each critique changed, what to try next, and how to verify a change.
The visual decisions themselves live in `design-brief.md`.

File: `prototypes/colonization/index.html` is v4, the current prototype.
`prototypes/fruiting-block/index.html` is v3 (checkpoint). Its published copy
is https://claude.ai/artifact/ABsSSFFQttPcM38vC4qP1S.
`prototypes/background/index.html` is v1 (checkpoint, 2-D layers).
`prototypes/substrate/index.html` is v2 (superseded).

## v4: the reference model in site style

v4 runs the reference colonization model unchanged. The reference lives in
the `hjp` harness workspace at
`initiatives/personal-site/docs/design/mycelium-colonization-3d.html`, with
its model notes beside it. The code between `// SIM-BEGIN` and `// SIM-END`
in v4 must stay byte-identical to the reference. Check it with a `diff` of
that range in both files.

What v4 changes is the renderer and the page:

- Hyphae are WebGL 2 ribbons without end caps. Translucent caps overlap at
  joints and bead the strand.
- Color: the `color hyphae by` select picks one of five modes.
  - **Connectivity** (default). Each spawn grain grows in its own color:
    purple, cyan, orange, pink, yellow. When two networks fuse, a ring of
    light runs out from the join through the joined network. The smaller
    network then eases into the larger one's color over about 2 s. When
    every grain belongs to one network, the colony turns green. The model
    keeps no grain per hypha, so the renderer infers it. A new segment starts
    at its tip's last committed point, and a branch names its parent tip. A
    fusion segment ends at the exact midpoint of the segment it hit.
  - **Ground nutrient.** The starting nutrient rank at the segment start,
    between the 5th and 95th percentiles: purple poor, cyan middle, green
    rich.
  - **Natural**, **time of formation**, and **branch order**, from the
    reference renderer, in Dracula colors.
  - In connectivity and ground nutrient, new growth starts near `fg` and
    fades to its color over 2.5 h.
- Depth: substrate opacity sets how much light the substrate takes between
  the camera and each hypha. That loss dims the hypha and blurs it by up to
  `SCATTER` pixels, so blur grows with depth in the substrate. At opacity 0,
  every hypha is sharp. There is no focal plane.
- Camera: `ZOOM` 1 makes the block's projection cover the frame on both axes
  at the base view. The default 2 is the wheel's maximum, so the view sits
  well inside the block.
- Camera control: no parallax. A drag on the background around the panel
  rotates the block, and the wheel there zooms between 0.4× and 2×. The zoom
  slider follows the wheel. The slow orbit pauses during a drag. Touch drag
  is not tested.
- Inoculation: five spawn grains mixed through the block by default. On
  seed `20261125` they join at 5.0, 6.4, 7.0, and 8.3 simulated hours.
- Warm-up: 1 simulated hour before the first frame, so the grains show
  apart for about 16 s before the first join.
- Substrate: dust points, green in rich ground and purple in poor ground.
  Dust fades as nutrient is consumed. There is no block outline and no
  ray-marched volume.
- Tips: small pink points.
- Live heads: each frame draws one ribbon per tip, from its last committed
  point to the tip. Between model steps, each tip moves forward along its
  last step of motion. Without this, a strand grows in 0.04 mm chunks and
  stands still on most frames.
- Timing: 0.25 simulated hours per second, so about four minutes per
  generation. Then hold 8 s, fade 14 s, and regrow from the next seed.
- Controls: tab `7 substrate` docks the panel left and exposes substrate
  opacity, zoom, speed, orbit speed, tips, the six growth parameters (live), and
  inoculation and patchiness (both restart the block). Tabs do not change
  the URL hash, because the hash holds the seed.
- Saved settings: each change in the tab saves to `localStorage` under
  `substrate-settings` at once. Wheel zoom saves too. Only settings that
  differ from the site defaults are stored, with a version number. The saved
  settings load before the warm-up. Camera rotation is not saved. The hash
  keeps the seed only. "reset to site defaults" clears the store. "copy as
  defaults" copies the two site-default lines at the top of the tunables
  block. To promote a setup, paste those lines over the same two lines.
  A first-time visitor gets the site defaults, and nothing is stored until a
  control changes.

v4 tunables sit under the `tunables` comment in the second script block. The
v3 tunables table below applies to v3 only.

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

| 2026-11-25 | Not happy with v3. Built a reference model with Fable, happy with the core model, adapt it to the site style | v4: reference model unchanged, Dracula renderer, motif in color only |

| 2026-11-25 | Likes v4. Expose controls in a tab, slower, spawn on the top face, zoom in so the block is slightly larger than the frame, blur should follow depth in the substrate | Substrate tab, 0.3 h/s, top inoculation, cover-fit zoom 1.1, scattering blur set by substrate opacity in place of defocus |

| 2026-11-25 | Seed growth from several spots, one color per subnetwork, and a color change when they connect. Keep the reference color modes, with this one as the default. Shorten the warm-up so the colors last longer | Mixed inoculation by default, connectivity color mode with join rings, `color hyphae by` select with five modes, warm-up 3 h to 1 h |
| 2026-11-25 | Remove the parallax. Grab and rotate the block with the mouse, zoom with the wheel. Then: "much more comfortable than before" | Pointer tilt, idle drift, and panel shift removed. Drag rotates, wheel zooms |
| 2026-11-25 | Save simulation settings across reloads, then promote favorites to the site defaults | Autosave to `localStorage` (only settings that differ from the defaults), reset button, "copy as defaults" snippet. Site defaults gathered into two lines |
| 2026-11-25 | Promoted tuned settings to the site defaults | `vmax` 0.26, patchiness 65%, 0.25 h/s, zoom 2, substrate opacity 51% |
| 2026-11-25 | A control for orbit speed in place of the checkbox, to make the orbit slower | `orbit speed` slider, 0 to 10°/s, default 2.9°/s (the former 0.05 rad/s). 0 stops the orbit. The footer `orbit` button stays the on/off toggle. `ORBIT_SPEED` joins the site-default line |

The user's last word on v3: "a good start". On v4: "I like this".

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
