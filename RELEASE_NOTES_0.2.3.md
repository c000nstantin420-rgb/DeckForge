# DeckForge 0.2.3

**Tag:** `v0.2.3`  
**Type:** Bug-fix release

## Changes

- A completed single-hand Blackjack round now renders one overall settlement banner instead of duplicating the same result inside the hand and below it.
- Split Blackjack hands retain their individual outcome banners, alongside the overall settlement summary.
- Carries forward DeckForge 0.2.2: offline single-file runtime, physical Blackjack shoe and Durak deck, configurable AI seats, table-rule variants, and mobile table polish.

## Launch

Open `deckforge.html` directly in a browser. No install, build step, external assets, or network connection is required.

## Verification

- In-app diagnostics: **39 passed / 0 failed**.
- `node --check`: **PASS**.
- Static audit: 161,274 bytes, 1,531 lines, one inline script, no external assets, no duplicate IDs, no missing DOM references, and no inline event attributes.
- Real browser flow at 390×844: one-seat Blackjack deal → stand → settlement. The single-hand `.outcome-banner` count changed from **2 before the fix to 1**; both next-action buttons remained visible, document geometry stayed 390×844, and browser errors were zero.
- Seeded split fixture (`52`) settled two hands and rendered two per-hand results plus one aggregate summary (3 banners total).
- Regression proof: `proofs/banner-fix-qa.json`; screenshot: `proofs/blackjack-outcome-single-mobile.png`.
- Table-variant proof remains in `proofs/multiplayer-variants-qa.json`.

## Known limits

- Physical iPhone/iPad Safari and real audio hardware were not tested; Chromium mobile emulation was used.
- Currency is fictional and stored locally. Online multiplayer is not included; the multi-seat opponents are AI-controlled.
