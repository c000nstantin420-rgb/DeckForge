# DeckForge 0.2.8.0

- **Tag:** `v0.2.8.0`
- **Type:** UNO, table-picker, Durak rules, responsive card sizing, and project-page feature release
- **Previous release:** `v0.2.6.7`

## Changes since 0.2.6.7

- **UNO deck mode:** Added classic four-color UNO as a separate deck choice within Mau-Mau setup. The classic shoe has 108 physical cards; four optional, clearly labeled custom Wilds make 112. Includes the seven-card deal, Skip/Reverse/Draw Two/Wild/Wild Draw Four, UNO calls and penalties, Wild Draw Four challenge, point or round-win scoring, and opt-in house rules such as stacking, Jump-In, Seven-Zero, draw-until-playable, and multi-card play. UNO settings do not overwrite Mau-Mau settings. Rule sources and defaults are documented in [`references/uno-variant-rules.md`](references/uno-variant-rules.md).
- **Mau-Mau suit wishes:** Playing a matching non-Jack card satisfies and clears the previous wish; a newly played Jack sets the next wish. Added engine and rendered-flow regression checks.
- **Home tile:** Updated the Mau-Mau launch tile to show both Mau-Mau and UNO card art and explain the optional UNO deck.
- **Durak rules:** Split Throw-in and Transfer into independent On/Off options. All four combinations are supported, legacy exclusive saves migrate, and Throw-in scope is shown only when Throw-in is enabled; All players still requires four seats.
- **Card sizing:** Enlarged cards where the table allows and tuned compact layouts. A four-seat, ten-card UNO hand remains reachable by scrolling on phones and fits at supported wider layouts; card faces and action labels remain legible.
- **Home game picker:** Added a horizontal, scroll-snapped and keyboard-selectable rail for Blackjack, Durak, and Mau-Mau + UNO. Spin makes a decelerating random table pick; the user opens the selected lobby separately. Spin never places a wager or starts a round.
- **Project page:** Replaced the redirect-only Pages root with a responsive DeckForge landing page. The playable app remains at `deckforge.html` and remains a single offline HTML file.

## Launch

- Open [`deckforge.html`](deckforge.html) directly for the standalone game.
- Standalone release download: [DeckForge-0.2.8.0.html](https://github.com/c000nstantin420-rgb/DeckForge/releases/download/v0.2.8.0/DeckForge-0.2.8.0.html).
- ZIP release archive: [DeckForge-0.2.8.0.zip](https://github.com/c000nstantin420-rgb/DeckForge/releases/download/v0.2.8.0/DeckForge-0.2.8.0.zip).
- Hosted project page: <https://c000nstantin420-rgb.github.io/DeckForge/>
- Hosted game: <https://c000nstantin420-rgb.github.io/DeckForge/deckforge.html>
- Currency is fictional and local-only; there are no deposits or real-money wagers.

## Verification

- In-app diagnostics: **144/144 passed** at each of `320×700`, `320×800`, `390×844`, `834×1194`, `1194×834`, `1024×768`, and `1280×720`; **0 browser errors**.
- Durak engine stress: **336 seeded matches** across **42** deck/seat/Throw-in/Transfer/scope configurations; **15,019 audited actions**, **41 draws**, **0 failures**. Every action was followed by a physical-card audit; matches completed in 22–99 steps.
- Verified independent-rule UI/persistence, all four rule combinations, selection/Spin/Open behavior, no-wager semantics, reduced-motion selection, cancellation on navigation, UNO setup/engine paths, and the version badge.
- `node --check` and `git diff --check` pass. Project-page browser/layout, link, and visual checks are recorded in `proofs/project-page-0.2.8.0.json`.
- Chromium emulation only. Physical iPhone/iPad Safari was not tested. A desktop CUA horizontal-scroll probe was delivered but did not visibly move the carousel, so it is not counted as a touch-swipe proof.

## Evidence

- `proofs/self-test-0.2.8.0.json`
- `proofs/durak-stress-0.2.8.0.json`
- `proofs/static-audit-0.2.8.0.json`
- `proofs/project-page-0.2.8.0.json`
- `proofs/durak-independent-rule-toggles-0.2.8.0.json`
- `proofs/table-picker-carousel-0.2.8.0.json`
- `proofs/card-scaling-0.2.8.0.json`
- `proofs/uno-flow-0.2.8.0.json`
- The self-test matrix also exercises Mau-Mau suit-wish resolution and the Mau-Mau + UNO Home tile.
