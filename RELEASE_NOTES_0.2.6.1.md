# DeckForge 0.2.6.1

**Tag:** `v0.2.6.1`
**Type:** Durak hand polish, batched attacks, touch drag-and-drop, and full-pot settlement

## Changes since 0.2.6

- Reduced Durak hand cards to a compact, consistent card size with a correct playing-card aspect ratio. A normal six-card hand stays centered with clear gaps; larger hands scroll instead of squeezing or covering card corners.
- Added same-rank multi-select for opening attacks and throw-ins. Tap to add/remove cards, reject mixed ranks, show the selected count/rank, and play the batch from a clearly labeled action button. Throw-in batches require a selected covered pile and keep every physical card linked to that target.
- Added batch drag-and-drop: selecting several cards and dragging one of them onto the table plays the selected batch. Single-card actions remain supported.
- Made the enabled `Push onward` button a direct drop target. Dragging a legal matching-rank card onto it gives the target a strong amber highlight and executes the transfer; tapping the button still works.
- Changed Durak settlement so the winner receives one ante from every configured seat (`players × stake`). A human loss forfeits only the human ante; a draw or stalemate returns it. The result shows both the gross pot returned and net profit, and the bankroll header refreshes immediately.
- Updated the visible header badge and runtime assertion to `v0.2.6.1`.

## Features retained from 0.2.6

- Compact-landscape Durak layouts and the action dock remain inside the visual viewport, including four-seat tables with six attack pairs.
- Eligible human throw-ins can be queued against an explicit covered pile while an AI acts. Queueing does not steal the turn or remove the card; cancel, Continue AI, stale-target cleanup, and the 12-second timeout remain supported.
- `Push onward` is optional and separate from defense/take. Blackjack and Durak offer table-bounded next-round wager controls. Repeated complete Durak positions settle as a stalemate draw with the human ante returned.

## Launch

Open `deckforge.html` directly in a browser. No server, install, build step, external assets, or network connection is required.

## Verification

- In-app diagnostics: **75 passed / 0 failed**.
- Durak stress: **168/168 seeded matches** across 14 valid 2–4-seat rule configurations; every move preserved the 36-card audit and every match reached a legal terminal result.
- Fresh browser flow: actual Durak lobby setup → start → legal player actions → settlement completed with zero browser errors.
- Browser flows covered same-rank opening batches by tap and drag, targeted multi-card throw-in, single-card regression, Push onward by tap and drag, queue/continue/timeout, and a four-seat pot credit through the result action.
- Four-seat payout: with four `$10` antes, the actual winner credit was `$40`; the bankroll moved from `$250` to `$280` after the `$10` ante, the banner showed `+$30` and `$40 returned`, and the top bankroll header refreshed to `$280`.
- Layout geometry passed at **390×844 (DPR 3), 1024×768, 1024×600, 1194×834 (DPR 2), 1194×700, 1366×768, and 1366×600**. Six cards had no overlap or clipping, the hand stayed above the dock, all dock bounds were inside the visual viewport, and the document matched viewport dimensions.
- Vision review passed for iPhone-emulated `390×844`, iPad-landscape-emulated `1194×834`, the Push onward drag target, and the four-seat pot settlement.
- `node --check`, JSON validation, static audit, and `git diff --check`: **PASS**.
- Evidence: `proofs/self-test-0.2.6.1.json`, `proofs/durak-0.2.6.1-stress.json`, `proofs/durak-0.2.6.1-layout.json`, `proofs/browser-qa-0.2.6.1.json`, and `proofs/deckforge-0.2.6.1-*.png`.

## Known limits

- Physical iPhone/iPad Safari and hardware touch were not tested; responsive screenshots used Chromium device emulation and drag-handler checks used synthetic touch `PointerEvent`s.
- Real audio hardware was not tested. Currency is fictional and local-only; additional seats are AI-controlled.
