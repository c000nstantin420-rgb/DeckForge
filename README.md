# DeckForge

DeckForge is an offline single-file card room for iPhone, iPad, and desktop browsers.

## Games

- **Blackjack** — a real six-deck shoe with physical card identities, hit, stand, double, split, insurance, natural Blackjack 3:2, dealer soft-17 rules, discard tracking, and shoe reshuffles.
- **Durak** — a complete configurable 2–4-seat Podkidnoy match with selectable 24/36/52-card decks, trump, same-rank multi-card attacks/throw-ins and optional transfer, tap and drag-and-drop controls, defenses, takes, draw order, and AI opponents.

## Bankroll and progression

The fictional bankroll, wins, table settings, and transaction history persist in `localStorage`. Each game has four stake tables and wins unlock higher tables. A new wager made at a zero balance grants exactly `$100` before the wager is deducted. Switching games or leaving the page cancels an unfinished round and returns its stake; the completed stats remain saved. In Durak, the winner collects one ante from every seat (`players × stake`); a human loss loses only the human ante, and a draw returns it. A reload opens the home screen and restores the saved stats/settings, but does not resume a live hand. The `Reset local save` control is the intentional deletion path. On the first DeckForge-branded load, the pre-DeckForge local save is cleared once so the renamed app starts clean; stats created afterward persist normally.

## Run

Open `deckforge.html` directly in a browser. No server, install, build, font, image, or network connection is required.

## Release and verification

Current release: **0.2.6.6** (`v0.2.6.6`), the requested Durak deck/rule update. The published `v0.2.6.56` header release remains unchanged; the earlier mistaken `v0.0.0.1` release is superseded.

- **Decks:** Durak setup offers 24 cards (9–Ace), 36 (6–Ace), or 52 (2–Ace), four suits each. At 24 cards/four seats there is no talon; the last dealt card sets trump.
- **Optional rule:** One exclusive selector: Throw-in, Transfer, or Off (both disabled). Throw-in keeps Neighbors/All players scope; All players unlocks at four seats. Transfer chains same-rank passes before any cover, constrained by six cards and the next defender’s hand.
- **Navigation/UI:** Settings persist; table subtitle and setup summary stay synchronized. The Home game card starts the selected table; a rendered Transfer action passes the defense onward. Evidence: `proofs/durak-config-ui-0.2.6.6.json`, `proofs/durak-transfer-action-0.2.6.6.json`, and `proofs/durak-ipad-0.2.6.6.json`.
- In-app diagnostics: **89 passed / 0 failed**.
- Seeded Durak stress: **360 matches / 30 configs / 15,398 audited actions**, across all three decks and 2–4 seats; **0 failures**, 22–101 steps, under a 1,500-step bound. Evidence: `proofs/durak-stress-0.2.6.6.json`.
- **Vision:** Phone 320×800 and 390×844, iPad portrait 834×1194/landscape 1194×834 reviewed. The 24-card/four-seat zero-talon trump display, 36-card Off deal, 52-card Transfer action, and four-seat throw-in selectors were checked. A 320px tier-title/lock collision and cramped Transfer/`DEFEND` copy were corrected. Chromium emulation only; physical Apple Safari was not tested.
- `deckforge.html` static audit: **307,131 bytes / 2,289 lines**, one inline script, 87 literal DOM refs; no missing/duplicate IDs, external assets/URLs, or inline event attributes. `node --check`, JSON/PNG validation, static audit, and `git diff --check`: **PASS**. Proof: `proofs/static-audit-0.2.6.6.json`.
- Release artifacts: `RELEASE_NOTES_0.2.6.6.md` and `releases/DeckForge-0.2.6.6.zip`.

`plan.md` records implementation and measured acceptance evidence. Currency is fictional and local-only.
