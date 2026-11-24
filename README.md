# DeckForge

DeckForge is an offline single-file card room for iPhone, iPad, and desktop browsers.

## Games

- **Blackjack** — a real six-deck shoe with physical card identities, hit, stand, double, split, insurance, natural Blackjack 3:2, dealer soft-17 rules, discard tracking, and shoe reshuffles.
- **Durak** — a complete configurable 2–4-seat Podkidnoy match with a real 36-card deck, trump, same-rank multi-card attacks/throw-ins, tap and drag-and-drop controls, defenses, takes, draw order, and AI opponents.

## Bankroll and progression

The fictional bankroll, wins, table settings, and transaction history persist in `localStorage`. Each game has four stake tables and wins unlock higher tables. A new wager made at a zero balance grants exactly `$100` before the wager is deducted. Switching games or leaving the page cancels an unfinished round and returns its stake; the completed stats remain saved. In Durak, the winner collects one ante from every seat (`players × stake`); a human loss loses only the human ante, and a draw returns it. A reload opens the home screen and restores the saved stats/settings, but does not resume a live hand. The `Reset local save` control is the intentional deletion path. On the first DeckForge-branded load, the pre-DeckForge local save is cleared once so the renamed app starts clean; stats created afterward persist normally.

## Run

Open `deckforge.html` directly in a browser. No server, install, build, font, image, or network connection is required.

## Release and verification

Current release: **0.0.0.1** (`v0.0.0.1`). The version is visible in the site header on the home screen and both game screens.

- In-app diagnostics: **76 passed / 0 failed**.
- Durak stress: **168 seeded matches** across 14 valid 2–4-seat throw-in/push configurations; every move preserved the 36-card audit and each match reached a legal terminal result.
- Mobile lobby: detailed seat/rule settings start collapsed at widths `≤760px`, with a current-config summary and `Customize` entry point. At `390×844`, wager and start are above the fold; the short `375×667` lobby scrolls vertically without horizontal overflow.
- Depth on demand: all Blackjack and Durak settings expand, stay open while options change, and collapse on demand. Returning Home and reopening resets to the mobile default. Desktop at `1024×768` starts expanded.
- Fresh browser flows: 3-seat Blackjack → deal → Stand → settlement; 4-seat Durak/all-player → legal `Take cards` action. Both physical-card audits passed with zero runtime errors.
- Disclosure transitions: Customize, setting rerender, Hide, Home/re-entry reset, and automatic `760px` breakpoint defaults all passed. A synthetic touch-pointer sequence hit-tested and opened the rendered Customize button.
- Vision reviewed collapsed/expanded Blackjack and Durak lobbies at `390×844`, Blackjack at `375×667`, and desktop Blackjack at `1024×768`.
- Desktop limit: at `1024×768`, the expanded lobby has no horizontal overflow but its lower wager/Deal controls are below the fold; page height is `1143px`, so vertical scroll is needed.
- `node --check`, JSON validation, static audit, and `git diff --check`: **PASS**.
- Evidence: `proofs/self-test-0.0.0.1.json`, `proofs/durak-0.0.0.1-stress.json`, `proofs/browser-qa-0.0.0.1.json`, `proofs/setup-disclosure-0.0.0.1.json`, `proofs/blackjack-ui-flow-0.0.0.1.json`, `proofs/durak-ui-flow-0.0.0.1.json`, `proofs/touch-qa-0.0.0.1.json`, and `proofs/deckforge-0.0.0.1-*.png`.

`plan.md` records implementation and measured acceptance evidence. Currency is fictional and local-only. Physical iPhone/iPad Safari and real audio were not tested; layout checks used Chromium emulation and the touch proof used synthetic pointer input.
