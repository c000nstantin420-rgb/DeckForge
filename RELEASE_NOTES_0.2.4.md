# DeckForge 0.2.4

**Tag:** `v0.2.4`
**Type:** Blackjack bug-fix and split UI release

## Changes

- Double now draws exactly one card and closes that hand, even when its total remains below 21. Hit, Stand, Double, and Split are no longer legal on that hand.
- After doubling the first split hand, play advances to the next hand. The active hand is highlighted and the action prompt names it; completed hands show `DOUBLED`, `STOOD`, or their result.
- Split hands use a compact per-hand net badge plus one aggregate settlement banner. Mobile stacks hands; tablet places them side-by-side. Multi-seat AI lanes remain visible.
- Rules now state the existing split limits: exact matching rank, one split per round, split Aces receive one card and stand, and split 21 is not a natural.
- Carries forward the 0.2.3 fix for the duplicated single-hand settlement banner and the 0.2.2 table variants/mobile polish.

## Launch

Open `deckforge.html` directly in a browser. No install, build step, external assets, or network connection is required.

## Verification

- In-app diagnostics: **44 passed / 0 failed**.
- `node --check`: **PASS**; static audit: 171,507 bytes, 1,589 lines, one inline script, no external assets, duplicate IDs, missing DOM references, or inline event attributes.
- TDD reproduced the v0.2.3 bug with seed 52; the two double/advance regressions failed before the fix. All 44 checks pass on this release.
- Mobile 390×844: a solo double from 11 to 13 settles after one draw; only `Deal next hand` / `Choose another table` remain. Split→double closes Hand 1 and activates Hand 2; both hands and one aggregate result fit without overflow.
- Tablet 1024×768: split hands display side-by-side; cards, active badges, aggregate result, and action dock fit without clipping.
- Four-seat mobile split: all three AI seats, both split hands, per-hand loss amounts, one aggregate result, and next actions remain visible at 390×844.
- Structured proof: `proofs/blackjack-split-double-qa.json`; screenshots are listed there.

## Known limits

- Physical iPhone/iPad Safari and real audio hardware were not tested; Chromium emulation was used.
- Currency is fictional and stored locally. Online multiplayer is not included; additional seats are AI-controlled.
