# Architecture and design record

This file records technical decisions, not product behavior. Approved behavior is in 01_REQUIREMENTS.md. Items marked Proposal are not implementation instructions when they affect material behavior.

## Confirmed constraints

- The game is intended for static hosting on GitHub Pages.
- Browser-local persistence is required; no backend, accounts, cloud storage, or online leaderboard is approved.
- The world is unbounded in four orthogonal directions. Grid generation and clues must be consistent across 16×16 boundaries and reproducible across save/resume.
- Assets are in assets/: Nunito variable font, dark/light SVG sprite libraries, and preview sheets.

## Components to plan

Logical responsibilities will need to cover board generation and adjacency, reveal/flag rules, world/grid unlock state, mode clocks and lives, scoring/high-score records, local persistence, and rendering/input. Module boundaries and framework are not chosen.

## Data-flow constraints

- User actions update one mode's active run and its score.
- Save/resume must preserve generated board state and progress.
- Time Attack must compute elapsed time across page closure and pause; a ticking interval alone cannot satisfy that rule.
- Cross-grid clues must not depend on which neighboring grid was displayed first.
- If storage writes fail, show the required error toast.

## Unresolved technical choices

- App structure and language/framework (plain HTML/CSS/JavaScript versus a framework).
- Browser storage API and schema/version migration approach.
- Deterministic world generation, seed representation, and when neighboring grid data is generated.
- Zero expansion at locked grid boundaries (also a product-rule question).
- Rendering strategy for an unbounded world and supported viewport sizes.
- How GitHub Pages deployment is triggered and verified.
- How dark/light theme is selected and persisted.

Do not finalize these choices without checking the requirements and recording material decisions in DECISIONS.md.
