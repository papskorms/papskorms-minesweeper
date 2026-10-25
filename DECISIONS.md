# Decisions and open questions

Use 01_REQUIREMENTS.md for approved behavior and 02_ARCHITECTURE.md for the selected technical direction. This file tracks rationale and unresolved items without duplicating full requirements.

## Approved product decisions

- Public source repository and GitHub Pages are approved.
- The world is an infinite orthogonal set of 16×16 grids; each grid has exactly 40 mines (15.625%) and clues count mines across grid boundaries.
- The start cell is selected at random from the four central cells when a run is created, is guaranteed safe with clue zero, and is saved with the run. Zero expansion stops at locked-grid boundaries.
- A grid is solved by revealing every safe cell; flags are optional; solving unlocks its orthogonal neighbors.
- There are 3 lives. A mine mistake animates for 3 seconds and blocks reveals and flags; zero lives or timer ends immediately. Default is untimed; Time Attack starts on the first reveal, resets to 60 seconds after a grid solve, and continues during pause and page closure.
- Browser-local saves and high scores are separate by mode. If storage is unavailable/full, the game shows a toast and continues in session-only mode.
- When a mode has no saved run, start a new game.
- Mouse: left reveals, right flags, left drag pans. Keyboard: WASD moves selection, arrows smoothly accelerate camera pan, Z reveals, X flags. Touch: tap reveals, 1.5-second hold flags, held drag pans.
- Game-over board review is temporary and does not restore after a browser restart.
- The tutorial uses a question-mark control, dimmed background, and X close button; it does not pause Time Attack; first-time status is per mode.
- The visual direction is rounded application panels/type and square cells; grids touch with dashed boundaries; locked grids are dimmed with locks; flags are red in both themes.
- Reduced-motion preference persists, is not inherited from browser/OS, and removes mistake-tile rumble.
- Score is one point for each newly revealed safe tile (including zero expansion); flags and mine mistakes score zero. Top ten per mode; no in-progress entries; ask on completion/quit about optional registration with username up to five letters; tie date favors newer.
- No backend, account system, cloud save, multiplayer, ads, additional modes, or online leaderboard is approved.

## Approved technical direction

- Plain HTML, CSS, and JavaScript ES modules without a framework/build step for the initial app.
- Initial browser target: Chrome, Opera, Brave; responsive common screen sizes.
- DOM controls for visible tiles, rendered only for the current viewport.
- Deterministic per-grid mine generation from a saved run seed and grid coordinates; store progress rather than duplicate mine layouts.
- IndexedDB for runs, preferences, and local score records.
- Static GitHub Pages publishing from the repository root on main.

## Open — resolve before dependent implementation

- After a non-final mistake animation is interrupted by page closure, auto-flag immediately on resume or continue remaining delay?
- Does theme selection persist across sessions?
- Does declining score registration omit the record or create an anonymous record? Allowed characters/case and alphabetical sort direction?
- Does confirming a conflicting import replace the existing mode save?
- Does pause Save write the active run or create a separate checkpoint? Is board input blocked while open?
- What browser versions/platforms and viewport range define initial support? What are selection boundary behavior and initial selection position?
- Cleared or unavailable storage may prevent recovery.
## Architectural consequences

- No multiplayer synchronization or cross-device recovery is available without a backend.
- Local scores are not tamper-resistant.
- Time Attack uses a wall-clock deadline so time advances while closed; system-clock changes may affect it.
- Finite browser storage can fill as explored progress grows; session-only fallback prevents a storage error from automatically ending a run but cannot preserve it after reload.