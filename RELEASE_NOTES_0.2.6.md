# DeckForge 0.2.6

**Tag:** `v0.2.6`

**Type:** Durak landscape, throw-in planning, and next-round wager controls

## Changes since 0.2.5

- Kept all Durak actions inside the visual viewport on compact landscape screens, including a four-seat table with six attack pairs. The board scrolls internally where needed; the hand does not overlap the action dock.
- Added a longer Durak AI reaction window when the human can throw in. Choose a matching card and covered target pile while the AI acts, then queue it without changing the turn. The queued play executes only when the human turn arrives; invalid/stale plans are cleared. A 12-second timeout resumes the AI, and `Continue AI` remains available.
- Throw-ins now require an explicit matching target pile and are placed next to it. The pile relationship is shown in the table UI and the AI chooses a legal target automatically.
- Added an optional `Push onward` action whenever a legal push exists, even before selecting a defense card. Defend, push, and take remain separate choices.
- Added a next-round stake selector beside `Deal next hand` and `New duel`. A new wager is applied only to the next game; completed Durak results retain the original stake for settlement and display.
- Repeated complete Durak positions now resolve as a visible stalemate draw with the current duel stake returned, rather than cycling forever. The rules and result copy distinguish this from an all-empty draw.
- Updated the visible header badge to `v0.2.6` on home and active game screens.

## Launch

Open `deckforge.html` directly in a browser. No server, install, build step, external assets, or network connection is required.

## Verification

- In-app diagnostics: **69 passed / 0 failed**.
- Durak stress: **168 seeded matches** across 14 valid 2–4-seat throw-in/push configurations; every action passed the 36-card audit and every match reached a valid terminal state. A separate seeded endgame verified the repeated-position stalemate path.
- Rendered UI matches finished for 2, 3, and 4 seats. Browser flows exercised targeted throw-in during an AI turn, turn-safe queue execution, stale-queue invalidation, optional push, defense, take, Continue AI, and the reaction timeout.
- Touch-pointer drag to a selected throw-in pile queued the card without removing it out of turn; the subsequent human-turn action inserted it beside the selected pile.
- Six-pair/four-seat Durak geometry passed at **390×844, 1024×768, 1024×600, 1194×834, 1194×700, 1366×768, and 1366×600**: all action buttons were fully inside the visual viewport, the hand stayed above the dock, and the document did not overflow.
- Blackjack and Durak next-round selectors were exercised from finished games. Changed stakes debited correctly, and a forced Durak rerender kept the prior settlement tied to the original wager.
- AI scheduling used **1.4–2.2s** while planning a throw-in and returns to **320–760ms** after queueing. Browser callback measurements are retained in the QA proof; timer dispatch showed headless scheduling jitter.
- `node --check`: **PASS**. Static audit: one inline script, no external assets, duplicate IDs, missing DOM references, or inline event attributes.

## Known limits

- Physical iPad Safari and real audio hardware were not tested. Responsive checks used Chromium device emulation; touch-drag checks used synthetic `PointerEvent` events with `pointerType="touch"`.
- Currency is fictional and local-only. Online multiplayer is not included; additional seats are AI-controlled.
