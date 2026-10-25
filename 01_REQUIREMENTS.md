# Requirements

Status labels: **Approved** means directly confirmed by the user or explicitly delegated. **Open** means behavior must not be invented. Acceptance criteria are observable checks for approved behavior.

## R-01 — Public static game

**Status:** Approved. Publish source in a public GitHub repository and make the game playable through GitHub Pages. **Goal:** public access to source and game. **Preconditions/outcome:** the published page loads in a browser and allows play. **Rules:** no backend is approved. **Edges:** deployment-failure handling is open. **Acceptance:** public source is accessible; GitHub Pages serves a playable game without installation.

## R-02 — Infinite grid world and unlocking

**Status:** Approved. The world is arranged in 16×16 grids. Only the center grid is initially playable. Solving a grid unlocks its directly adjacent up/down/left/right grids; diagonals do not unlock it. A grid is solved by revealing every safe tile; flags are optional. **Goal:** progressively explore an unbounded field. **Outcome:** when a grid becomes solved, its orthogonally adjacent locked grids become playable. **Rule:** zero expansion stops before entering a locked grid and does not reveal or alter any tile in that grid. **Acceptance:** locked grids cannot be interacted with; solving a grid unlocks only its orthogonal neighbors; flags are not required for completion.

## R-03 — Safe start

**Status:** Approved. Before the game starts, only the initial grid’s start tile can be clicked. At run creation, choose one of the four central cells at random; save that selection for the run. The selected tile is safe and has clue value zero. Its normal zero expansion follows, stopping at any locked-grid boundary. **Goal:** avoid an initial guess. **Outcome:** clicking the selected start cell starts the game, reveals it, and begins normal expansion. **Rule:** zero expansion stops at locked-grid boundaries, as specified in R-02. **Acceptance:** other initial tiles cannot start or alter play; the selected start cell is always zero and never consumes a life.

## R-04 — Mine density and clues

**Status:** Approved. Mine density is enforced per grid at approximately 15.6%; use 40 mines per 256-cell grid (15.625%). Clues count adjacent mines across grid boundaries. **Goal:** medium density and consistent edge clues. **Outcome:** generated grids have a fixed mine count and each clue reflects the surrounding eight cells, including across boundaries. **Acceptance:** each grid has exactly 40 mines; the start-safety rule constrains their placement; border clues include adjacent cells in neighboring grids.

## R-05 — Tile interactions

**Status:** Approved. Revealing an already revealed tile does nothing. Flagging toggles a flag; a second flag action unflags it. A mine marked as a mistake cannot subsequently be unflagged or revealed again. A safe tile may be flagged without penalty. Only playable grids accept player interactions. **Goal:** predictable Minesweeper controls. **Mouse:** left click reveals; right click flags/unflags; left-button drag pans. **Keyboard:** WASD moves cell selection; arrows pan; Z reveals; X flags. **Touch:** quick tap reveals; holding a tile 1.5 seconds flags; holding while moving pans. **Acceptance:** repeated reveal on a revealed tile has no effect; flag/unflag toggles an ordinary tile; safe-tile flags do not affect lives or completion; mistake mines remain locked after resolution.

## R-06 — Mine mistake and lives

**Status:** Approved. A mine reveal triggers a red mistake highlight and brief rumble, then an automatic flag after 3 seconds. There are 3 starting lives; the game ends immediately when lives reach zero, even during the final mistake animation. Repeated input during the 3-second delay does not count. During a mistake animation, no tiles can be revealed or flagged; repeated input does not count. The animation may be cancelled if the page closes. **Goal:** make mistakes visible and consequential. **Open:** after a non-final animation is interrupted by page closure, whether the tile is auto-flagged immediately on resume or the remaining delay continues. **Acceptance:** a mine attempt consumes one life once; it is auto-flagged after 3 seconds if uninterrupted; no tile reveal or flag is accepted during animation; zero lives or timer ends immediately without waiting.

## R-07 — Modes and separate saves

**Status:** Approved. Provide Default and Time Attack. Default has no countdown. A player may switch modes during an active game; each mode has its own save. **Goal:** support relaxed and timed play. **Outcome:** switching modes preserves the current mode's save and opens the other mode's saved state. **Rule:** if the selected mode has no saved run, begin a new game. **Acceptance:** Default has no timer; switching modes does not overwrite the other mode's saved run.

## R-08 — Time Attack clock

