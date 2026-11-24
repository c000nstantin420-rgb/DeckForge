# DeckForge 0.2.5 — Blackjack and Durak correctness, feel, and versioning

## Baseline

- Start from clean `v0.2.4` / commit `98d435f`.
- Previous live review produced reproducible findings: multi-seat Blackjack waits forever after resolving the Ace insurance window; Durak attack cards lose red-suit color, defense piles obscure rank/pips, the trump card is squeezed, the human hand stays in deal order, multi-seat matches stop/loop incorrectly, round state leaks, mobile actions clip, and drag targeting is too small.
- Research the multiplayer rule contract against Pagat's Podkidnoy Durak rules (`https://www.pagat.com/beating/podkidnoy_durak.html`) and keep the existing 36-card, 2–4 seat, neighbors/all throw-in and optional push configuration.

## Behavior contract

### Blackjack

- With 2–4 seats, a dealer Ace opens the insurance window. Choosing either Take insurance or No thanks must start the AI-seat scheduler when resolution changes the game to `phase='ai'`.
- Cover insurance-on/off, dealer-natural settlement, AI progress to the human turn, no repeated timers, and zero browser errors. Preserve current bankroll and payout rules.

### Durak

- Keep 36 physical cards and a valid shoe audit after every transition.
- Seats stay indexed. Use circular clockwise active-seat helpers; never assign the same seat as attacker and defender. Skip empty seats once the talon is exhausted.
- After a beaten-off bout, the old defender leads the next bout and the next active seat clockwise defends. After a take, the defender loses the lead; the next active seat clockwise attacks and the following active seat defends.
- At bout end, draw all active players toward six in a deterministic order: principal attacker first, then clockwise seats, defender last. Preserve the exposed trump as the last talon card.
- After the talon is empty, remove empty-handed players from turns and finish only at bout end when one player still holds cards (that player is the Durak); if nobody has cards, settle as a draw. Translate this to the existing human-vs-AI bankroll result without ending early while several seats still hold cards.
- Round-only thrower/push state resets at every bout. Push must have a legal next defender with enough cards to cover the table; keep the current one-push-per-bout limit and explain it in the rules.
- AI must always either change state or produce a visible paused/invariant state. Never reschedule an unchanged no-op forever.

### Presentation and feel

- Sort only a copy of the human display hand: fixed suit grouping, ascending rank within a suit, non-trump groups first, every trump last. Do not mutate engine hand order or card identity.
- Render red suits red in attacks and defenses; use consistent rank/suit corners and sized center pips for hand, table, and trump cards.
- Display attack/defense pairs so each card's rank and central pip stay legible; enlarge the defense drop target to cover the visible uncovered attack/pair.
- Render a compact, proportionate face-up trump plus a visible face-down talon stack and count.
- At 390×844, preserve the full action dock for an ordinary defense and a six-attack table; at 1024×768 verify multi-seat board/table visibility and no clipping.
- Keep the established 320–760ms AI pacing unless real interaction tests show a problem; record low/mid/high delay timings and verify immediate human feedback.
- Show the current `APP_VERSION` visibly in the top bar on both home and game screens, including the narrow viewport.

## TDD and verification

1. Add deterministic regressions for Blackjack insurance scheduling transitions and Durak role order after defense/take/push, empty-seat skipping, terminal results, draw order, round-flag reset, sorting, card markup and drag target geometry.
2. Demonstrate red failures before engine/UI fixes; preserve the existing 44 tests.
3. Run at least 12 seeded autonomous matches per supported Durak seat count (2, 3, 4). Audit all 36 physical cards after every step, use a hard step bound plus consecutive-no-progress guard, reject premature terminal states, and verify the single remaining Durak/draw result.
4. Fresh browser flows: Blackjack 3/4 seats → Ace insurance → each insurance choice → AI → human action/settlement; Durak 2/3/4 seats through defense, take, throw-in and next-bout transitions; verify one legal drag-to-defense and an invalid drop; capture errors and state.
5. Measure AI callbacks at low/mid/high delay values. Probe 390×844 and 1024×768, including max six attack pairs; assert every action button fully in viewport, all hand/table/trump rectangles visible, no unexpected scroll or clipping.
6. Verify top-bar version marker on home and live game. Run `node --check`, JSON/DOM/static audit, `git diff --check`, update measured proof and screenshots.
7. Build `releases/DeckForge-0.2.5.zip` from an explicit file list, verify every archive entry and exact source bytes, then commit, annotate `v0.2.5`, push, publish the release and verify Pages, release notes, public asset checksum and live `0.2.5` UI.

## Constraints

- Preserve the standalone offline single-file app; no external assets, build dependencies or online multiplayer.
- Physical iPhone/iPad Safari and real audio are not claimable unless actually tested.
- Keep source/code/UI text English. Do not package test bankroll or active game data; browser QA used isolated contexts.

## Verification results before publishing

- In-app diagnostics: `60 passed / 0 failed / 60 total` at `2026-11-24T09:31:11.004Z`.
- Static audit: `deckforge.html` 199,731 bytes / 1,734 lines; one inline script; `node --check` PASS; zero duplicate IDs, missing literal `getElementById` references, external resource references, inline event attributes or network URL literals.
- Durak engine stress: 100/100 matches terminated with per-step 36-card audits across 2-seat neighbors+push, 2-seat throw-in-off/no-push, 3-seat neighbors+push, 4-seat neighbors+push, and 4-seat all-players+push.
- Rendered UI matches completed for 2, 3, and 4 seats, plus a controlled human-loss settlement. Separate flows passed take, push, throw-in, valid touch-pointer defense drop, invalid covered-target drop, and the visible AI no-op guard.
- Blackjack insurance: both choices passed with 3 seats; a 4-seat dealer-Ace path advanced through three AI callbacks, returned the player turn, then settled.
- Delay measurements: 328ms low, 546/547ms midpoint, 775ms high end-to-end including browser/timer dispatch overhead.
- Viewport: six Durak pairs and the complete dock stayed in bounds at 390×844 and 1024×768 for 2–4 seats. Header badge `v0.2.5` was visible on home and live game screens.
- Proofs and screenshots: `proofs/self-test.json`, `proofs/browser-qa.json`, `proofs/static-audit.txt`, and five `proofs/deckforge-0.2.5-*.png` files.
- Hardware limits: physical iPhone/iPad Safari and real audio were not tested; CDP touch dispatch timed out, while the rendered synthetic `PointerEvent` touch path passed.

## Release verification

- Release version/tag: `0.2.5` / `v0.2.5`.
- GitHub Release: `https://github.com/c000nstantin420-rgb/DeckForge/releases/tag/v0.2.5`.
- Local archive: `releases/DeckForge-0.2.5.zip`; SHA-256 `71ccf5af6e494c73fbab0a93680221386db91c1697ac859f3faa3be61d44dd8e`; 13 explicit files, no directory entries, and packaged `deckforge.html` compares byte-for-byte with the runtime source.
- The release workflow checks out the tag and publishes Pages. Remote tag/asset/workflow/live-site read-back is required before reporting publication complete.
