# DeckForge

DeckForge is an offline single-file card room for iPhone, iPad, and desktop browsers.

## Games

- **Blackjack** — a real six-deck shoe with physical card identities, hit, stand, double, split, insurance, natural Blackjack 3:2, dealer soft-17 rules, discard tracking, and shoe reshuffles.
- **Durak** — a complete configurable 2–4-seat Podkidnoy match with a real 36-card deck, trump, attacks, throw-ins, defenses, takes, draw order, and AI opponents.

## Bankroll and progression

The fictional bankroll, wins, table settings, and transaction history persist in `localStorage`. Each game has four stake tables and wins unlock higher tables. A new wager made at a zero balance grants exactly `$100` before the wager is deducted. Switching games or leaving the page cancels an unfinished round and returns its stake; the completed stats remain saved. A reload opens the home screen and restores the saved stats/settings, but does not resume a live hand. The `Reset local save` control is the intentional deletion path. On the first DeckForge-branded load, the pre-DeckForge local save is cleared once so the renamed app starts clean; stats created afterward persist normally.

## Run

Open `deckforge.html` directly in a browser. No server, install, build, font, image, or network connection is required.

## Release and verification

Current release: **0.2.5** (`v0.2.5`). The version is visible in the site header on the home screen and both game screens.

- In-app diagnostics: **60 passed / 0 failed**.
- Durak stress: **100 seeded matches** across five 2–4-seat rule configurations; all matches terminated and every step preserved the 36-card audit.
- Full rendered-UI Durak matches completed for 2, 3 and 4 seats. Separate flows exercised attack, throw-in, defense, push, take, a human-loss terminal result, a valid touch-pointer drag and an invalid drop.
- Blackjack insurance was tested with both choices in 3-seat play and through all three AI turns in a 4-seat game; the human action and settlement completed.
- Responsive Durak geometry passed at 390×844 and 1024×768 with six table pairs; action controls remained inside the viewport.
- Static audit: one inline script, no external assets, duplicate IDs, missing DOM references or inline event attributes; `node --check` passes.
- Structured evidence: `proofs/self-test.json`, `proofs/browser-qa.json`, `proofs/static-audit.txt`, and `proofs/deckforge-0.2.5-*.png`.
- Historical Blackjack split/double evidence remains in `proofs/blackjack-split-double-qa.json`; previous release notes and artifacts are retained.

`plan.md` records the implementation plan and measured acceptance evidence. Currency is fictional and local-only. Physical iPhone/iPad Safari and real audio were not tested; Chromium mobile emulation and synthetic `PointerEvent` touch-pointer flows were used.
