# DeckForge 0.2.6.55

- **Tag:** `v0.2.6.55`
- **Type:** Small UI patch — centered DeckForge wordmark and Home navigation
- **Version change:** `0.2.6.5` → `0.2.6.55` (literal requested target)
- **Previous release:** `v0.2.6.5`; no existing tag is moved.

## Changes since v0.2.6.5

- Replaced the old left spade/version brand with a styled, text-only **DeckForge** wordmark centered in the top bar. `Deck` stays white; `Forge` uses a subtle green/blue gradient.
- The wordmark is a real Home button. It returns to the menu from Blackjack and Durak shortcuts, lobbies, and active rounds; unfinished wagers still follow the existing refund path.
- Removed the redundant Home tab. Blackjack and Durak shortcuts remain on the left, with the version badge; bankroll and utility controls remain on the right. At 320px the version badge uses a compact size and stays visible.

## Launch

- Open `deckforge.html` directly; the app remains a self-contained offline file.
- Live site: <https://c000nstantin420-rgb.github.io/DeckForge/>
- Currency is fictional and stored locally.

## Verification

- In-app diagnostics: **83/83 passed**, 0 failed, 0 runtime errors.
- Brand navigation by rendered coordinate clicks returned Home from Blackjack and Durak lobbies and active Blackjack/Durak rounds; active physical-card audits passed.
- Wordmark center is within `0.01px` of the content center at `320`, `360`, `375`, `390`, `414`, `834`, `1024`, and `1194px`. No document-width overflow or overlap; the version badge remains visible at every tested width.
- Vision reviewed phone Home/live Blackjack (`390×844`), iPad portrait Home/live Blackjack (`834×1194`), and iPad landscape live Blackjack (`1194×834`). Header groups and wordmark remained legible and unclipped.
- Static audit: **297,225 bytes**, 2,262 lines, one inline script, 80 literal DOM references, no missing/duplicate IDs, external assets, inline event attributes, or URLs. `node --check`, four proof JSONs/nine PNG signatures and dimensions, and `git diff --check` passed.
- Proofs: `proofs/center-brand-0.2.6.55.json`, `proofs/version-badge-0.2.6.55.json`, `proofs/self-test-0.2.6.55.json`, plus the versioned Home/game screenshots in `proofs/`.

## Known limits

Physical iPhone/iPad Safari was not tested; geometry and Vision checks used Chromium device emulation. Real touch hardware was not available; navigation was tested through rendered browser input. The app has no network dependency at runtime.
