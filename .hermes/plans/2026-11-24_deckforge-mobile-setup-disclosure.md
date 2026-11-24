# Mobile setup progressive disclosure

## Goal

Make the mobile lobby feel simple at first glance while preserving the full Blackjack/Durak table configuration behind an obvious, accessible control. Desktop setup remains expanded by default.

## Acceptance

- At widths `375` and `390` CSS px, a fresh Blackjack or Durak lobby starts with `aria-expanded="false"`; detailed table controls are hidden, while the selected configuration summary, wager and start controls remain in the normal lobby flow. At `390×844` the start button is above the fold; `375×667` may scroll vertically but has no horizontal overflow.
- Tapping **Customize** reveals all seat/rule controls; the toggle updates its label/icon and `aria-expanded` state.
- Changing any setting rerenders the lobby without collapsing an explicitly opened panel; tapping **Hide** collapses it.
- Leaving to Home and reopening a game returns to the mobile default (collapsed).
- At `1024×768`, detailed settings start expanded. The user can still collapse them.
- No horizontal overflow or browser errors at the tested viewports; configuration choices still change the actual game config.

## Implementation / test order

1. Add a deterministic test for the mobile/desktop default disclosure breakpoint.
2. Add an ephemeral `appState` override, reset it on fresh game entry, and keep it across configuration rerenders.
3. Render a compact effective-config summary plus an accessible Customize/Hide button; put all detailed settings inside a hidden-able panel.
4. Add mobile styling for the summary/toggle, keeping wager and Deal/Start outside the advanced panel.
5. Verify fresh mobile/desktop lobbies, expand/change/collapse/reopen flows, diagnostics, overflow, and Vision screenshots.

## Release authorization

The user clarified that `0.0.0.1` meant increment the current `0.2.6.2` version by one in its final component: publish `0.2.6.3` with tag `v0.2.6.3`. The earlier `v0.0.0.1` publication was a misunderstanding; preserve it for history but mark it superseded. Do not move or reuse its tag.

## Completed verification

- **Mobile default:** Fresh `390×844` Blackjack and Durak lobbies start collapsed (`aria-expanded=false`) with a compact current-rules summary and a visible `Customize +` button. Wager/start controls are visible above the fold at 390px. At `375×667`, the summary remains unclipped and the lobby scrolls vertically without horizontal overflow.
- **Depth on demand:** `Customize` reveals every game-specific setting. Changing Blackjack to 3 seats or Durak to 4 seats / all-player throw-in rerenders without collapsing the panel. The config reached the actual started game (`Blackjack players=3`, `Durak players=4, throwIn=all`) and both deck audits passed.
- **Reset behavior:** Returning Home and reopening a game resets to the mobile collapsed default. Crossing the 760px breakpoint while no explicit override exists expands on desktop and collapses on mobile. A fresh `1024×768` desktop lobby starts expanded.
- **Diagnostics:** `76/76` in-app assertions; Durak stress `168/168` seeded matches over 14 variants; zero failures and browser errors.
- **Browser flows:** Blackjack 3 seats → Deal → Stand → settled payout; 4-seat Durak/all-player → legal Take cards action. Both audits remain valid. Separate proof JSONs record the flows and the 8-step disclosure/reset/breakpoint matrix.
- **Touch integration:** A touch-pointer sequence hit-tested the actual `Customize` button and opened the panel; synthetic Chromium input only, not physical Safari.
- **Vision:** Fresh 390px Blackjack collapsed/expanded, Durak collapsed/expanded, and 375px Blackjack collapsed screens passed. On 1024×768, the desktop panel starts expanded with no horizontal overflow; the lower wager/start controls require page scroll (document height 1143px).
- **Static checks:** `node --check`, static audit (`266305` bytes, `2009` lines, 51 unique IDs, no duplicate/missing references or external assets), JSON validation, and `git diff --check` pass.
- **Release:** Correct version increment `0.2.6.2` → `0.2.6.3`; `VERSION`/`APP_VERSION` `0.2.6.3`, tag `v0.2.6.3`, notes `RELEASE_NOTES_0.2.6.3.md`, archive `releases/DeckForge-0.2.6.3.zip`. Preserve and mark the mistaken `v0.0.0.1` release superseded; verify the corrected release and Pages read-back.
