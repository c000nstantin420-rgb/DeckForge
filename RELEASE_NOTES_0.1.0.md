# DeckForge 0.1.0

## Release

Initial private release of the offline DeckForge single-file card room.

## Included

- Real six-deck Blackjack shoe with crypto-backed Fisher–Yates shuffling and physical card identities.
- Blackjack hit, stand, double, split, insurance, 3:2 natural Blackjack, dealer stands on soft 17, discard tracking, and penetration reshuffle.
- Real 36-card Durak deck with trump, attack, throw-in, defense, take, draw order, terminal rules, and AI.
- Fictional persistent dollar bankroll, table progression, wagers, payouts, transaction history, and the exact `$100` bankruptcy grant.
- iPhone/iPad-first dark card-room UI with centered mode selection, safe-area controls, fixed mobile action dock, visible Durak trump card, and reduced-motion support.
- In-app Diagnostics & engine proofs panel.

## Verification

- Engine: **26 passed / 0 failed**.
- Static audit: `node --check` passed; one inline script; no external assets; no duplicate IDs; no missing `$()` DOM references; no inline event attributes.
- Browser Blackjack reached `settled` through the rendered lobby and action flow.
- Browser Durak reached `finished` through rendered card/action controls, with player losses, a player win, and a draw recorded across UI matches.
- Persistence proof: `$250 → $240` survived reload.
- Bankruptcy proof: `$0 → +$100 → $90` after a `$10` start, with one grant and visible toast.
- Mode-centering proof: desktop strip center `512` at 1024px with four 230px cards; mobile strip center `195` at 390px with two centered columns.
- Mobile geometry: `scrollWidth` stayed equal to viewport width at 390×844 and 1024×768; fixed action dock reached the viewport bottom.

## Known limits

The release has not been exercised on a physical iPhone/iPad Safari installation or real audio hardware. Currency is fictional and local-only; there is no account, payment, or network multiplayer system.
