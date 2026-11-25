# DeckForge 0.2.6.6

- **Tag:** `v0.2.6.6` (exact requested version)
- **Type:** Durak deck and optional-rule update
- **Previous published release:** `v0.2.6.56`; its tag and asset are unchanged.

## Changes

- Added Durak deck choices: **24 cards (9–Ace)**, **36 cards (6–Ace)**, or **52 cards (2–Ace)**. Every rank has four suits; the existing table still supports 2–4 seats.
- A 24-card/four-seat deal uses all cards. With no talon left, the last dealt card sets trump.
- Replaced the independent extra-rule controls with one exclusive selector: **Throw-in**, **Transfer**, or **Off** (both disabled). Throw-in keeps **Neighbors**/**All players** scope; All players requires four seats.
- Transfer is legal before any attack card is covered, uses a matching rank, and passes the entire attack to the next active defender. Transfers can chain; the next defender must hold enough cards for the enlarged table, subject to the six-card/table hand limit.
- Updated the live table summary, lobby feature cards, help, rules copy, and Transfer action label to match the selected configuration.

## Launch

- Open `deckforge.html` directly; the app remains a single offline HTML file.
- Live site: <https://c000nstantin420-rgb.github.io/DeckForge/>
- Currency is fictional and local-only.

## Verification

- In-app diagnostics: **89/89 passed**, 0 failed, 0 browser errors.
- Seeded stress: **360 matches across 30 configurations**, **15,398 audited actions**, 0 failures; each match terminated within a 1,500-step limit (observed 22–101 steps).
- Real rendered flows verified: persisted deck/rule choices, 24-card four-seat deal with empty talon and trump in hand, 52-card four-seat deal with 28 cards left, and a clicked Transfer action with physical-card audit before/after.
- Vision reviewed phone `320×800` and `390×844`, iPad portrait `834×1194`, and iPad landscape `1194×834`. The compact Durak lobby's tier-title/lock collision and cramped Transfer/`DEFEND` copy were fixed; the 24-card zero-talon/trump display, 36-card Off deal, 52-card Transfer action, and four-seat throw-in selectors were checked. Chromium emulation only; physical Apple Safari was not tested.
- Static audit: **307,131 bytes**, 2,289 lines, one inline script, 87 literal DOM references, no missing/duplicate IDs, external assets, inline event attributes, or URLs. `node --check`, ten proof JSONs/ten PNG signatures and dimensions, and `git diff --check` passed.
- Rule references checked: <https://www.durak.app/en/rules/> and <https://durak-live.com/en/rules/durak-card-decks-24-36-52>.

## Known limits

The app continues to support 2–4 seats; the 52-card option does not add 5–6-seat tables. Physical iPhone/iPad Safari was not tested.
