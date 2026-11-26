# DeckForge

**DeckForge 0.2.8.1** is a local-first card room for Blackjack, Durak, Mau-Mau, and an optional UNO deck. The game runs as one self-contained HTML file; no install, account, server, fonts, images, or network connection are needed to play from a downloaded copy.

## Play

- **Project page:** <https://c000nstantin420-rgb.github.io/DeckForge/>
- **Play directly:** <https://c000nstantin420-rgb.github.io/DeckForge/deckforge.html>
- **Standalone game:** open [`deckforge.html`](deckforge.html) in a browser.
- **Download:** [Standalone HTML](https://github.com/c000nstantin420-rgb/DeckForge/releases/download/v0.2.8.1/DeckForge-0.2.8.1.html) · [ZIP archive](https://github.com/c000nstantin420-rgb/DeckForge/releases/download/v0.2.8.1/DeckForge-0.2.8.1.zip) · [Release notes](https://github.com/c000nstantin420-rgb/DeckForge/releases/tag/v0.2.8.1)
- **Release notes:** [`RELEASE_NOTES_0.2.8.1.md`](RELEASE_NOTES_0.2.8.1.md)

## Games

- **Blackjack:** one-to-four seats, a six-deck physical shoe, hit/stand/double/split/insurance, configurable natural payout and dealer soft-17 rule.
- **Durak:** two-to-four-seat Podkidnoy, 24/36/52-card decks, AI opponents, throw-in scope, and independently switchable **Throw-in** and **Transfer** rules. Existing saved rule choices migrate to the new settings.
- **Mau-Mau:** two-to-four seats, configurable decks, scoring, calls, effects, and Idioten-Mau. Its suit wish clears when a matching non-Jack card resolves it; a new Jack sets a new wish.
- **UNO:** an optional, separately configured deck mode inside Mau-Mau. Classic UNO uses 108 physical cards; four optional custom Wilds make 112. House-rule controls remain separate from Mau-Mau settings. UNO rule sources and defaults are documented in [`references/uno-variant-rules.md`](references/uno-variant-rules.md); Mau-Mau variants are documented in [`references/mau-mau-rules.md`](references/mau-mau-rules.md).

## Home game picker

The Home rail loops through Blackjack, Durak, and Mau-Mau + UNO. Swipe, wheel, or drag horizontally to coast in that direction; the reel slows and randomly snaps to a game. The **Spin** button uses the same reel, while keyboard and direct tile selection remain available. A spin only chooses a table—it never starts a round or places a wager. Open the selected lobby separately.

## Local-first economy

The bankroll, wins, table settings, and transaction history stay in browser `localStorage`. All currency is fictional; there are no deposits or real-money wagers. Reloading returns to Home and restores saved settings, but does not resume an unfinished hand. Leaving an unfinished round returns its stake.

## Verification

- In-app suite: **148/148 passed** at `320×700`, `320×800`, `390×844`, `834×1194`, `1194×834`, `1024×768`, and `1280×720`; zero browser errors.
- Infinite Home reel: 63 visual slots, exactly three accessible tabs, and 0 px visible-card shift at both recenter thresholds. Button, pointer drag, touch-pointer, horizontal wheel, reverse coast, keyboard, reduced motion, cancellation/navigation, no-wager, and separate Open flows were checked.
- Durak stress matrix: **336 seeded matches**, 42 deck/seat/rule configurations, 15,019 audited actions; zero failures. Every action preserved physical-card accounting.
- Inline JavaScript syntax and release-archive integrity are checked; project-page browser/layout/link QA and the tag-based Pages workflow are documented in `proofs/` and `.github/workflows/pages.yml`.
- Chromium emulation was used for visual and responsive checks. Physical iPhone/iPad Safari was not tested.

The public landing page is separate from the playable `deckforge.html`; the game itself remains a single offline HTML file. Currency and progress are fictional and local-only.
