# Decisions and open questions

Use 01_REQUIREMENTS.md for full behavior. This file tracks status and rationale without duplicating detailed rules.

## Approved

- Public project: source repository is public; game is intended for GitHub Pages.
- Board: infinite orthogonal world of 16×16 grids; each grid has 40 mines (15.625%); clues include adjacent cells across boundaries.
- Start/progression: center cell is a guaranteed zero; only it starts the game; solve means reveal every safe cell; flags are optional; solving unlocks orthogonal neighbors.
- Risk/modes: three lives; mine mistake animation lasts 3 seconds and blocks reveals; Default is untimed; Time Attack starts on first reveal, resets to 60 seconds per solved grid, and continues during pause/closure.
- Saves/scores: browser-local saves per mode; local high scores per mode; save on quit; show a toast when storage is unavailable/full; a game-over board is review-only and does not persist after browser restart.
- UI: question-mark tutorial with dimmed backdrop and X close; opens automatically on first play; square cells, rounded panels/type, contiguous grids with dashed boundaries, dim locked grids with lock icons, red flags in both themes.
- Accessibility: persistent reduced-motion preference; do not inherit the browser/OS preference.
- Delegated scoring rule: award one point per newly revealed safe cell, including zero-expansion cells; flags and mine mistakes score zero. Rationale: score tracks safe progress and does not reward an error.

## Open — ask before dependent implementation

- When zero expansion reaches a locked grid, should expansion stop at that boundary or reveal into that grid? How does this interact with unlocking?
- What username appears in local high-score ties, and how does the player set it?
- For equal score and username, does the earlier or later achievement date rank first?
- Is first-time tutorial state tracked once per browser/game or independently per mode?
- Does the player choose dark/light theme, and should that choice persist? Mockups/assets show both directions but do not settle the control.
- Which inputs are required: mouse, touch, keyboard, and what action flags a tile?
- Are flag actions blocked during the 3-second mistake animation, or only reveals?
- If Time Attack reaches zero during the mistake animation, should timeout end immediately or wait until the animation completes?
- If switching to a mode with no saved run, should it start immediately or offer a choice?
- Does pause-menu Save write the existing save or create a separate checkpoint?
- How should importing a save handle invalid, older, or existing saved data? Save import is approved; export is not yet confirmed.
- What precise effect does reduced motion have on the rumble/highlight?
- Which browsers and screen sizes are supported?

## Technical decisions not yet selected

Framework/language, local storage API and schema, deterministic generation strategy, deployment automation, and theme implementation are open architecture choices. See 02_ARCHITECTURE.md.
