# Repository guide

## Project and layout
- Static browser demo that learns Flappy Bird through neuroevolution; all simulation and training run in the browser. There is no application backend, package manager, or build step.
- `index.html`: canvas, speed controls, and script loading. Load `Neuroevolution.js` before `game.js`.
- `game.js`: physics, collisions, rendering, scoring, and learning integration.
- `Neuroevolution.js`: feedforward networks and genetic selection, crossover, and mutation.
- `img/`: sprites; `README.md`: learning API and options; `License`: MIT license.

## Run locally
From the repository root:
```sh
python3 -m http.server 8000 --bind 127.0.0.1
```
Keep the server running and open http://127.0.0.1:8000/. Training starts automatically. Reloading resets the in-memory training state.

## Making changes
- Keep changes focused and follow the existing JavaScript style: tabs, `var`, and prototype methods. Avoid unrelated reformatting or adding frameworks/build tooling without a task requirement.
- Preserve browser compatibility and relative asset paths.
- The demo uses 50 networks with topology `[2, [2], 1]`. Inputs are normalized bird height and the next gap's upper boundary; output above `0.5` flaps. Fitness is survival ticks, not pipes passed.
- Update the README when changing public learning options or setup instructions.

## Verification
- There is no automated test suite or lint configuration in this repository.
- For behavior changes, serve the app and check that sprites render, birds move and collide, generations advance after all birds die, and speed controls work. Check the browser console for errors.
- For learning changes, verify population size, network inputs/outputs, and score recording across multiple generations. Outcomes are random; do not require a fixed score.
- Review `git diff --check` and the final diff; report which checks were actually performed.
