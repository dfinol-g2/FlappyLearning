# Neon orbital lab verification

Original sci-fi feature preview: `http://127.0.0.1:8002/`.
Combined feature verification: `http://127.0.0.1:8000/`.
Source visual truth: approved Neon orbital lab art direction supplied in the task
(navy `#07111F`, cyan `#53E4FF`, violet `#A78BFA`, white `#EAF6FF`, robotic
bird, station gates and quiet orbital background). No selected mockup image was
included in this chat, so exact image-to-image fidelity cannot be certified.

Implementation screenshot: `/tmp/orbital-lab-desktop.jpg`, 1000 x 900 pixels at
1000 x 900 CSS viewport, density 1. Also inspected 414px and 320px wide views.
State: live training, x3 selected in the saved desktop capture, keyboard focus on
the repository link. Returned preview to x1 and reset viewport override afterward.

## Findings and evidence

- Typography: system monospace throughout, clear heading hierarchy, all four
  canvas counters retained. Controls and labels wrap without page overflow at
  320px. Canvas telemetry scales with the original 500 x 512 logical canvas.
- Layout: centered compact 534px panel; original gameplay aspect ratio preserved.
  Observed document width equals viewport width at 320px and 1000px.
- Colors: opaque HUD backing prevents pipes obscuring the counters. Text contrast
  on navy: white 17.25:1, cyan 12.54:1, violet 6.96:1, secondary text 9.87:1.
- Assets: robotic cyan visor bird, metallic cyan gate caps and distant stations
  match the approved written direction. Transparent bird is 34 x 24; opaque pipe
  textures are 40 x 512; opaque background is 288 x 512. Rendered bird remains
  40 x 30 and gates remain 50px wide. No glow outside collision silhouettes.
- Tiling: visually inspected moving background at multiple offsets. Adjacent
  edge pixels differ by at most 3 RGB levels (mean 0.708), without a visible seam.
- Copy: four counters, original speed choices, repository link retained. Survival
  time is described accurately; no implementation details appear in the game UI.
- Interaction: clicked x1, x2, x3, x5 and MAX; exactly one aria-pressed speed at a
  time. Tab traverses all five buttons and the link; Enter and Space activate
  buttons; violet focus outline is clearly visible. Browser console: no warnings
  or errors. Sprites loaded; birds moved, collided and generations advanced.

Full-view and focused HUD/control inspections found no actionable visual issues
against the written brief. An exact source/implementation comparison was not
possible because the source mockup was not supplied. No false side-by-side or
pixel-fidelity comparison is claimed. No P0/P1/P2 fix iterations were required.

## Simulation checks

A temporary Node VM comparison used the baseline game and the changed game with
identical seeded random generators and the unchanged Neuroevolution.js. Seeds
1, 42 and 8675309 each ran 6000 ticks. Serialized game/neural state matched after
every tick (18000 total), spanning 84 generation transitions. Explicit touching
and separated pipe-edge checks and top/bottom canvas-boundary checks passed.
Display calls every 60 ticks left simulation state unchanged. All speed values
and selected states passed. `node --check game.js` and `git diff --check` passed.
This harness was temporary; no framework or test dependency was added.

## Pause/Resume integration

The combined feature adds native `button#pause-resume` to `.controls`
and retains its managed scheduler behavior. Shared `button`, `button:hover` and
`button:focus-visible` rules apply. `#pause-resume[aria-pressed="true"]` uses
restrained amber `#F2BE68` for the paused state. Speed state is scoped strictly
to `button[data-fps]`, so it does not clear the pause button's aria-pressed state.
Pause behavior is integrated and verified below.

## Raster provenance

Generated with the built-in imagegen tool, then fitted using macOS sips.
Final assets live in this worktree's `img/` directory. Prompt set:

- `img/bird.png`: one right-facing compact robotic Flappy Bird, orthographic
  side view, metallic white/slate body, cyan visor, violet segmented wing, tiny
  beak; palette above; readable at 34 x 24, transparent tight bounds, no aura,
  text, logos or other objects.
- `img/background.png`: horizontally seamless 288 x 512 portrait orbital tile;
  nearly uniform navy, sparse muted cyan/white stars, distant low-detail station
  silhouettes in the bottom quarter, quiet top 70%, dark edge strips, no text,
  bright nebula, birds or gates.
- `img/pipetop.png`: front-facing straight rectangular dark metallic station
  column, subtle panel lines, cyan illuminated cap flush to bottom, light inside
  cap, no exterior aura, perspective, text or scenery. Cropped to the column and
  fitted to 40 x 512. `img/pipebottom.png` is its vertical mirror.

## Checklist and limitation

- Implementation, asset, browser, keyboard and deterministic checks complete.
- Exact selected-mockup comparison remains unavailable; supply the original
  selected visual if strict image-to-image Product Design signoff is required.

## Combined feature verification

Verified on 2026-11-09 after integrating TCP-2 and TCP-3 from the shared
`workshop/base` commit `116cd51`. Merge resolution preserves both scheduling
and speed-button selection updates in `speed()`, and inserts the native
Pause/Resume button into the sci-fi controls. `AGENTS.md` and
`Neuroevolution.js` are unchanged from the shared baseline.

- `node --check game.js`, `node --check Neuroevolution.js`, and
  `git diff --check workshop/base..HEAD` passed.
- A temporary Node VM harness compared combined and baseline simulation state
  after every tick: seeds 1, 42 and 8675309, 6000 ticks each (18000 ticks and
  84 generation transitions). State matched exactly; rendering did not mutate
  simulation state. Pipe contact/separation and canvas-boundary checks passed.
- The harness passed all five speed selections, full simulation/neural-state
  stability while paused, speed changes while paused, exact-state resume
  without catch-up, stale MAX callback invalidation, and 100 rapid toggle
  cycles with one live update chain. Canceled MAX callbacks may remain queued
  until delivered; they do not advance the state.
- In the in-app browser, the combined sci-fi assets and all four counters
  rendered. Birds moved, collided, and generations advanced (generation 4
  before MAX, then 22 and 59). All five speeds selected correctly while
  running and paused. Tab reached Pause/Resume from MAX; Enter and Space
  toggled it, with a visible violet focus outline and amber paused state.
- Paused canvas state remained visibly unchanged for over five seconds at x1
  and MAX, including birds, pipes, background and counters. Changing speed
  kept the game paused. Resume continued movement and learning.
- Captured browser console reported no warnings or errors. Local screenshot:
  `/tmp/flappy-combined-paused.jpg` (verification artifact, not committed).
- Read-only subagent review of the combined candidate found no important
  correctness or integration findings. Its documentation note is resolved
  by this combined verification section.

Result: passed against the approved written art direction and combined feature
requirements. Exact image-to-image fidelity is not applicable: no source
mockup was supplied. Cross-browser/device coverage beyond the in-app browser
was not performed for this integration pass. VM harness and screenshots were
temporary verification artifacts, not a permanent automated test suite.
