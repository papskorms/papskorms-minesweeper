# Starter assets

## Font
- Nunito Variable, weights 200–1000: fonts/Nunito-Variable.ttf.
- SIL Open Font License 1.1: fonts/OFL.txt.
- fonts.css shows the local @font-face declaration.
- Source: [Google Fonts Nunito](https://github.com/google/fonts/tree/main/ofl/nunito).

## SVG sprites
- sprites.svg is the dark-theme symbol library; sprites-light.svg is the light-theme variant.
- Cell IDs: cell-hidden, cell-hidden-hover, cell-open, cell-number-1 through cell-number-8, cell-flag, cell-mine, cell-mistake, cell-locked.
- Icon IDs: icon-lock, icon-heart, icon-heart-empty, icon-clock, icon-help, icon-close, icon-pause, icon-play, icon-infinite, grid-boundary.
- Number colors follow common Minesweeper conventions. Flags are red in both themes. Cells are square; surrounding interface panels may be rounded.

Reference a symbol with an SVG use element, for example:
<svg viewBox="0 0 64 64" aria-hidden="true"><use href="sprites.svg#cell-number-1"></use></svg>

## Preview
Open asset-sheet.svg and asset-sheet-light.svg to view the dark and light contact sheets.
