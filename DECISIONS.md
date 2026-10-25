# Decisions and open questions

Use 01_REQUIREMENTS.md for approved behavior and 02_ARCHITECTURE.md for the selected technical direction. This file tracks rationale and unresolved items without duplicating full requirements.

## Approved product decisions

- Public source repository and GitHub Pages are approved.
- The world is an infinite orthogonal set of 16×16 grids; each grid has exactly 40 mines (15.625%) and clues count mines across grid boundaries.
- The start cell is selected at random from the four central cells when a run is created, is guaranteed safe with clue zero, and is saved with the run. Zero expansion stops at locked-grid boundaries.
- A grid is solved by revealing every safe cell; flags are optional; solving unlocks its orthogonal neighbors.
- There are 3 lives. A mine mistake animates for 3 seconds and blocks reveals. Default is untimed; Time Attack starts on the first reveal, resets to 60 seconds after a grid solve, and continues during pause and page closure.
- Browser-local saves and high scores are separate by mode. If storage is unavailable/full, the game shows a toast and continues in session-only mode.
- When switching to a mode with no saved run, show a choice; exact options remain open.
- Board navigation supports WASD, arrow keys, mouse drag, and touch drag.
- Game-over board review is temporary and does not restore after a browser restart.
- The tutorial uses a question-mark control, dimmed background, and X close button; it does not pause Time Attack.
- The visual direction is rounded application panels/type and square cells; grids touch with dashed boundaries; locked grids are dimmed with locks; flags are red in both themes.
- Reduced-motion preference persists and is not inherited from the browser/OS.
- Score is one point for each newly revealed safe tile (including zero expansion); flags and mine mistakes score zero.
- No backend, account system, cloud save, multiplayer, ads, additional modes, or online leaderboard is approved.

## Approved technical direction

- Plain HTML, CSS, and JavaScript ES modules without a framework/build step for the initial app.
- DOM controls for visible tiles, rendered only for the current viewport.
- Deterministic per-grid mine generation from a saved run seed and grid coordinates; store progress rather than duplicate mine layouts.
- IndexedDB for runs, preferences, and local score records.
- Static GitHub Pages publishing from the repository root on main.

## Open — resolve before dependent implementation

- How does a player reveal a tile and flag/unflag one with mouse, touch, and keyboard? How should the app distinguish a tap/click from a pan drag?
- If a mode has no saved run, what are the choices presented and which one is selected by default?
- Should first-time tutorial state be tracked once globally or separately for each mode?
- How is dark/light theme selected and persisted?
- What username is used for local score ties? For equal score and username, does an earlier or later date rank first? How many records are kept, and how are in-progress records shown?
- Are flag actions blocked during the 3-second mistake animation?
- If Time Attack reaches zero during a mistake animation, does timeout end immediately or wait until the animation completes?
- What happens if the page closes during the mistake animation: preserve its remaining duration or finish it on resume?
- Does pause-menu Save write the existing save or create a separate checkpoint? Pause-menu board input behavior should also be confirmed.
- For save import, how should invalid/older files and an existing mode save be handled? Import is approved; export is not.
- What exact visual effects change when reduced motion is enabled?
- Which browsers and screen sizes are supported?
- Should a navigation key pan one cell, one grid, or a fixed viewport distance?

## Architectural consequences

- No multiplayer synchronization or cross-device recovery is available without a backend.
- Local scores are not tamper-resistant.
- Time Attack uses a wall-clock deadline so time advances while closed; system-clock changes may affect it.
- Finite browser storage can fill as explored progress grows; session-only fallback prevents a storage error from automatically ending a run but cannot preserve it after reload.