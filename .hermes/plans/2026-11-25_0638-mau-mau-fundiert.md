# Mau-Mau Implementation Plan

> Implementation plan for the current DeckForge task; execute in this session.

**Goal:** Add a polished, playable Mau-Mau table with a documented common-rule default and configurable popular house rules.

**Architecture:** Add a pure `MauMauGame` state machine that uses DeckForge's physical `Card`/`PhysicalShoe` identities, then integrate it as a third game through the existing home, lobby, persistence, bankroll, progression and responsive table shell. Put all rule choices in one normalized, persisted configuration; do not let render/event code reimplement legality.

**Tech Stack:** One offline `deckforge.html` ES-module-style IIFE, built-in seeded RNG/self-tests, existing browser harness and screenshot/Vision QA.

---

## Rules research and chosen baseline

Mau-Mau has no single official rulebook; sources explicitly separate its stable core from family/region variants. Use the German-speaking Palast baseline: 32 cards (7–Ace), 2–4 seats, five cards each; match suit or rank; draw one when unable/unwilling; 7 draws two, 8 skips, Jack wishes a suit. Defaults: stack 7s, draw one and allow an immediately playable card, do not allow Jack-on-Jack, and require a one-card “Mau” call.

Expose rather than hard-code the widely documented alternatives: 32/36/52/64-card packs; 5/6/7 starting cards; play-after-draw; 7 stacking; 8 stacking; 9 reverse; Ace action (none/replay/skip/reverse); 52-card 2-as-draw-two; Jack-on-Jack; Jack-to-finish; opening-card effects; multi-card same-rank play; missed-call penalty; match target; and selected common Palast rules (10 clears a Jack wish, Queen of Clubs creates a four-card challenge, Jack finish bonus). Keep each rule's constraints and help text explicit. No Pesten/Jokers, Prší, or Swiss Tschau-Sepp full ruleset are silently mixed into the Mau-Mau baseline.

Sources consulted:
- Mau-Mau Palast, “Regelvielfalt” (their common 32-card/2–4 seat baseline and advertised setup/effect toggles): https://www.mau-mau-palast.de/lektion-3-regelvielfalt-beim-mau-mau/
- HowTo Cards (32/52/64 deck options, 5/6 cards, play-after-draw, 7/8/Jack, Mau calls, Ace/9/2 variants): https://howto.cards/en/mau-mau/
- Spielstrategen, evidence/variant comparison (no official rules; regional deck/action differences): https://spielstrategen.de/spiele/mau-mau/regeln/
- Pagat’s Eights-group overview and Crazy Eights rules (family relationship; variants are not automatically Mau-Mau rules): https://www.pagat.com/eights/ and https://www.pagat.com/eights/crazy8s.html

## Requirements ledger

| Area | Contract | Acceptance proof |
|---|---|---|
| Home/game routing | Add a third Home card; keep mode selection out of the top bar; brand still returns Home. | DOM test and actual click flow. |
| Decks/deal | 32 (7–A), 36 (6–A), 52 (2–A), or 64 (two 32-card packs); 2–4 seats; hand size 5/6/7; physical unique serials. | Exact rank/suit/count and post-deal audit for every deck and seat count. |
| Core turn | Match suit/rank, Jack wish, draw/recycle, no legal moves/pass, explicit direction and end states. | Legal/illegal action tests and full-deck audit after every transition. |
| Popular rules | Explicit normalized options with UI constraints/help; effect handlers for 7/8/9/A/2/Jack, draw timing, multiple cards, start card, Jack restrictions, wish-clearing and Club Queen variant. | One deterministic test per rule and pairwise interaction tests. |
| Calls/scoring | Explicit Mau/Mau-Mau announcement windows and penalties; round/match score target and optional Jack finish bonus. | Missed/announced call tests, finish tests, score and bankroll settlement assertions. |
| AI | Legal shedding policy, suit choice from hand, appropriate special-card responses, bounded progress and stalemate handling. | Seeded bounded match matrix; audit each action; no repeated-state loops. |
| Persistence/economy | Persist normalized table settings and Mau-Mau progression; active match remains unsaved; unfinished wager refunds on Home/reload. | Reload settings, refund, win/loss/draw settlement and progression tests. |
| Visual/UI | English labels/help, progressive disclosure on mobile, visible hand/stock/discard/turn/scores/action dock. | Phone/tablet/desktop geometry plus inspected Home, setup, live, Jack-wish, call, and settled captures. |

## Implementation tasks

1. **Add failing tests first** in `deckforge.html` for config normalization, exact physical deck composition, draw/recycle, rank/suit/Jacks, each action rule, call penalties, match scoring, AI bounds and audit conservation. Run `DeckForge.runSelfTests()` and preserve the red result before adding the engine.
2. **Implement pure config and engine**: `normalizeMauMauConfig`, card-rank sets, explicit phases, legal action predicates, physical-zone audit, draw-pile recycling, calls, optional effects, round/match results and a bounded AI policy. Keep all UI state outside the engine.
3. **Integrate save/progression/economy**: add the Mau-Mau defaults to old-save-safe normalization, a win track/table modes, human stake debit/refund/payout and end-of-match progression.
4. **Wire Home/lobby/table**: add Home card, table title/description, normalized settings with `?` help, mobile collapsed summary, Rules modal, fresh-deal/start and action event delegation; add a dedicated Mau-Mau content panel.
5. **Build the table UI**: opponents, discard/top wish, draw stock count, selected/tappable hand, Jack suit picker, explicit Mau/Mau-Mau calls, draw/play actions, response windows, AI thinking, score, outcome and next-match controls. Make every action's visible copy reflect actual state.
6. **Run all engine and static tests**, then fresh `file://` browser checks with error/rejection hooks. Exercise real taps (not only direct engine calls), persistence/refund, AI response, and at least one full player-controlled round from deal to settlement; save structured proofs/screenshots.
7. **Inspect final screenshots with Vision** and re-run affected tests after any visual fix. Verify widths/scroll, tappable hand cards, modal/help text, no console errors and correct physical-card audit.
8. **Update `README.md`, `plan.md` and Mau-Mau QA notes** with the chosen rules and measured results. The user later requested the explicit `0.2.6.7` release; after all tests and browser gates pass, update version files, package the evidence, publish the tag/release, and verify Pages and the downloadable asset.
