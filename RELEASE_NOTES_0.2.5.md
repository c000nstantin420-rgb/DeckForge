# DeckForge 0.2.5

**Tag:** `v0.2.5`
**Type:** Blackjack multiplayer handoff and Durak correctness/UI release

## Changes since 0.2.4

- Fixed Blackjack's multi-seat AI scheduler handoff after both insurance choices. The AI now runs through all configured seats, returns control to the human, and settles the hand.
- Corrected Durak's clockwise attacker/defender transitions after a defended bout, a take, and a push. Refill order is attacker first and defender last; empty seats are skipped once the talon is empty.
- Fixed a four-seat `all players` throw-in loop: seats that pass are recorded for the bout and cannot be selected again. The AI also advances past empty attackers; any unexpected no-op stops visibly instead of repeating forever.
- Terminal Durak results now wait until bout end, identify the last player holding cards, and handle an all-empty draw. Push requires a legal next defender with enough cards to cover the table.
- Sorted the rendered Durak hand by suit/rank with all trumps last, without reordering engine cards. Improved table-card readability, red-suit color, compact trump/talon rendering, defense hit targets, and drag-target highlighting. The exposed trump is not duplicated after the talon empties.
- Added a visible `v0.2.5` header badge and accessible version label on home and game screens.

## Launch

Open `deckforge.html` directly in a browser. No install, build step, external assets, or network connection is required.

## Verification

- In-app diagnostics: **60 passed / 0 failed**.
- Durak: **100 seeded autonomous matches** across five 2–4-seat rule configurations; every step passed the 36-card audit and every match terminated.
- Rendered UI: seeded 2-, 3-, and 4-seat Durak matches reached a terminal result. Separate flows passed attack, throw-in, defense, push, take, a controlled human-loss result, a valid touch-pointer drop, and an invalid drop.
- Blackjack: both insurance choices completed in 3-seat tests; 4-seat insurance advanced through all three AI turns, returned to the human, and settled.
- Durak six-pair geometry passed at **390×844** and **1024×768** without document overflow or clipped action buttons.
- AI action-to-callback measurements: **328 ms** low, **546/547 ms** midpoint, **775 ms** high including browser/timer dispatch overhead; scheduled delay remains 320–760 ms.
- `node --check`: **PASS**. Static audit: one inline script, no external assets, duplicate IDs, missing DOM references, or inline event attributes.
- Evidence: `proofs/self-test.json`, `proofs/browser-qa.json`, `proofs/static-audit.txt`, and `proofs/deckforge-0.2.5-*.png`.

## Known limits

- Physical iPhone/iPad Safari and real audio hardware were not tested. Chromium mobile emulation and synthetic `PointerEvent` events with `pointerType="touch"` were used; CDP hardware-touch dispatch was unavailable.
- Currency is fictional and local-only. Online multiplayer is not included; additional seats are AI-controlled.