**Status:** Approved. The timer starts at 60 seconds on the player's first reveal. Solving any grid resets it to 60 seconds. It continues while the page is closed or the game is paused. Reaching zero ends the game immediately; timeout takes precedence over a grid solve occurring simultaneously. Mistakes still cause game over. **Goal:** time pressure with progress-based resets. This also applies during mistake animations as specified in R-06. **Acceptance:** no timer before first reveal; timer continues during pause/page closure; a solved grid resets it to 60 seconds; reaching zero ends the run and beats a simultaneous solve.

## R-09 — Pause menu and quit

**Status:** Approved. The pause menu presents Continue, Save, and Exit in that order. In Time Attack, the clock continues while this menu is open. Exit saves and returns to the main menu for mode selection. **Goal:** let players manage a run without suspending Time Attack. **Open:** whether pause blocks board input and whether Save writes the current save or creates a separate checkpoint. **Acceptance:** menu has the three named actions; Resume returns to play; Quit saves and returns to mode selection; timed play continues while paused.

## R-10 — Game-over review

**Status:** Approved. The game-over screen can be temporarily closed so the player can inspect that run's board. Starting a new game or quitting replaces the board. Closing the page after game over counts as quitting; a game-over board does not persist after browser restart. **Goal:** permit review without retaining a finished board as the active run. **Acceptance:** closing the game-over overlay reveals the board; a new game or quit replaces it; browser restart does not restore the game-over board.

## R-11 — Browser saves and recovery

**Status:** Approved. Save active games in browser storage so players can quit and resume in the same browser. Keep separate active saves by mode. Save on quit. Time Attack elapsed time must continue while closed. Browser data clearing may remove saves; recovery is not guaranteed. **Goal:** resume interrupted play. **Rule:** if browser storage is unavailable or full, show an error toast and continue in session-only mode; unsaved progress may be lost when the page closes or reloads. **Open:** save schema/versioning, migration, and save-import validation/conflict behavior are documented in DECISIONS.md. Save-file import was approved; export was suggested but not confirmed. **Acceptance:** a saved run resumes with board, flags, revealed tiles, lives, mode, and timer state intact; storage failure displays a toast; cleared data is not represented as recoverable.

## R-12 — Score and local records

**Status:** Approved. Score is based on total revealed cells and applies to both modes. Each mode has its own local top-10 high-score list. Score accrues during play but in-progress runs are not listed. On completion or quit, ask whether the player wants to register the score; username is optional and limited to 5 letters. Ties are prioritized by score, then alphabetical username, then newer completion first. **Delegated scoring rule:** count each newly revealed safe cell once, including cells revealed by zero expansion; flags and mine mistakes score zero. **Goal:** reward safe progress and track personal records. **Open:** whether omitting a username skips registration or creates an anonymous entry; allowed username characters/case; alphabetical sort direction. **Acceptance:** every safe tile contributes at most once; both modes record separately; the run score is preserved during play and on end/quit; ties use the declared field priority once username/date policies are chosen.

## R-13 — Tutorial

**Status:** Approved. A question-mark button opens short tutorial text in a container over a slightly darkened board. The container has an X close button. It does not pause Time Attack. The tutorial opens automatically the first time the user plays each mode; first-time status is tracked separately per mode. **Goal:** explain controls and progression. **Acceptance:** button opens tutorial; X closes it; backdrop is dimmed; automatic opening occurs on first play of each mode only; Time Attack keeps running.

## R-14 — Lives display

**Status:** Approved. Show remaining lives as a lives counter, starting at 3, not as a mistake counter. **Acceptance:** display starts at 3 and reflects mine mistakes according to R-06; safe flags do not change it.

## R-15 — Reduced motion

**Status:** Approved. Provide a reduced-motion preference that persists across sessions. Do not follow the browser/OS preference. When enabled, remove the mistake-tile rumble. **Acceptance:** saved preference is restored in a later session; rumble is removed when enabled.

## R-16 — Visual direction

**Status:** Approved as visual direction. Rounded type and rounded application panels; board tiles remain square. Adjacent grids touch with subtle dashed boundary lines. Locked grids are dimmed with a lock indicator. Flags are red in both themes. Use typical Minesweeper number colors. Provide dark and light themes, selected with a small switcher in the bottom-left corner. **Open:** whether theme choice persists across sessions and final exact palette. Generated mockups are inspiration, not pixel-perfect specifications. **Acceptance:** implemented visuals preserve square cells, contiguous grids with dashed boundaries, red flags in both themes, colored numbers, and clear dimmed locks.

## R-17 — Browser and accessibility input

**Status:** Approved for input methods and initial browser targets. Support Chrome, Opera, and Brave; adapt to common screen sizes. Held-arrow camera panning smoothly accelerates. **Open:** browser versions/platforms, viewport range, touch drag threshold, WASD boundary behavior, and initial selection position.
