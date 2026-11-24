# DeckForge 0.2.6.2

- **Tag:** `v0.2.6.2`
- **Type:** Short-phone Durak/Blackjack layout, settlement dock, and drag-ghost polish

## Changes since 0.2.6.1

- Fixed the clipped three-seat Durak hand on short iPhones. Opponent status is condensed into two columns, the trump/talon bar keeps the deck artwork clear of its count, and the six-card hand stays above a single-row Beat / Push / Take dock.
- Rebalanced compact Blackjack for short screens. Dealer, AI seats, player hand, and decisions remain visible; AI cards use a readable 8px overlap instead of wrapping into extra rows. The player action row stays compact, and the insurance/AI phase no longer reserves an empty action dock.
- Kept the Blackjack result controls usable on phones: the next-wager selector spans the dock, with `Deal next hand` and `Choose another table` side-by-side at full width.
- Fixed the 390px iPhone Blackjack overflow by preventing the dealer/player panels from flex-shrinking and keeping multi-card AI rows within their seat boxes.
- Raised and slightly reduced the Durak drag ghost so it does not obscure the highlighted `Push onward` label.
- Fixed the Durak terminal view so a finished match no longer prompts `Open attack`; it shows `Match complete` and drops the oversized empty-talon card placeholder while retaining the `0 cards left` count.
- Preserved existing 0.2.6.1 rules and behavior: physical card identities, 2–4-seat tables, same-rank batches, tap/drag interactions, optional push, queueable throw-ins, and all-seat pot settlement.

## Launch

Open `deckforge.html` directly in a browser. No install, server, external asset, or network connection is required.

## Verification

- In-app diagnostics: **75/75**, zero failures.
- Durak stress: **168/168 seeded matches** across 14 valid 2–4-seat throw-in/push variants; audits passed after every move and all matches reached a legal terminal state.
- At `375×667`, three-seat Durak displayed all six cards and all three actions; the hand ended at `y=611` and the dock began at `y=614`. A four-seat `375×600` check also kept every card and action in view.
- At `375×667`, three-seat Blackjack insurance and player states displayed dealer, AI hands, player cards, and the relevant controls without page overflow. The four-seat `375×600` player state also fit.
- At `390×844`, three- and four-seat Blackjack passed with AI rows fitting their seat boxes, full player hands, and one-row action controls. Both games also passed at `390×844` and `1194×834` iPad-landscape emulation.
- Fresh rendered flows passed: three-seat Blackjack lobby → deal → human action → settlement; three-seat Durak lobby → legal actions → terminal result. A Durak push worked by tap and by drag-to-button; the 36-card audit stayed valid.
- Vision reviewed the compact Durak defender, Blackjack insurance/player/settlement states, 390px iPhone layouts, 1194×834 iPad layouts, and the highlighted Durak push drop target.
- `node --check`, static audit, JSON validation, and `git diff --check`: **PASS**.
- Evidence: `proofs/self-test-0.2.6.2.json`, `proofs/durak-0.2.6.2-stress.json`, `proofs/durak-0.2.6.2-push-qa.json`, `proofs/browser-qa-0.2.6.2.json` (with `-layouts` and `-ui-flows` details), `proofs/static-audit-0.2.6.2.txt`, and `proofs/deckforge-0.2.6.2-*.png`.

## Known limits

- Physical iPhone/iPad Safari and hardware audio were not tested. Layout checks used Chromium device emulation; drag integration used synthetic touch `PointerEvent`s. Currency remains fictional and local-only.
