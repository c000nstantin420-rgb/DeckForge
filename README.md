# DeckForge

**DeckForge 0.2.8.0** is a local-first card room for Blackjack, Durak, Mau-Mau, and an optional UNO deck. The game runs as one self-contained HTML file; no install, account, server, fonts, images, or network connection are needed to play from a downloaded copy.

## Play

- **Project page:** <https://c000nstantin420-rgb.github.io/DeckForge/>
- **Play directly:** <https://c000nstantin420-rgb.github.io/DeckForge/deckforge.html>
- **Standalone game:** open [`deckforge.html`](deckforge.html) in a browser.
- **Download:** [Standalone HTML](https://github.com/c000nstantin420-rgb/DeckForge/releases/download/v0.2.8.0/DeckForge-0.2.8.0.html) · [ZIP archive](https://github.com/c000nstantin420-rgb/DeckForge/releases/download/v0.2.8.0/DeckForge-0.2.8.0.zip) · [Release notes](https://github.com/c000nstantin420-rgb/DeckForge/releases/tag/v0.2.8.0)
- **Release notes:** [`RELEASE_NOTES_0.2.8.0.md`](RELEASE_NOTES_0.2.8.0.md)

## Games

- **Blackjack:** one-to-four seats, a six-deck physical shoe, hit/stand/double/split/insurance, configurable natural payout and dealer soft-17 rule.
- **Durak:** two-to-four-seat Podkidnoy, 24/36/52-card decks, AI opponents, throw-in scope, and independently switchable **Throw-in** and **Transfer** rules. Existing saved rule choices migrate to the new settings.
- **Mau-Mau:** two-to-four seats, configurable decks, scoring, calls, effects, and Idioten-Mau. Its suit wish clears when a matching non-Jack card resolves it; a new Jack sets a new wish.
- **UNO:** an optional, separately configured deck mode inside Mau-Mau. Classic UNO uses 108 physical cards; four optional custom Wilds make 112. House-rule controls remain separate from Mau-Mau settings. UNO rule sources and defaults are documented in [`references/uno-variant-rules.md`](references/uno-variant-rules.md); Mau-Mau variants are documented in [`references/mau-mau-rules.md`](references/mau-mau-rules.md).

## Home game picker

Select Blackjack, Durak, or Mau-Mau + UNO from the horizontal Home rail. Scroll-snap and keyboard selection update the chosen table; **Spin** makes a decelerating random pick. Spin only selects a lobby—it never starts a round or places a wager. Open the chosen table separately.

## Local-first economy

The bankroll, wins, table settings, and transaction history stay in browser `localStorage`. All currency is fictional; there are no deposits or real-money wagers. Reloading returns to Home and restores saved settings, but does not resume an unfinished hand. Leaving an unfinished round returns its stake.

## Verification

- In-app suite: **144/144 passed** at `320×700`, `320×800`, `390×844`, `834×1194`, `1194×834`, `1024×768`, and `1280×720`; zero browser errors.
- Durak stress matrix: **336 seeded matches**, 42 deck/seat/rule configurations, 15,019 audited actions; zero failures. Every action preserved physical-card accounting.
- Inline JavaScript syntax and release-archive integrity are checked; project-page browser/layout/link QA and the tag-based Pages workflow are documented in `proofs/` and `.github/workflows/pages.yml`.
- Chromium emulation was used for visual and responsive checks. Physical iPhone/iPad Safari was not tested.

The public landing page is separate from the playable `deckforge.html`; the game itself remains a single offline HTML file. Currency and progress are fictional and local-only.
