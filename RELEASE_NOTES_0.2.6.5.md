# DeckForge 0.2.6.5

- **Tag:** `v0.2.6.5`
- **Type:** Patch release — Blackjack round recovery and bankroll analytics
- **Version change:** `0.2.6.3` → `0.2.6.5` (requested target; `.4` intentionally skipped)
- **Previous release:** `v0.2.6.3`. The earlier mistaken `v0.0.0.1` tag remains superseded; no existing tag was moved.

## Changes since v0.2.6.3

- **Blackjack round recovery:** Starting a new hand now resets the old `settled` phase before resolving the new deal. Previously, an immediate dealer natural could see the previous round's settled guard and produce no settlement. The fix records the new result and `lastRound`, shows one outcome, and leaves the next-hand action usable.
- **Bankroll dashboard:** Replaced the repetitive summary with balance, saved Blackjack/Durak wins, and bankruptcy grants; added gross income-versus-expenses bars, net cash flow, a balance trend with a clear low–high range, opening/closing ledger reconciliation, and paired inflow/outflow charts for all eight categories.
- **Ledger clarity:** Category charts distinguish Blackjack, Durak, insurance, double, split, bankruptcy grants, refunds/reversals, and other transactions. Recent activity shows the newest 12 rows. Analytics use the latest 80 saved transactions, order equal timestamps deterministically, and handle missing timestamps without epoch labels.
- **Responsive behavior:** The dashboard scrolls within its modal and remains legible at mobile, iPad portrait, and iPad landscape sizes. Empty histories have explicit no-data copy and a reconciled current balance.

## Launch

- Open `deckforge.html` directly in a browser; the app remains offline and single-file.
- Live site: <https://c000nstantin420-rgb.github.io/DeckForge/>
- Currency is fictional and stored locally.

## Verification

- In-app browser diagnostics: **82/82 passed**, 0 failed, 0 page errors.
- Blackjack regression UI: at `390×844`, round 1 settled, round 2 was forced to dealer `10♣ A♣` against player `4♠ 4♥`, exactly one “Dealer Blackjack” result rendered, and round 3 returned to an actionable player phase. Both round audits passed; 0 runtime errors.
- Durak stress: **168/168 seeded matches** across 14 configurations; each move audited, legal terminal results, 0 failures, 32–73 steps.
- Bankroll analytics fixture: 14 transactions across all eight categories reconciled `$0 + $154 = $154`; the empty save reconciled `$250 + $0 = $250`. Tests cover the 80-entry cap, stable equal/missing timestamps, empty/single histories, paired category bars, and trend labels.
- Vision review: mobile `390×844`, iPad portrait `834×1194`, and iPad landscape `1194×834`; overview, category, and recent-activity views showed no horizontal overflow, overlapping content, or clipped chart labels. Empty history geometry also passed at all three sizes.
- Version badge `v0.2.6.5` verified on Home and active Blackjack at `390×844`.
- Static audit: **295,371 bytes**, 2,252 lines, one inline script, 76 literal DOM references, no missing/duplicate IDs, external assets, inline event attributes, or URLs. `node --check`, proof JSON validation, PNG signatures/dimensions, and `git diff --check` passed.
- Evidence: `proofs/self-test-0.2.6.5.json`, `proofs/blackjack-reentry-0.2.6.5.json`, `proofs/durak-0.2.6.5-stress.json`, `proofs/bankroll-layout-0.2.6.5.json`, `proofs/bankroll-empty-0.2.6.5.json`, `proofs/version-badge-0.2.6.5.json`, `proofs/static-audit-0.2.6.5.json`, and 13 versioned PNG screenshots in `proofs/`.

## Known limits

Physical iPhone/iPad Safari and hardware audio were not tested. Visual review used Chromium device emulation; the regression fixture used deterministic cards only in an isolated browser. The app has no network dependency at runtime.
