# DeckForge 0.2.6.3

- **Tag:** `v0.2.6.3`
- **Type:** Patch release — mobile table-setup progressive disclosure
- **Version change:** `0.2.6.2` → `0.2.6.3` (incremented the final component by one)
- **Supersession:** The earlier `v0.0.0.1` publication used a mistaken version string and is marked superseded. This is the correct continuation of `v0.2.6.2`.

## Changes since v0.2.6.2

- On mobile widths (`≤760px`), Blackjack and Durak table settings start collapsed behind a clear **Customize** control; the current rules stay visible in a compact summary.
- **Customize** reveals all seat and rule controls. The panel stays open while selections change, can be hidden on demand, and resets to the mobile default when returning Home and reopening a lobby.
- With no explicit override, changing viewport across the 760px breakpoint switches the default: collapsed on mobile, expanded on desktop.
- Wager selection and Deal/Start remain outside the advanced-settings panel. No game rules, wager math, physical card identity, or game-table layout changed.

## Launch

Open `deckforge.html` directly in a browser. No install, server, external assets, or network connection is required.

## Verification

- In-app diagnostics: **76/76**; zero failed assertions.
- Durak stress: **168/168 seeded matches**, across 14 valid 2–4-seat throw-in/push variants; every move preserved the physical-card audit and each match reached a legal terminal result.
- Fresh mobile Blackjack: configure 3 seats → deal → `Stand` → settlement. Fresh 4-seat Durak with all-player throw-in: legal attack action. Both game audits passed; zero browser errors.
- Disclosure interaction, setting rerender, Hide, Home/re-entry reset, and mobile/desktop resize defaults passed. Synthetic touch-pointer input hit-tested and opened the actual Customize control.
- Vision reviewed collapsed/expanded Blackjack and Durak at `390×844`, Blackjack at `375×667`, and expanded desktop Blackjack at `1024×768`.
- `node --check`, static audit (`266305` bytes, `2009` lines, 51 unique IDs, no external assets/URLs, duplicate IDs, or missing DOM references), JSON validation, and `git diff --check`: **PASS**.
- Proofs are included in `proofs/` and the versioned release archive.

## Known limits

- At `1024×768`, the expanded desktop lobby has no horizontal overflow, but the lower setup/wager/Deal controls extend below the fold and require vertical scrolling.
- Physical iPhone/iPad Safari and hardware audio were not tested. Layout checks used Chromium emulation; touch verification used synthetic pointer input. Currency is fictional and local-only.
