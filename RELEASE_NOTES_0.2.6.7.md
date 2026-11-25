# DeckForge 0.2.6.7

- **Tag:** `v0.2.6.7`
- **Type:** Mau-Mau feature release with multiplayer Durak settlement correction
- **Previous release:** `v0.2.6.6`

## Changes

- Added **Mau-Mau** as the third game in Home. Supports 2–4 seats, 32/36/52/64-card packs, 5/6/7-card hands, AI opponents, physical-card audits, local settings, bankroll settlement, and progression.
- Added configurable scoring: round wins, penalty points, and points race; match/card-value choices; Mau/Mau-Mau call rules; missed-call penalties; Seven/Eight/Two stacks; Nine and Ace effects; Jack suit wishes and Jack-on-Jack/finish options; opening/final-card rules; and fixed-round **Idioten-Mau** (3/5/7 rounds).
- Researched the common baseline and conflicting house rules. Mau-Mau has no single governing rulebook; sources, defaults, and exact variant semantics are documented in [`references/mau-mau-rules.md`](references/mau-mau-rules.md).
- Reflowed the expanded Mau-Mau rule editor into a readable single-column list so long Ace/call labels stay clear in narrow sidebars and phone layouts.
- Fixed Durak multiplayer placement settlement. The engine now records finish order once the talon is empty and the bout resolves. First place alone receives the full pot; any later finisher before the Durak receives only their own ante back; the last Durak loses their ante; a draw returns the ante. Only first place adds a progression win.
- Durak result screens now show the actual place and payout. Removed the redundant settlement toast that covered rematch controls, and corrected singular/empty opponent card labels.

## Launch

- Open `deckforge.html` directly; the app remains a single offline HTML file.
- Live site: <https://c000nstantin420-rgb.github.io/DeckForge/>
- Currency is fictional and local-only.

## Verification

- In-app diagnostics: **121/121 passed**, 0 failures, 0 browser errors.
- Durak placement stress: **360 matches across 30 configurations**, **15,430 audited actions**, 0 failures; all terminated in 22–97 steps, including 49 draws. The 311 non-draw matches had complete, unique finish orders with the final Durak last.
- Manual Mau-Mau UI match: two seats, 32 cards, five-card deal, Jack suit wish, “Mau” and “Mau-Mau” calls; player won, `$20` returned on a `$10` ante, `$10` net profit; physical-card audit **32/32**.
- Manual Durak UI scenario: three seats, 36-card deck, `$10` ante; finish order was Player 3 → human → Player 2 (Durak). Human received `$10` back, `$0` net, no progression win; physical-card audit **36/36**. Proof: `proofs/durak-placement-payout.json`.
- Responsive/visual review: Mau-Mau active table at `320×800`, `390×844`, and `1024×768`; real Idioten-Mau setup opened and checked at `320×800`, `390×844`, `834×1194`, `1024×768`, `1194×834`, and `1366×900` (18 advanced controls, 0 label/button overflows); Durak second-place results at `390×844`. No page-width overflow; final hand card remains scroll-reachable and payout/rematch controls are unobstructed.
- Static audit: **423,139 bytes / 2,833 lines**, one inline script, 58 unique element IDs, 51 unique literal DOM references with no missing IDs, 0 external asset references, 0 inline event attributes, and 0 runtime HTTP URLs. `node --check`, proof JSON validation, PNG signatures/dimensions, and `git diff --check` passed.
- Chromium device emulation only; physical iPhone/iPad Safari and hardware audio were not tested.

## Evidence

- `proofs/self-test-0.2.6.7.json`
- `proofs/durak-stress-0.2.6.7.json`
- `proofs/mau-mau-manual-0.2.6.7.json`
- `proofs/mau-mau-responsive-0.2.6.7.json`
- `proofs/durak-placement-payout.json`
- `proofs/static-audit-0.2.6.7.json`
