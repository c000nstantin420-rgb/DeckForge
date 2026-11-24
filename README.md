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

Current release: **0.2.6.55** (`v0.2.6.55`), a small UI patch over `0.2.6.5`. The earlier mistaken `v0.0.0.1` release remains superseded.

- **Header:** A stylized `DeckForge` wordmark is centered at the top and contains no icon or version label. It is the Home/menu button; clicking it from either game tab, a lobby, or a live round returns to Home. Blackjack and Durak shortcuts remain at the left; bankroll and utility controls remain at the right.
- **Version:** `v0.2.6.55` remains visible outside the centered wordmark beside the game shortcuts.
- **Responsive QA:** Wordmark center measured within `0.01px` of the content center at `360`, `375`, `390`, `834`, `1024`, and `1194px`; no horizontal overflow or header overlap. At `320px`, a compact version badge remains visible and the header still fits.
- **Navigation flows:** Brand returned Home from Blackjack and Durak lobbies and from live Blackjack/Durak states. Active physical-card audits stayed valid; no browser errors. Evidence: `proofs/center-brand-0.2.6.55.json` and versioned header screenshots in `proofs/`.
- **Vision:** Reviewed Home and active-game headers at phone `390×844` and iPad portrait/landscape; centered wordmark and side groups stayed clear and unclipped. Physical Apple hardware/Safari was not available; tests used Chromium emulation.
- In-app diagnostics: **83 passed / 0 failed**.
- `deckforge.html` static audit: **297,225 bytes / 2,262 lines**, one inline script, 80 literal DOM refs; no missing/duplicate IDs, external assets/URLs, or inline event attributes. `node --check`, JSON/PNG validation, static audit, and `git diff --check`: **PASS**. Proof: `proofs/static-audit-0.2.6.55.json`.
- Release artifacts: `RELEASE_NOTES_0.2.6.55.md` and `releases/DeckForge-0.2.6.55.zip`.

`plan.md` records implementation and measured acceptance evidence. Currency is fictional and local-only.
