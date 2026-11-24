# DeckForge 0.2.2

## Highlights

- Polished mobile Blackjack table: larger readable cards, clear dealer/player zones, compact metadata, and contextual actions such as `Double +$10`.
- Polished mobile Durak table: explicit turn/role badges, compact trump/talon panel, real empty-table drop target, readable six-card hand fan, and contextual attack/defense actions.
- Durak keeps a real desktop side column for trump, rules, and table log; the active action dock remains visible at 1024×768.
- Four-seat Durak AI can throw in from eligible side seats according to `Neighbors` or `All players` policy.
- GitHub Pages deployment now runs on `main` pushes and on every published GitHub release, checking out the release tag for the published site.
- Runtime artifact remains `deckforge.html`; no external assets or network dependency were added to the app.

## Verification

- 39/39 in-app assertions passed.
- `node --check` passed.
- Mobile QA: 390×844, six Durak cards fully visible, no horizontal/vertical page overflow, zero browser errors.
- Desktop QA: 1024×768, Durak side column and action dock visible, no overflow, zero browser errors.
- Tap attack flow verified: selected card → contextual `Attack <card>` → defense phase.
- Side-AI throw-in verified in a deterministic four-seat `All players` fixture.
