# DeckForge

DeckForge is an offline single-file card room for iPhone, iPad, and desktop browsers.

## Games

- **Blackjack** — a real six-deck shoe with physical card identities, hit, stand, double, split, insurance, natural Blackjack 3:2, dealer soft-17 rules, discard tracking, and shoe reshuffles.
- **Durak** — a complete configurable 2–4-seat Podkidnoy match with selectable 24/36/52-card decks, trump, same-rank multi-card attacks/throw-ins and optional transfer, tap and drag-and-drop controls, defenses, takes, draw order, and AI opponents.
- **Mau-Mau** — configurable 2–4-seat shedding game with 32/36/52/64-card packs, 5/6/7-card hands, three scoring systems, suit/rank play, physical-card audit, calls, draw/skip/stack/reverse rules, Jack suit wishes, and the fixed-round Idioten-Mau variant. House rules are normalized, explained with `?` help, and stored locally.

Mau-Mau has no single official rulebook; rule defaults and source disagreements are documented in [`references/mau-mau-rules.md`](references/mau-mau-rules.md).

## Bankroll and progression

The fictional bankroll, wins, table settings, and transaction history persist in `localStorage`. Each game has four stake tables and wins unlock higher tables. A new wager made at a zero balance grants exactly `$100` before the wager is deducted. Switching games or leaving the page cancels an unfinished round and returns its stake; the completed stats remain saved. In Durak, first place receives the full pot (`players × stake`); later finishers who escape the Durak get only their own ante back, and the last player loses their ante. A draw returns the human stake. In Mau-Mau, the match winner receives one ante from every seat, a human loss loses only the human ante, and a draw returns it. Mau-Mau Idioten matches use the same match stake across their fixed rounds. A reload opens the home screen and restores the saved stats/settings, but does not resume a live hand. The `Reset local save` control is the intentional deletion path. On the first DeckForge-branded load, the pre-DeckForge local save is cleared once so the renamed app starts clean; stats created afterward persist normally.

## Run

Open `deckforge.html` directly in a browser. No server, install, build, font, image, or network connection is required.

## Release and verification

Current release: **0.2.6.7** (`v0.2.6.7`), adding configurable Mau-Mau and correcting multiplayer Durak placement payouts. Previous release: `v0.2.6.6`; older header release `v0.2.6.56` remains unchanged, and the earlier mistaken `v0.0.0.1` release is superseded.

- **Durak decks/rules:** 24 (9–Ace), 36 (6–Ace), or 52 (2–Ace) cards; 2–4 seats; exclusive Throw-in/Transfer/Off choice. At 24 cards/four seats there is no talon; the last dealt card sets trump.
- **Durak payouts:** Finish order is tracked separately from the Durak loser. First place receives the full pot; later non-last finishers get their ante back; the last Durak loses it; draws refund it. Only first place advances progression.
- **Mau-Mau:** 2–4 seats; 32/36/52/64-card packs; 5/6/7-card hands; multiple scoring modes, common special-card/call variants, and Idioten-Mau. Rule sources and defaults: `references/mau-mau-rules.md`.
- **Diagnostics:** **121/121 passed**, 0 failures and 0 browser errors.
- **Durak stress:** **360 matches / 30 configurations / 15,430 audited actions**, 0 failures; all terminated in 22–97 steps, including 49 draws. All 311 non-draw matches had complete, unique finish orders with the final Durak last. Proof: `proofs/durak-stress-0.2.6.7.json`.
- **Manual UI:** Mau-Mau 32-card match exercised Jack wish, “Mau” and “Mau-Mau”; player won with 32/32 cards audited. Three-seat Durak second place returned only the `$10` ante (`$0` net); progression did not count a win; 36/36 cards audited.
- **Responsive/Vision:** Active Mau-Mau table at 320×800, 390×844 and 1024×768. The real Idioten-Mau setup (18 advanced controls; 3/5/7 rounds) was checked at 320×800, 390×844, 834×1194, 1024×768, 1194×834 and 1366×900; 0 label/button overflows and no document width exceeded its viewport. Durak second-place result reviewed at 390×844. Chromium only; physical Safari was not tested.
- **Static audit:** `deckforge.html` is **423,139 bytes / 2,833 lines**, one inline script, 58 unique IDs and 51 unique literal DOM references; no missing/duplicate IDs, external assets/URLs, or inline event attributes. `node --check`, proof JSON/PNG validation, static audit, and `git diff --check` passed.
- **Artifacts:** `RELEASE_NOTES_0.2.6.7.md`, `releases/DeckForge-0.2.6.7.zip`, and release proofs in `proofs/`.

`plan.md` records implementation and measured acceptance evidence. Currency is fictional and local-only.
