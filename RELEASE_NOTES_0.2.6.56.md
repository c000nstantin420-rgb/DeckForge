# DeckForge 0.2.6.56

- **Tag:** `v0.2.6.56`
- **Type:** Corrective UI patch — remove mode selection from the top bar
- **Version change:** `0.2.6.55` → `0.2.6.56` (fix-forward; `.55` is already published)
- **Previous release:** `v0.2.6.55`; its published tag and asset are unchanged.

## Changes since v0.2.6.55

- Removed the Blackjack and Durak mode shortcuts from the top bar. It now contains only the version badge at left, the centered DeckForge wordmark/Home button, and bankroll/utility controls at right.
- Kept game selection on the Home screen. The Blackjack and Durak game cards still open their existing lobbies.
- The centered wordmark continues to return Home from either lobby or an active Blackjack/Durak round.

## Launch

- Open `deckforge.html` directly; DeckForge remains an offline single-file app.
- Live site: <https://c000nstantin420-rgb.github.io/DeckForge/>
- Currency is fictional and local-only.

## Verification

- In-app diagnostics: **83/83 passed**, 0 failed, 0 browser errors.
- Both Home game cards still open their games. Rendered browser input verified the centered brand returns Home from Blackjack and Durak lobbies and active rounds; both game audits passed.
- The top bar has zero mode-selector buttons. The wordmark center is within `0.01px` at widths `320`, `360`, `375`, `390`, `414`, `834`, `1024`, and `1194px`; no horizontal overflow or header collisions. The version badge remains visible.
- Vision reviewed phone Home at `320×800` and `390×844`, mobile active Blackjack, iPad portrait Home/active Blackjack, and iPad landscape Home. No mode controls remain in the top bar; the wordmark and remaining controls fit cleanly.
- Static audit: **295,759 bytes**, 2,253 lines, one inline script, 78 literal DOM references, no missing/duplicate IDs, external assets, inline event attributes, or URLs. `node --check`, three proof JSONs/ten PNG signatures and dimensions, and `git diff --check` passed.
- Evidence: `proofs/topbar-mode-removal-0.2.6.56.json`, `proofs/self-test-0.2.6.56.json`, `proofs/static-audit-0.2.6.56.json`, and versioned screenshots in `proofs/`.

## Known limits

Physical iPhone/iPad Safari was not tested. Layout and Vision review used Chromium device emulation; navigation used rendered browser input, not physical touch hardware.
