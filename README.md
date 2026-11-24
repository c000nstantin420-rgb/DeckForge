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

Current release: **0.2.6.2** (`v0.2.6.2`). The version is visible in the site header on the home screen and both game screens.

- In-app diagnostics: **75 passed / 0 failed**.
- Durak stress: **168 seeded matches** across 14 valid 2–4-seat throw-in/push configurations; every move preserved the 36-card audit and each match reached a legal terminal result.
- Durak mobile: at `375×667`, the 3-seat defender sees all six cards and all three actions in one row; the hand remains above the dock. A four-seat `375×600` fixture also kept every card and action reachable. The terminal board now shows `Match complete` instead of a stale `Open attack` prompt, and the empty finished talon has no oversized placeholder.
- Blackjack mobile: three-seat insurance and player-action screens passed at `375×667`; AI hands use readable overlapped mini-cards, controls remain visible, and the empty AI/insurance dock is suppressed. A four-seat player screen passed at `375×600` and `390×844`; settlement actions use two full-width columns.
- Regression viewports: both games passed at `390×844` iPhone emulation and `1194×834` iPad-landscape emulation. Document dimensions matched the viewport; no card clipping, action overlap, or horizontal overflow.
- Rendered flows: Blackjack lobby → three-seat deal → human action → settlement passed. A three-seat Durak lobby-to-terminal match passed; rendered Push onward worked by tap and touch-pointer drag. All physical-card audits passed and browser errors were zero.
- Vision review passed for the compact Durak defender, Blackjack insurance/player/settlement screens, 390px iPhone layouts, iPad landscape layouts, and the highlighted Durak push drop target.
- `node --check`, JSON validation, static audit, and `git diff --check`: **PASS**.
- Evidence: `proofs/self-test-0.2.6.2.json`, `proofs/durak-0.2.6.2-stress.json`, `proofs/durak-0.2.6.2-push-qa.json`, `proofs/browser-qa-0.2.6.2.json` (with `-layouts` and `-ui-flows` details), `proofs/static-audit-0.2.6.2.txt`, and `proofs/deckforge-0.2.6.2-*.png`.

`plan.md` records implementation and measured acceptance evidence. Currency is fictional and local-only. Physical iPhone/iPad Safari and real audio were not tested; responsive checks used Chromium device emulation and synthetic touch-pointer flows.
