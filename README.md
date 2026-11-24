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

Current release: **0.2.6.5** (`v0.2.6.5`). The version is visible in the site header on the home screen and game screens. The earlier mistaken `v0.0.0.1` release remains superseded; `0.2.6.5` continues from `0.2.6.3`.

- In-app diagnostics: **82 passed / 0 failed**.
- Blackjack recovery: starting a new round clears the previous settled phase before dealing; a second-round dealer natural now records one settlement, displays one outcome, and allows round 3. Deterministic UI proof: `proofs/blackjack-reentry-0.2.6.5.json` and `proofs/deckforge-0.2.6.5-iphone-390-blackjack-round2-natural.png`.
- Bankroll dashboard: income/expenses/net comparison, balance trend with an explicit range, ledger reconciliation, and paired inflow/outflow charts for all eight categories. Recent ledger is capped to the latest 80 transactions; the most recent 12 are shown.
- Bankroll QA: a 14-entry fixture reconciled `$0 + $154 = $154`; empty history also reconciles and shows a clear no-data state. Mobile/iPad geometry and screenshots: `proofs/bankroll-layout-0.2.6.5.json`, `proofs/bankroll-empty-0.2.6.5.json`, and versioned PNGs in `proofs/`.
- Vision reviewed mobile `390×844`, iPad portrait `834×1194`, and iPad landscape `1194×834`; no horizontal overflow, overlapping labels, or chart clipping. Physical Apple hardware/Safari was not available, so this is Chromium device emulation.
- Durak stress: **168 seeded matches / 14 configurations**, audits on every move, zero failures; bounded matches completed in 32–73 steps. Proof: `proofs/durak-0.2.6.5-stress.json`.
- Release badge: `v0.2.6.5` is present on Home and active Blackjack at `390×844`; proof: `proofs/version-badge-0.2.6.5.json` plus its screenshots.
- `node --check`, JSON validation, static audit, and `git diff --check`: **PASS**. Detailed measurements: `proofs/static-audit-0.2.6.5.json`.
- Release artifacts: `RELEASE_NOTES_0.2.6.5.md` and `releases/DeckForge-0.2.6.5.zip`.

`plan.md` records implementation and measured acceptance evidence. Currency is fictional and local-only. Physical iPhone/iPad Safari and real audio were not tested; layout checks used Chromium emulation and the touch proof used synthetic pointer input.
