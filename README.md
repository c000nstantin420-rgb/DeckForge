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

Current release: **0.2.6** (`v0.2.6`). The version is visible in the site header on the home screen and both game screens.

- In-app diagnostics: **69 passed / 0 failed**.
- Durak stress: **168 seeded matches** across 14 valid 2–4-seat throw-in/push configurations; every move preserved the 36-card audit and every match reached a legal terminal result. A repeated full position is resolved as a visible stalemate draw with the duel stake returned.
- Rendered UI: complete 2-, 3- and 4-seat Durak matches; targeted throw-in queued while an AI acted and executed only when the human turn arrived; optional push, defense and take; stale queue invalidation; Continue AI and timeout resume.
- Durak landscape geometry: six-pair/four-seat table checked at **390×844, 1024×768, 1024×600, 1194×834, 1194×700, 1366×768 and 1366×600**. Every action button and the full dock stayed inside the visual viewport; hand stayed above the dock; no document overflow.
- Next-round wager selector was exercised from settled Blackjack and Durak screens. A new stake debited correctly, while the completed Durak payout display remained tied to its original wager.
- AI timing: throw-in planning extends the scheduled response to **1.4–2.2s**; after a choice is queued, AI returns to **320–760ms**. The 12-second planning timeout and manual Continue AI path both resume safely.
- Static audit: one inline script, `node --check` PASS, no external assets, duplicate IDs, missing DOM references or inline event attributes.
- Evidence: `proofs/self-test.json`, `proofs/browser-qa.json`, `proofs/static-audit.txt`, `proofs/deckforge-0.2.6-*.png`, plus layout and stress JSON proofs.
- Historical Blackjack split/double evidence remains in `proofs/blackjack-split-double-qa.json`; previous release notes and artifacts are retained.

`plan.md` records the implementation plan and measured acceptance evidence. Currency is fictional and local-only. Physical iPhone/iPad Safari and real audio were not tested; Chromium mobile emulation and synthetic `PointerEvent` touch-pointer flows were used.
