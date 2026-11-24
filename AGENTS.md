# Agent context: jamesethridge.com

## Work mode

Exploration (design and planning). Checkpoint commits on a `planning/<slug>`
branch, squash-merge to `main` when a plan is approved. Conventions come from
`~/projects/engineering-practice` (ADR 0002, git workflow practice).

## Read first

1. `docs/design-brief.md` for the visual direction and the decisions already made.
2. `plans/handoff.md` for the current state and the next ordered actions.

## Rules

- **Facts come from `~/projects/curriculum-vitae`.** Resume and CV content is
  the single source of truth there. Never invent career facts in this repo.
  Mark placeholders in square brackets.
- **Employer content rules apply.** No customer names, internal URLs, coworker
  names, or credentials. The employer name appears only where a resume requires it.
- **Design tokens are fixed.** The Dracula palette and its semantic roles in
  `docs/design-brief.md` do not change without a recorded decision.
- **Prototypes are standalone HTML.** One file, no build step, fonts from Google
  Fonts only, no other external resources.
- **Design canvas.** The live canvas is
  https://claude.ai/artifact/TRuAqofEhxja5YHQSnxniR. Exports in `design/canvas/`
  are reference copies; the canvas is the editing surface.
