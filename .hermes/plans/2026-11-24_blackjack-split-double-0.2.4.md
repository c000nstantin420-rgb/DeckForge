# Blackjack split/double repair — DeckForge 0.2.4

## Baseline

- Start from the clean `v0.2.3` release.
- Seed `52` reproduces the defect: split hands are 3♦+A♠ (14 soft) and 3♦+K♣ (13); doubling Hand 1 draws one card but leaves it `stood=false`, `canAct=true`, `activeIndex=0`, with Hit/Stand still enabled.
- At 390×844 both split hand zones fit, but the active hand has no strong visual treatment; the action note does not identify which hand it controls.

## Behavior contract

- Double doubles only the current hand's wager, draws exactly one physical card, then closes that hand unconditionally. Hit/Stand/Double/Split must be illegal for the doubled hand. If another split hand can act, advance to it and keep the round open; otherwise settle.
- Preserve current table rules: split only an exact rank pair, one split per round, split Aces get one card and stand, and 21 after a split is not a natural Blackjack.
- Each split hand retains its own cards, wager, status, and settlement; card audit must pass after split, double, and final settlement.
- Make split state obvious: distinct `YOUR TURN`, `UP NEXT`, `STOOD`, `DOUBLED`, or `BUST` status, strong active-hand styling, and an action prompt naming the active hand.
- Fit and remain readable at 390×844 and 1024×768 without clipping or horizontal overflow.

## TDD and verification

1. Add deterministic tests that fail on v0.2.3 for double-under-21 locking, action rejection, split-hand advancement, split-Ace behavior, and split 21 payout.
2. Fix the engine; rerun all diagnostics and card audits.
3. Exercise rendered split → double → next-hand actions with actual browser clicks; verify the old doubled hand is locked and the next hand is visibly active. Capture mobile and desktop/tablet evidence and zero-error/viewport measurements.
4. Run static checks, update proof JSON and release notes, rebuild `DeckForge-0.2.4.zip`, verify its contents against the worktree, then commit, tag, push, publish, and read back the release/Pages deployment.

## Results

- v0.2.3 red run: **40 passed / 2 failed**; the failures were the sub-21 double leaving the round open and a doubled split hand retaining `activeIndex=0`.
- v0.2.4 final diagnostics: **44 passed / 0 failed**; seeded split-Ace and split-21 payout tests pass.
- Final source: 171,507 bytes / 1,589 lines; one inline script; no external assets, duplicate IDs, missing DOM references, inline handlers, or `node --check` errors.
- Real browser flows: a solo double below 21 settles with only `Deal next hand` / `Choose another table`; split → double closes Hand 1 and activates Hand 2; split settlement shows two compact per-hand net badges plus one aggregate banner.
- Responsive flows passed at 390×844 for solo and 4-seat games and 1024×768 tablet; table-zone scrollHeight equals clientHeight and page errors are empty.
- Evidence: `proofs/blackjack-split-double-qa.json` and six captured mobile/tablet screenshots. Physical iPhone/iPad Safari and real audio remain untested.
