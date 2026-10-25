# Architecture

**Status:** Technical direction approved. Product behavior remains governed by 01_REQUIREMENTS.md. Unresolved behavior and choices remain listed in DECISIONS.md.

## Deployment and runtime

- Use plain HTML, CSS, and JavaScript ES modules. Do not add a framework or build step for the initial static app.
- Publish the static app from the repository root of the public main branch with GitHub Pages after a playable build exists.
- Use relative asset URLs so the app works under the GitHub Pages project path.
- No backend, account service, remote runtime dependency, or online leaderboard.

## Components

- **UI and input:** mode selection, board, tutorial, pause/game-over overlays, theme and reduced-motion controls; translate input into domain actions.
- **Game rules:** reveal/flag validation, zero expansion, mistake/lives, scoring, grid solve and unlock rules. Isolated from storage and rendering.
- **World generator:** deterministic grid layout and clue calculation from a run seed and signed grid coordinates.
- **Run coordinator:** load a mode, apply user actions, call game rules, update mode/clock, and persist resulting state.
- **Clock:** maintain the Time Attack deadline using wall-clock time, not elapsed in-memory interval time.
- **Persistence:** IndexedDB access for active runs, preferences, and local score records; import validation.
- **Renderer:** render only the current viewport and relevant nearby grids. Do not create DOM nodes for the entire infinite world.

Keep module boundaries small and dependency-free; each responsibility may be implemented as a plain JavaScript module.

## World and generation

A grid coordinate is a signed pair (gridX, gridY); a cell has local coordinates in the range 0–15. World coordinates are derived from the grid and local cell coordinates. Each run has a seed. A deterministic pseudorandom generator keyed by the run seed and grid coordinates produces exactly 40 mines per grid, independent of visit order.

For the initial grid, choose one of the four central cells from the run seed and exclude that cell and its eight neighbors from mine placement so the chosen start cell is a zero. Place the grid’s 40 mines among the remaining cells. Persist the seed and selected start cell so a save resumes the same world.

Clues query the eight neighboring world cells, including cells in adjacent grids. Generate those layouts on demand even if the neighboring grid is not unlocked. Reveal expansion may traverse playable grids but stops before entering a locked grid, as now approved. Flags do not affect solve completion.

## Data model

Use one IndexedDB database with versioned object stores:

- **settings:** reduced-motion preference and other approved persistent preferences.
- **runs:** one active run per mode, keyed by mode.
- **scores:** local score records, keyed by record/run ID and mode.

An active run contains schema version, run ID, mode, world seed, selected start cell, lives, score, status, and first-reveal timestamp. Time Attack also stores an absolute deadline timestamp. Per-grid progress stores revealed cells, user flags, and any pending mistake state, keyed by signed grid coordinate. Mine layouts and clues are derived from the seed; solved grids and unlocked neighbors can be derived from saved progress.

Represent each 256-cell state with compact bitsets where practical. Validate schema version and ranges when loading. Exact save-import conflict behavior and migration policy remain open.

An ongoing score record is updated as safe cells are revealed and finalized on end/quit. Whether ongoing records appear in the ranked high-score view remains open.

## Data flow and contracts

1. Mode selection loads that mode’s IndexedDB run. If none exists, show the approved choice flow; its options remain unresolved.
2. A game action is passed to the rules module. It returns the next state and domain events such as safe cells revealed, mistake started, grid solved, or game over.
3. The coordinator updates score, lives, deadline, completion, and unlock state, then persists the run and its ongoing score record.
4. The renderer updates from state; it does not own game rules.
5. On resume, compare the current wall clock with the saved deadline. A zero/expired Time Attack run ends immediately according to the approved timeout priority. Mistake-animation/timeout collision is still unresolved.
6. Browser storage failure shows a toast and switches to session-only play. The session cannot be recovered after close/reload.

Conceptual API contracts:

- WorldGenerator.getGrid(seed, gridX, gridY) -> GridLayout
- WorldGenerator.getClue(seed, worldX, worldY) -> 0..8
- GameRules.applyAction(run, action, timestamp) -> { run, events }
- RunRepository.load(mode) -> Run | null
- RunRepository.save(run) -> success | StorageError
- ScoreRepository.update(run) -> success | StorageError
- SaveImporter.validate(fileData) -> ValidatedSave | ImportError

The rules API receives time explicitly where timing affects outcomes, making it possible to reason about the deadline and mistake animation separately from UI timers.

## Security and errors

- There is no server-side identity or trusted score enforcement. Local scores and save data can be edited by a player; that is acceptable without an online leaderboard.
- Treat imported save files as untrusted. Validate shape, version, coordinates, and value ranges before applying them. Do not render imported strings as HTML.
- Catch IndexedDB open/write/quota failures, show the required toast, and continue in session-only mode. Persistence and scores may be lost after reload.
- Avoid external scripts, runtime fonts, and network calls. Use the included local font and SVG assets.
- Browser data clearing can remove saves; no recovery path is promised.

## Rendering and deployment trade-offs

Use DOM controls for visible cells and CSS grid for boards. DOM is easier to label, focus, and interact with accessibly than Canvas; render only the current view to avoid DOM growth as the world expands. The board navigation controls are WASD, arrow keys, mouse drag, and touch drag. The exact reveal/flag gestures and click-versus-drag threshold remain open.

GitHub Pages branch-root publishing is the simplest fit because the app has no build step. A GitHub Actions deployment workflow may be added later if a build or automated publishing checks become useful.

## Alternatives considered

- **Framework and bundler:** React/Vite could help if the interface becomes substantially more complex, but adds dependencies and a build step. Not selected for the initial app.
- **localStorage:** simpler synchronous API, but rewriting an ever-growing world and risking quota limits is a poor fit. IndexedDB is selected.
- **Canvas:** potentially efficient for a large viewport, but complicates accessible tile controls. DOM is selected with viewport-limited rendering.
- **Persist full mine layouts:** straightforward to inspect but stores redundant data. Seeded deterministic generation plus progress bitsets is selected.
- **GitHub Actions deployment:** more control over build and checks but unnecessary for plain static files. Root branch publishing is selected.

## Still unresolved

See DECISIONS.md for product decisions that still need answers, including pointer/touch reveal and flag actions, tutorial scope, score identity and tie details, theme choice, timed-game edge cases, import conflicts, reduced-motion effects, and browser/viewport support.