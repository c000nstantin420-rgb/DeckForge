# UNO deck mode for Mau-Mau — rules and variants

This note defines the app's card/deck switch. `UNO` is a product name; the card faces in DeckForge are original CSS artwork using the public card colors, values, and action names, not scans or copies of Mattel's card art.

## Sources checked

- [Mattel M1006-0824 instruction sheet (PDF)](https://service.mattel.com/instruction_sheets/M1006.pdf). This *Batman The Dark Knight UNO* edition lists 19 number cards per color, two each of Draw Two / Reverse / Skip per color, four Wilds and four Wild Draw Fours, plus four edition-specific Joker's Wild cards (112 total). This demonstrates that special extra cards vary by edition; DeckForge's optional +4 are generic custom Wilds, not Batman/Joker cards.
- [Mattel HXY06-0970G1 instruction sheet (PDF)](https://service.mattel.com/instruction_sheets/HXY06-Eng.pdf). The 2024 targeted-action edition specifies the common seven-card deal, one-card turn, color/number/symbol match, draw one/play if playable, UNO call and two-card catch penalty, Wild Draw Four color restriction/challenge, and optional 500-point scoring. It also has targeting-specific cards that are intentionally not imported here.
- [UNO! official mobile-game rules](https://www.letsplayuno.com/support/rules.html). Its 7 hand-swap / 0 rotate behavior is a digital-game variant; it is not presented here as a universal physical-deck rule.
- [UNO Rules: popular house rules](https://uno-rules.com/house-rules/) and [UNO Rules Canada: house rules](https://unorules.ca/uno-house-rules/). Stacking, Jump-In, Seven-Zero, and draw-until-playable are commonly agreed house variants, not a universal classic ruleset.

## Deck composition

The default `Classic 108` deck contains:

- Four colors: red, yellow, green, blue.
- Per color: one 0; two each of 1–9; two each of Skip, Reverse, and Draw Two (25 cards per color; 100 total).
- Four Wild and four Wild Draw Four cards (8 more; 108 total).

Optional `Add 4 custom Wilds` makes 112 cards. They act as ordinary Wilds in this implementation until the user-defined house-effect editor exists; they do not imitate any licensed themed card.

## Classic defaults in DeckForge

- Deal seven cards; play exactly one matching color, number, or action symbol. A Wild is playable on any turn.
- If no card can be played, draw one. If it is playable, the player may play it immediately or keep it and end the turn. Players may choose to draw even if they already hold a legal card.
- Skip loses the next turn. Reverse changes direction; with two seats, Reverse acts as Skip. Draw Two makes the next seat draw two and lose the turn.
- Wild chooses the continuing color. Wild Draw Four chooses a color, makes the next seat draw four and lose the turn, and is legal only if the player has no card of the current color (a matching number/action in another color does not block it). The next seat may challenge: if illegal, the player who used it draws four; if legal, the challenger draws six. The chosen color remains active.
- Call `UNO` after a play leaves one card. If caught before the next player's turn begins, the player draws two cards by default. The penalty size and call requirement are configurable.
- The final Draw Two / Wild Draw Four resolves before the hand winner is scored. Numbers score face value; action cards score 20; Wilds and Wild Draw Fours score 50. The default match ends when a player reaches 500 points.

## Optional house settings

- **Starting hand:** 5, 7, or 10 (7 is classic).
- **Draw stacking:** Off (classic), same-type only, or any Draw Two / Wild Draw Four. Stacking delays the draw/skip until the chain ends.
- **Jump-In:** A human may play an exact color-and-symbol duplicate of the discard out of turn; play proceeds from that seat. It is disabled while a draw/challenge/hand-swap response is pending.
- **Seven-Zero:** A played 7 swaps the player’s hand with one chosen seat; a played 0 rotates all hands in the current direction. Both take effect before going-out settlement.
- **Draw until playable:** Continue drawing instead of stopping at one card; if no playable appears before the stock is exhausted, pass the turn.
- **Multiple same-number cards:** Optional same-rank batch play; action effects resolve using each selected physical card. Classic remains one-card-per-turn.
- **Scoring:** Classic point race (250 / 500 / 1000) or round wins (first to 1 / 3 / 5).
- **Two-seat Reverse:** Skip (classic special case) or plain Reverse/no net order change.

Every house option remains opt-in and is shown as a house variant in the setup help and active rule summary.
