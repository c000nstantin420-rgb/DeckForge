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

Current release: **0.2.6.56** (`v0.2.6.56`), a correction to the header in `0.2.6.55`. The earlier mistaken `v0.0.0.1` release remains superseded.

- **Top bar:** Removed the Blackjack/Durak mode shortcuts entirely. It now has only the version badge at left, the centered DeckForge Home button, and utilities at right. Game selection remains on the Home cards.
- **Navigation:** Both Home game cards still open their games; the centered wordmark returns Home from Blackjack/Durak lobbies and active rounds. Card audits and browser errors passed. Proof: `proofs/topbar-mode-removal-0.2.6.56.json`.
- **Responsive QA:** Wordmark center stays within `0.01px` of center at `320`, `360`, `375`, `390`, `414`, `834`, `1024`, and `1194px`. No header overlap or horizontal overflow; version remains visible.
- **Vision:** Reviewed mobile Home/active game and iPad portrait/landscape Home/active screens; no visible top-bar mode controls or header collisions. Physical Apple hardware/Safari was not available; tests used Chromium emulation.
- In-app diagnostics: **83 passed / 0 failed**.
- `deckforge.html` static audit: **295,759 bytes / 2,253 lines**, one inline script, 78 literal DOM refs; no missing/duplicate IDs, external assets/URLs, or inline event attributes. `node --check`, JSON/PNG validation, static audit, and `git diff --check`: **PASS**. Proof: `proofs/static-audit-0.2.6.56.json`.
- Release artifacts: `RELEASE_NOTES_0.2.6.56.md` and `releases/DeckForge-0.2.6.56.zip`.

`plan.md` records implementation and measured acceptance evidence. Currency is fictional and local-only.
