# Durak Placement Payout Fix

> Initial scope: keep this as an unversioned fix until the user explicitly requests a release. The user later authorized v0.2.6.7; include this fix after all release gates pass.

**Goal:** Track multiplayer Durak finishing order and ensure the full table pot is awarded only to first place.

**Architecture:** Keep `winner` as the existing terminal compatibility field, but add an explicit `finishOrder`/human placement to `DurakGame`. Settle economy and render the result from placement: first gets the full pot; any non-last place gets only their own ante back; the final Durak loses their ante; draws return the ante.

**Tech Stack:** `deckforge.html`, inline JavaScript, deterministic in-app tests, Chromium browser QA.

---

## Scope and acceptance ledger

| Concern | Existing defect | Acceptance proof |
|---|---|---|
| Finish order | Engine stores only “human is Durak” vs “human escaped”; it cannot distinguish 1st from 2nd in 3+ seats. | A real-card three-seat fixture reaches `finishOrder: [AI, human, Durak]` and reports human place 2. |
| Payout | `durakOutcomeValues` awards `wager × players` to every human who is not the Durak. | First place alone receives the full pot; second receives own wager only (net 0); last gets 0; draw returns own wager. |
| UI | All survivors are labeled “You escaped” and shown as collecting the full pot. | Second-place outcome names the place and reports stake returned, not pot won; first and last retain clear results. |
| Compatibility | Two-seat winner, loss, draw, bankroll header, and next-round wager must remain correct. | Engine/UI regressions for two, three, and four seats; settlement tests preserve the completed wager. |
| Release boundary | The user explicitly authorized `0.2.6.7` after the fix was confirmed. | Include the tested payout correction in `v0.2.6.7`; preserve `v0.2.6.6` and verify the tag, asset, Pages deploy, and live bytes. |

## Tasks

1. Add failing deterministic tests for 3-seat order/payout/UI and payout by place in 2/3/4-seat cases. Use physical cards from the shoe and preserve the deck audit.
2. Implement `finishOrder` tracking when the talon is empty, including batch attacks, defense, transfer, AI out-seat handling, and end-of-round draw-up order.
3. Derive placement settlement and render first / second / Durak outcomes using the captured wager, not the next-round stake.
4. Run `node --check`, `git diff --check`, the complete in-app test suite, targeted UI/payout checks, a fresh-browser Durak result flow, bankroll persistence/header assertions, and 320×800/390×844/1024×768 geometry checks if the result UI changes layout.
5. Verify that only intended hunks changed; leave all release actions untouched.

## Risks

- A player may empty a hand while the talon still contains cards and draw back up; only record permanent finishes once the talon is empty.
- Several seats may become empty during the same bout. Record candidate finish events in action order, but commit their places only once the bout resolves—a cardless defender may still take cards. Record any seats still empty after talon draw-up in the engine’s explicit draw order.
- Keep stalemate/all-empty draws as ante returns and preserve existing two-seat payout semantics.
