# CardForge

CardForge is an offline single-file card room for iPhone, iPad, and desktop browsers.

## Games

- **Blackjack** — a real six-deck shoe with physical card identities, hit, stand, double, split, insurance, natural Blackjack 3:2, dealer soft-17 rules, discard tracking, and shoe reshuffles.
- **Durak** — a complete two-player Podkidnoy match with a real 36-card deck, trump, attacks, throw-ins, defenses, takes, draw order, and an AI opponent.

## Bankroll and progression

The fictional bankroll persists in `localStorage`. Each game has four stake tables. Wins unlock higher tables. A new wager made at a zero balance grants exactly `$100` before the wager is deducted.

## Run

Open `index.html` directly in a browser. No server, install, build, font, image, or network connection is required.

## Verification

- Engine diagnostics: **26 passed / 0 failed**.
- Static audit: one inline script, no external assets, no duplicate IDs, no missing DOM references, and `node --check` passes.
- Browser QA covers Blackjack settlement, complete Durak matches, player win/loss/draw outcomes, persistence, the bankruptcy grant, touch-pointer navigation, and 390×844 / 1024×768 layout geometry.
- Detailed results and screenshots are in `proofs/`.
- `plan.md` contains the implementation plan and measured acceptance evidence.

Currency is fictional and local-only. Physical iPhone/iPad Safari and real audio hardware still need a device pass; Chromium mobile emulation and touch-pointer dispatch are covered.
