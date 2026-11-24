# DeckForge

DeckForge is an offline single-file card room for iPhone, iPad, and desktop browsers.

## Games

- **Blackjack** — a real six-deck shoe with physical card identities, hit, stand, double, split, insurance, natural Blackjack 3:2, dealer soft-17 rules, discard tracking, and shoe reshuffles.
- **Durak** — a complete configurable 2–4-seat Podkidnoy match with a real 36-card deck, trump, attacks, throw-ins, defenses, takes, draw order, and AI opponents.

## Bankroll and progression

The fictional bankroll, wins, table settings, and transaction history persist in `localStorage`. Each game has four stake tables and wins unlock higher tables. A new wager made at a zero balance grants exactly `$100` before the wager is deducted. Switching games or leaving the page cancels an unfinished round and returns its stake; the completed stats remain saved. A reload opens the home screen and restores the saved stats/settings, but does not resume a live hand. The `Reset local save` control is the intentional deletion path. On the first DeckForge-branded load, the pre-DeckForge local save is cleared once so the renamed app starts clean; stats created afterward persist normally.

## Run

Open `deckforge.html` directly in a browser. No server, install, build, font, image, or network connection is required.

## Verification

- Current engine diagnostics: **39 passed / 0 failed**.
- Static audit: one inline script, no external assets, no duplicate IDs, no missing DOM references, no inline event attributes, and `node --check` passes.
- Browser QA covers configurable 1–4-seat Blackjack, configurable 2–4-seat Durak, Blackjack payout/soft-17/insurance/surrender rules, Durak 2-seat push, 4-seat-only all-player throw-in, throw-in off, help buttons for every table setting, English-only UI, AI progression, PointerEvent drag attack/defense, persistence, the bankruptcy grant, and 390×844 / 1024×768 no-scroll gameplay geometry.
- Current variant measurements and screenshots are in `proofs/multiplayer-variants-qa.json`, `proofs/multiplayer-variants-static-audit.txt`, and `proofs/deckforge-*.png`; the `0.2.3` outcome-banner regression proof is in `proofs/banner-fix-qa.json`. Release `0.2.3` fixes the duplicate single-hand Blackjack result banner while preserving per-hand split results, and carries forward the `0.2.2` mobile polish, table variants, and side-AI throw-in support. GitHub Pages deploys on main pushes and published releases.
- `plan.md` contains the implementation plan and measured acceptance evidence.

Currency is fictional and local-only. Physical iPhone/iPad Safari and real audio hardware still need a device pass; Chromium mobile emulation and touch-pointer dispatch are covered.
