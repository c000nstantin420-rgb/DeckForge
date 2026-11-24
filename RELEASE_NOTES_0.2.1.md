# DeckForge 0.2.1

## Highlights

- Persistent local bankroll, wins, table settings, and transaction history.
- Switching tabs or leaving an unfinished round safely returns its stake.
- Reload restores saved stats and settings; active live rounds are intentionally not resumed.
- Durak push/transfer works with two seats.
- Durak `All players` throw-in unlocks at four seats; `Neighbors` and `Off` remain available below that.
- English setup UI with a `?` explanation for every configurable table setting.
- Felt/card-room home preview replaces the old flat placeholder.
- Runtime artifact is `deckforge.html`.

## Verification

- 39/39 in-app assertions passed.
- `node --check` passed.
- Browser QA: mobile 390×844, active tablet 1024×768, tab switch, reload, reset, two-seat push, all-player throw-in, throw-in off, and visual screenshot review.
- No external assets or network dependencies.
