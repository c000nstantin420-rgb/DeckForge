# DeckForge 0.2.8.1

- **Tag:** `v0.2.8.1`
- **Type:** Infinite Home wheel and gesture-coast update
- **Previous release:** `v0.2.8.0`

## Changes since 0.2.8.0

- **Infinite Home wheel:** The three logical games repeat through a 21-cycle visual reel (63 slots). It recenters to equivalent copies before reaching the ends while keeping exactly three accessible game tabs.
- **Gesture coast:** Horizontal touch/trackpad/wheel movement and desktop drag can start a coast in the gesture's direction. The reel slows over randomized laps and duration, then randomly snaps to one centered game. The Spin button uses the same coast path.
- **Separate Open action:** Selection and spinning only choose a table. Opening it shows the lobby; it never starts a round or places a wager. Keyboard and direct tile selection remain available.
- **Accessibility and motion:** Selected tabs/status remain announced, conflicting controls are disabled while coasting, reduced-motion preference skips animation but still chooses a valid game, and cancellation/navigation restore the controls.
- **Project page and docs:** Updated the current version, release links, and picker description for the infinite wheel; section anchors now clear the sticky header.

## Launch

- Open [`deckforge.html`](deckforge.html) directly for the standalone game.
- Standalone release download: [DeckForge-0.2.8.1.html](https://github.com/c000nstantin420-rgb/DeckForge/releases/download/v0.2.8.1/DeckForge-0.2.8.1.html).
- ZIP release archive: [DeckForge-0.2.8.1.zip](https://github.com/c000nstantin420-rgb/DeckForge/releases/download/v0.2.8.1/DeckForge-0.2.8.1.zip).
- Hosted project page: <https://c000nstantin420-rgb.github.io/DeckForge/>
- Hosted game: <https://c000nstantin420-rgb.github.io/DeckForge/deckforge.html>
- Currency is fictional and local-only; there are no deposits or real-money wagers.

## Verification

- In-app suite: **148/148 passed** at each of `320×700`, `320×800`, `390×844`, `834×1194`, `1194×834`, `1024×768`, and `1280×720`; **0 browser errors**.
- Picker QA: 63 visual slots and 3 accessible tabs; exact 0 px visual shift at both safe-edge recenter thresholds; no page-level horizontal overflow; Spin/Open controls remain inside the viewport.
- Browser interaction checks cover Spin-button coast, both coast directions, pointer drag, touch-pointer and horizontal-wheel integration, vertical wheel rejection, keyboard/tap selection, reduced motion, cancellation/navigation, and Open-to-lobby without a round or wager. A real Blackjack deal, legal action, and settlement were exercised with the version badge visible at `320×700`.
- `node --check`, proof JSON/PNG validation, ZIP validation, and `git diff --check` pass. Detailed evidence is in `proofs/self-test-0.2.8.1.json`, `proofs/table-picker-infinite-spin-0.2.8.1.json`, and `proofs/static-audit-0.2.8.1.json`.
- Chromium emulation only. Touch and wheel integration used constructed browser events and emulated scroll movement; physical iPhone/iPad Safari and trackpad hardware were not tested.

## Evidence

- `proofs/self-test-0.2.8.1.json`
- `proofs/table-picker-infinite-spin-0.2.8.1.json`
- `proofs/static-audit-0.2.8.1.json`
- `proofs/table-picker-infinite-spin-0.2.8.1-home-390x844.png`
- `proofs/table-picker-infinite-spin-0.2.8.1-home-1194x834.png`
- `proofs/table-picker-infinite-spin-0.2.8.1-spin-button-mid-390x844.png`
- `proofs/table-picker-infinite-spin-0.2.8.1-gesture-mid-390x844.png`
- `proofs/table-picker-infinite-spin-0.2.8.1-spin-result-390x844.png`
- `proofs/project-page-0.2.8.1.json` and `proofs/project-page-0.2.8.1-390x844.png`
- `proofs/project-page-wheel-0.2.8.1-390x844.png`
- `proofs/blackjack-settlement-0.2.8.1-320x700.png`
- `proofs/blackjack-settlement-0.2.8.1-390x844.png`
