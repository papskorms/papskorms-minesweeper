# Testing strategy

No test framework or commands are selected yet. Do not add or run tests unless the user asks for verification. When verification is requested, choose checks appropriate to the implemented architecture and report what ran.

## Required coverage before public release

- Board generation: exactly 40 mines per grid, safe zero start, reproducible saved state, and cross-boundary clues.
- Rules: revealed-tile no-op, flag toggle, mistake lockout and 3-second auto-flag, three-life game over, grid completion, and orthogonal unlocks.
- Modes/time: Default has no countdown; Time Attack starts on first reveal, continues through pause/page closure, resets after solve, and timeout wins simultaneous resolution.
- Persistence: separate mode saves, resume fidelity, save on quit, local score persistence, imported-save validation, and storage unavailable/full toast.
- UI: tutorial open/close and first-run behavior, pause menu actions, game-over board review, red flags and number colors in both themes, reduced-motion preference persistence.
- Deployment: public GitHub Pages page loads and asset paths work from the project subpath.

## Verification report

For requested verification, report commands/checks, pass/fail outcomes, browser/device scope, and known untested cases. Never describe a planned check as a completed test.
