# DeckForge 0.2.6 — landscape Durak actions and planned throw-ins

## Baseline / reproduction

- Start from release `v0.2.5` (`5afa449`). Keep the version at `0.2.5` until all acceptance gates pass.
- On a four-seat, six-pair Durak defense fixture, the dock is visible at `1024×768` (buttons y=670–716) and `1024×600` (y=502–548), but clipped at `1194×700` (y=678–724; viewport bottom 700) and `1366×600` (same y positions). The width-only `max-width:1100px` dock rule misses short iPad landscape sizes.
- Current AI turns hide all Durak action controls, and throw-in actions have no target pile: `attackCard()` always appends a pair; `selectedTarget` is only honored for defense. The UI may let a hand card/stack be selected during AI turns, but gives no clear pending-action state and AI pacing remains 320–760ms.

## Behavior contract

- Keep the complete Durak action dock within the visual viewport for iPad landscape and short-wide layouts, including a max six-pair table, hand, and 2–4 seats. The play area may scroll internally; controls must remain usable and not overlay the hand.
- When the human is eligible to throw in while an AI seat is taking its turn, show a clear reaction window and let the human preselect a legal card and target pile without changing the engine state out of turn. Preserve clockwise turn order; execute only when seat 0 becomes the legal actor. Provide a clear pass/continue path and invalidate stale queued choices safely.
- A player throw-in requires an existing fully covered table, a matching rank, an active eligible seat, remaining attack capacity, and an explicit target pile. Insert the new attack adjacent to its selected target and keep a visible/accessibility link to that pile. AI may choose a deterministic legal pile automatically.
- When the human can push, show an explicit optional “Push onward” choice if any legal push card exists, even before selecting it. The action must not be automatic; the user can still defend or take. Preserve the one-push/bout and next-defender capacity rules.
- On a settled Blackjack hand or finished Durak duel, show a next-round stake selector beside the rematch button. Only stakes within the current table bounds are offered; new wager changes must not rewrite the completed round’s settlement display or payout.
- If an identical complete Durak position repeats after the talon is empty, resolve it as an explicit stalemate draw and return the current duel stake. Show the reason accurately; do not call a held-card stalemate an all-empty draw.
- Keep the engine deterministic and UI-independent; seeded tests cover rules and state, browser tests cover queue/window, pile targeting, push-choice UI, next-round wagers, stalemate presentation, and viewport geometry.

## TDD / acceptance gates

1. Add failing engine regressions for throw-in target validation/insertion/link, invalid/missing targets, AI target selection, and no mutation while the human preselects during another seat’s turn.
2. Add failing UI-helper tests for the human reaction window and the unselected push suggestion; verify choices remain distinct from automatic moves.
3. In a fresh browser, measure the four-seat six-pair action dock at 390×844, 1024×768, 1024×600, 1194×834, 1194×700, 1366×768, and 1366×600. Assert every visible action button is fully within `visualViewport`, the hand does not overlap the dock, and table content scrolls only internally.
4. Exercise a 4-seat `all players` throw-in bout with several AI seats before the human: preselect a card and pile while AI acts, confirm no out-of-turn move, then let AI hand control over and execute the queued throw-in on the same pile. Verify a stale/illegal selection is rejected and a manual Continue/Passthrough action remains available.
5. Exercise optional push with no card preselected, then choose push, defense, and take in distinct fixtures; audit all 36 physical cards after every action.
6. Exercise next-round stake selection on settled Blackjack and Durak screens; change the next bet, start the next game, verify the new debit/wager, then force a rerender and confirm the prior Durak payout still reflects the completed duel’s stake.
7. Drive a deterministic repeated-position endgame through the engine and rendered result; assert stalemate copy, exact stake return, and a valid 36-card audit.
8. Run the full in-app suite, 2–4-seat Durak stress matrix, full browser flows, visual screenshots, `node --check`, JSON/static audit, and `git diff --check`. Update README, plan, release notes, proof JSON, and screenshots only after all tests pass.
9. Build the explicit `DeckForge-0.2.6.zip`, compare packaged runtime bytes, commit/tag, publish, and verify the release asset, both deploy runs, latest Pages build `built`, live `v0.2.6` badge, exact public runtime bytes, and clean working tree.

## Final verification snapshot

- In-app diagnostics: 69/69; static audit and `node --check` pass.
- Durak stress: 168/168 autonomous matches over 14 valid configurations, audited every action; separate seed `261201` terminates as a stalemate draw at step 77 with all 36 cards accounted for.
- Rendered UI: complete 2-, 3-, and 4-seat matches; targeted throw-in, target-pile selection, stale queue clearing, touch-pointer drag, optional push, Continue AI, and timeout resume all exercised.
- Next-round stake controls: Blackjack and Durak start the next round with the selected `$20` wager; the prior Durak settlement remains based on the original `$10` wager after rerender.
- Landscape geometry: six-pair action controls pass at 390×844, 1024×768, 1024×600, 1194×834, 1194×700, 1366×768, and 1366×600. All controls are inside `visualViewport`; hand stays above the dock; no document overflow.
- Version badge: `v0.2.6` verified on home and active game screens. Physical iPad Safari and real audio remain untested.

## Limits

- Physical iPad Safari and real audio are only claimable if actually exercised. Chromium iPad/mobile emulation and touch-pointer handler tests must be labeled as such.
- Do not bump or publish `0.2.6` before every acceptance gate is green.
