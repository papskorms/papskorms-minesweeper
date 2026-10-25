# Project overview

## Product

**Papskorms Minesweeper** is an infinite Minesweeper browser game. It is intended for the owner and public players, with a public source repository and a playable GitHub Pages site.

## Goals

- Let players explore an expanding world of 16×16 Minesweeper grids.
- Make the first move safe and understandable.
- Offer relaxed Default play and a strict one-minute Time Attack mode.
- Preserve active games and local high scores in the same browser.
- Present the game in a rounded, readable interface while keeping board tiles square.

## Scope

Approved for the initial product: Default and Time Attack, adjacent-grid unlocking, safe first reveal, three lives, local per-mode saves and high scores, a tutorial overlay, reduced-motion preference, and a public GitHub Pages build.

Deferred: accounts, cloud saves, cross-device sync, online leaderboard, multiplayer, ads/monetization, and extra modes. See 01_REQUIREMENTS.md for behavior and DECISIONS.md for unresolved questions.

## Constraints

- The published game is expected to run as a static GitHub Pages site.
- Save data and high scores are local to a browser profile. Clearing browser data can remove them.
- No backend, identity provider, or online leaderboard is approved.
- A public repository means source and included assets are public.

## Non-goals

The project is not currently a multiplayer service, a cross-device profile system, or an online competitive leaderboard.
