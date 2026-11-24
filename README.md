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

Current release: **0.2.6.1** (`v0.2.6.1`). The version is visible in the site header on the home screen and both game screens.

- In-app diagnostics: **75 passed / 0 failed**.
- Durak stress: **168 seeded matches** across 14 valid 2–4-seat throw-in/push configurations; every move preserved the 36-card audit and every match reached a legal terminal result.
- Rendered UI: actual lobby → start → human actions → settlement flow completed without browser errors. Same-rank opening batches worked by tap and drag; targeted multi-card throw-in inserted beside the selected pile; Push onward worked by tap and drag. Queue, Continue AI, timeout, and stale-plan paths were exercised.
- Payout: a four-seat `$10` ante winner received a `$40` credit, showed `+$30` / `$40 returned`, and the bankroll header refreshed to `$280` from `$250` after the `$10` ante was deducted.
- Layout geometry: **390×844 DPR3, 1024×768, 1024×600, 1194×834 DPR2, 1194×700, 1366×768, and 1366×600**. Six cards had no overlap or clipping, the hand stayed above the dock, every dock stayed inside the visual viewport, and the document matched the viewport.
- Vision review passed for the iPhone-emulated hand, iPad-landscape hand, highlighted Push onward drop target, and four-seat settlement screen.
- `node --check`, JSON validation, static audit, and `git diff --check`: **PASS**.
- Evidence: `proofs/self-test-0.2.6.1.json`, `proofs/browser-qa-0.2.6.1.json`, `proofs/durak-0.2.6.1-stress.json`, `proofs/durak-0.2.6.1-layout.json`, `proofs/static-audit-0.2.6.1.txt`, and `proofs/deckforge-0.2.6.1-*.png`.
- Historical Blackjack split/double evidence and previous release artifacts are retained.

`plan.md` records implementation and measured acceptance evidence. Currency is fictional and local-only. Physical iPhone/iPad Safari and real audio were not tested; responsive checks used Chromium device emulation and synthetic touch-pointer flows.
