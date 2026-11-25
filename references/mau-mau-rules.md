# Mau-Mau rules research and implementation choices

Mau-Mau has no universal governing rulebook. The shared core is to shed cards by matching suit or rank, draw when unable or choosing not to play, and win by emptying the hand. Special-card meanings, scoring, call penalties, and final-card effects vary by table. DeckForge treats those as explicit settings rather than presenting one family's rules as universal.

## Sources checked in a real browser

- [Mau-Mau Palast — Lektion 1: Was spiele ich zuerst?](https://www.mau-mau-palast.de/lektion-1-was-spiele-ich-zuerst/) — 32-card French-suit pack, five cards per player, suit/rank matching, the common Seven/ Eight/Jack effects, and German card-point values.
- [Mau-Mau Palast — Lektion 2: Behalte die Karten im Auge](https://www.mau-mau-palast.de/lektion-2-behalte-die-karten-im-auge/) — Seven stacking, Eight skipping, and the Palast base rule that a Jack cannot be played on another Jack.
- [Mau-Mau Palast — Lektion 3: Regelvielfalt beim Mau-Mau](https://www.mau-mau-palast.de/lektion-3-regelvielfalt-beim-mau-mau/) — explicit statement that household rules differ; common 32-card/2–4-player/five-card baseline; 64 cards from two packs; six-card deal; no-play-after-draw; Jack-on-Jack; Jack-finish double points; Ace extra play; Ten clears Jack wish; Queen of Clubs challenge; Nine reverses and skips at two seats; and Idioten-Mau.
- [Mau-Mau Palast — Lektion 4: Die besten Karten zum Beenden](https://www.mau-mau-palast.de/lektion-4-die-besten-karten-zum-beenden/) — call “Mau” before playing the penultimate card, two-card missed-call penalty in that Palast variant, and the risk of a final Seven being stacked back to its player.
- [HowTo Cards — Mau-Mau](https://howto.cards/en/mau-mau/) — one-card draw and optional immediate play, 32/52/64-card packs, popular 6-card deal, optional Ace replay, Nine reverse, 2 draw-two, Jack-finish restriction, one- or two-card call penalties, and a traditional scoring style (J=2, Q=3, K=4, A=11) versus Jack=20.
- [GameRules — Mau Mau](https://gamerules.com/rules/mau-mau/) — corroborates the 32-card/2–4-player/five-card game, 7/8/Jack effects, a winner-points race (first to 150), and the high-face-card scoring variant (Q/K/A=10, Jack=20).
- [Spielstrategen — Mau Mau Regeln](https://spielstrategen.de/spiele/mau-mau/regeln/) — explains why no single official rules authority exists and notes regional 32/36/52-card and 5–7-card practices.

Pagat's [Eights index](https://www.pagat.com/eights/) is useful for the wider shedding-game family, but its Crazy Eights rules are not treated as authority for German Mau-Mau card effects.

## DeckForge defaults

- 32 cards (7–Ace), two seats (user plus one AI), five cards each.
- Match by suit or rank; draw one if unable or choosing not to play; a playable drawn card may be played immediately.
- Seven draws two and may be answered by another Seven; Eight skips; Jack wishes a suit and cannot be played on Jack.
- “Mau” is required when playing the penultimate card; “Mau-Mau” on the last. Missed-call penalty defaults to one card, with 0/1/2 available because the checked sources disagree (one vs. two).
- Default scoring is one round win; first to one round takes the match. Optional score modes are a penalty-point cap (lowest total at the cap wins) and a points race (the round winner receives the other hands' values).
- German-style point values default to J=20, Q=10, K=10, A=11; the traditional J=2, Q=3, K=4, A=11 values are selectable. Number cards count face value.
- Final special effects resolve by default: a final stacked Seven can be passed back, and a final Ace with replay requires another play or draw. A setting allows immediate-win treatment instead.

## Optional variants and exact semantics

- Deck: 32 (7–Ace), 36 (6–Ace, regional), 52 (2–Ace), or 64 (two 32-card packs); 2–4 seats; 5/6/7 cards per normal hand.
- Draw and calls: optional no-play-after-draw, voluntary draw, call requirement, and 0/1/2 missed-call penalty.
- Action cards: stack Seven, chain Eight, optional Nine reverse (two-seat skip choice), Ace none/replay/skip/reverse, optional 2 draw-two for 52-card decks (and stacking), and apply/ignore the initial upcard effect.
- Jack and ending rules: Jack-on-Jack; forbid finishing on Jack; Ten clears a Jack wish; Queen of Clubs requires King of Clubs or four cards; double scoring when the winning card is Jack; optional multiple same-rank cards; and final-card effects on/off.
- **Idioten-Mau** is an alternative fixed-round format: all players start with one card, and the opponent with the largest hand at the round end gets one additional starting card next round. A tie for largest hand is resolved clockwise from the round winner. Choose 3/5/7 rounds; most round wins takes the match, a tie is a draw. These tie and match-end choices are DeckForge's explicit deterministic convention where the source description leaves them unspecified.

The game-family source also lists platform “Training” and branded table features; those are not card-rule mechanics and are not part of this offline local-bankroll game. DeckForge does not claim to implement every national cousin such as Pesten, Prší, or Tschau Sepp as if it were ordinary Mau-Mau.
