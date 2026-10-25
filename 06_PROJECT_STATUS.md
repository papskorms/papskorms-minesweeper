# Project status

**Updated:** 2026-10-25

## Current state

- Product behavior and unresolved items are documented in 01_REQUIREMENTS.md and DECISIONS.md.
- Starter assets exist under assets/: Nunito Variable and license, dark/light SVG sprites, and contact sheets.
- Source-of-truth documentation has been drafted from approved decisions.
- No game implementation exists yet. Public GitHub repository creation is the next publishing setup action.
- GitHub Pages is the approved playable hosting target; it is not configured yet.

## Next work

1. Set up the public repository and add the documentation and starter assets.
2. Resolve material questions that block board and UI implementation.
3. Select and record implementation, storage, generation, and deployment architecture.
4. Implement and verify the playable game.

## Risks and blockers

- Zero expansion at locked grid boundaries affects unlocking and must be settled before board logic.
- Username source and date tie direction affect local score ordering.
- Browser storage can be cleared or unavailable; local data cannot be guaranteed across devices.
- Time Attack timeout can coincide with a mistake animation; exact precedence is open.
