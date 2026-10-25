# Project status

**Updated:** 2026-10-25

## Current state

- Public repository: https://github.com/papskorms/papskorms-minesweeper
- Source-of-truth documents and starter visual assets are committed on main.
- The technical direction is selected in 02_ARCHITECTURE.md.
- No game implementation exists yet.
- GitHub Pages is the approved hosting target but is not configured; configure it after a playable build exists.

## Next work

1. Resolve the remaining behavior questions listed in DECISIONS.md.
2. Implement and verify the game according to 01_REQUIREMENTS.md and 02_ARCHITECTURE.md.
3. Publish the playable static app through GitHub Pages.

## Risks and blockers

- Most input bindings, tutorial behavior, mode switching, score-list policy, and game-over timing are now specified; remaining questions are tracked in DECISIONS.md.
- Browser storage can fill or be cleared. Session-only fallback cannot restore a run after reload.
- Browser families and responsive intent are defined; versions, viewport range, and gesture edge cases remain open.