# Development plan

**Status:** Proposed sequence only. No milestone is a user-approved schedule.

1. **Resolve remaining behavior questions:** interrupted mistake timing; username registration/validation and tie-order details; theme persistence; pause-save semantics; import conflict replacement; browser versions, viewport boundaries, touch threshold, and keyboard selection boundary behavior.
2. **Use the selected static architecture:** plain HTML/CSS/ES modules, DOM-rendered visible cells, seeded per-grid generation, IndexedDB, and GitHub Pages branch-root publishing. Details are in 02_ARCHITECTURE.md.
3. **Build board rules:** random saved start cell among the four centers, safe zero, locked-boundary expansion stop, fixed per-grid mines, cross-boundary clues, tile actions, mistakes/lives, solve/unlock behavior.
4. **Build persistence and modes:** per-mode saves, wall-clock Time Attack deadline, score records, storage-error session-only fallback, save import.
5. **Build UI and accessibility:** tutorial, pause menu, dark/light presentation, reduced motion, and approved navigation/interaction controls.
6. **Verify and publish:** follow 05_TESTING_STRATEGY.md, then publish the playable app through GitHub Pages.

Dependencies: resolve decisions that affect input, state, and ranking before implementing those parts. Architecture choices are selected but do not settle the remaining product questions. Current progress and immediate next task are in 06_PROJECT_STATUS.md.