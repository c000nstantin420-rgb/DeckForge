# DeckForge 0.0.0.1

> **Superseded:** This release used the wrong version string. The correct follow-up is `v0.2.6.3`; use that release instead.

- **Tag:** `v0.0.0.1`
- **Type:** Mobile lobby progressive disclosure
- **Version label:** Superseded; this publication used the wrong version string. The correct follow-up is `v0.2.6.3`.

## Changes since v0.2.6.2

- Collapsed detailed Blackjack and Durak table settings by default on mobile (`≤760px`). The lobby keeps a compact summary of the selected rules and a clear `Customize` control.
- `Customize` reveals all seat and rule controls. The panel stays open while settings change, can be collapsed with `Hide`, and resets to collapsed when leaving Home and entering a new mobile lobby.
- Desktop lobbies remain expanded by default; resizing across the mobile breakpoint updates the default while the user has not chosen an explicit state.
- Wager selection and the start action remain outside the advanced panel, so collapsing settings does not hide the main flow.
- No game rules, wager math, deck identity, or release-0.2.6.2 mobile game layouts changed.

## Launch

Open `deckforge.html` directly in a browser. No install, server, external asset, or network connection is required.

## Verification

- In-app diagnostics: **76/76**, zero failures.
- At `390×844`, fresh Blackjack and Durak lobbies start collapsed, show the active settings summary, and keep wager/start controls above the fold. At `375×667`, the summary and toggle fit without horizontal overflow; the lobby scrolls vertically.
- Expanded controls passed for Blackjack and Durak. Changing seats/rules preserved the expanded state and updated the effective summary; returning Home and re-entering restored the collapsed mobile default.
- Desktop `1024×768` setup is expanded by default. Resizing across the `760px` breakpoint updates the default when no explicit override exists.
- Fresh mobile Blackjack flow: 3 seats → Deal → `Stand` → settled payout; physical-card audit passed, no browser errors. Fresh 4-seat Durak flow: `all players` throw-in → legal `Take cards` action; audit passed.
- Disclosure state matrix passed: mobile default, expanded setting rerender, Hide, Home/re-entry reset, and resize to desktop/mobile defaults with no explicit override. A synthetic touch-pointer sequence hit-tested the actual Customize control and opened it.
- Vision reviewed collapsed/expanded Blackjack and Durak at `390×844`, Blackjack at `375×667`, and expanded desktop Blackjack at `1024×768`.
- Static gates: `node --check`, 76/76 in-app diagnostics, 168/168 Durak stress matches over 14 variants, static audit (`266305` bytes, `2009` lines, 51 unique IDs, no external assets/URLs or DOM-reference errors), JSON validation, and `git diff --check`: **PASS**.
- Evidence: `proofs/browser-qa-0.0.0.1.json`, `proofs/setup-disclosure-0.0.0.1.json`, `proofs/blackjack-ui-flow-0.0.0.1.json`, `proofs/durak-ui-flow-0.0.0.1.json`, `proofs/touch-qa-0.0.0.1.json`, and `proofs/deckforge-0.0.0.1-*.png`.

## Known limits

- On desktop at `1024×768`, the expanded lobby has no horizontal overflow but lower wager/Deal controls fall below the fold; page height is `1143px`, so vertical scrolling is needed.
- Physical iPhone/iPad Safari and hardware audio were not tested. Browser layout used Chromium device emulation; the touch proof used synthetic pointer input. Currency is fictional and local-only.
