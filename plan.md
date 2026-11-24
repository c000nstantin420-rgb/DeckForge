# DeckForge Implementation Plan

> **For Hermes:** Execute task-by-task, keep the HTML self-contained, and do not report completion before the browser and engine proofs are green.

**Goal:** Build an offline, single-file iPhone/iPad card-game app with a physically simulated deck/shoe, a real playable Blackjack mode, a real playable 2–4-seat Durak mode, persistent bankroll/progression, and a $100 bankruptcy grant.

**Architecture:** `deckforge.html` is the only runtime artifact and contains all CSS and JavaScript inline. The engine is separated into small classes/functions inside the script (`Deck`, `Shoe`, `BlackjackGame`, `DurakGame`, `Bankroll`, `Progression`, persistence, UI). Every draw removes a concrete card object from a shuffled array; no game asks RNG for a fresh rank/suit during play. `window.DeckForge` exposes the engine and a deterministic self-test suite for browser proofs without adding runtime dependencies.

**Tech Stack:** HTML5, CSS, vanilla ES modules-style code in one inline script, CSS playing cards, `localStorage`, `crypto.getRandomValues` with injectable seeded RNG for tests. No CDN, fonts, images, build step, or network dependency.

---

## Requirements ledger

| Requirement | Implementation | Acceptance proof |
|---|---|---|
| One offline single file | `deckforge.html`; inline CSS/JS only | no external `<script>`, `<link>`, image or network request; `node --check` passes |
| Genuine deck simulation | Fisher–Yates shuffle once; card objects are removed from `cards` by `draw`; discard/talon state is explicit | deck count decreases, drawn IDs never repeat, audit reports every card in exactly one zone |
| Blackjack | six-deck shoe, hit, stand, double, split, insurance, blackjack 3:2, dealer soft-17 policy, push/bust/payouts | direct engine tests plus a browser hand from deal through settlement |
| Durak | 36-card deck, trump, six-card deal, first attacker, attack/throw-in, beat, take, draw order, AI, end conditions | legal-move matrix, forced attack/defense/AI round, full browser match to winner |
| Currency | persistent dollars, wager deductions, payouts, mode stakes, visible transaction log | balance before/after assertions, reload persistence, no negative balance |
| Bankruptcy | exactly +$100 when balance reaches zero and user attempts a new wager/match | engine and UI test at `$0` verifies one grant and no repeated grants before spending |
| Progression | separate unlock tracks for Blackjack and Durak with increasing modes/stakes | locked mode cannot start; wins unlock exact next mode; UI reflects progress |
| iPhone/iPad UX | safe-area layout, large buttons, responsive table, touch-action, CSS cards, no horizontal overflow | 390×844 and 1024×768 geometry/overflow checks; real touch-dispatch on action buttons |
| Evidence | plan, test report, screenshots and measured browser output kept in project | `proofs/` contains JSON/report and unique screenshots; final response cites measured numbers |

## Rules fixed before implementation

- Blackjack uses six standard 52-card decks (312 cards) per shoe. Cards have unique `shoeId`/`serial`, even when rank and suit repeat. The shoe reshuffles only at a defined penetration threshold, never between individual draws.
- Blackjack dealer stands on soft 17. Natural Blackjack pays 3:2; a split 21 is not natural. Double-down is allowed on the first two cards of each hand; split hands may double. Insurance is available only against an Ace up-card and pays 2:1. One split is allowed initially, with aces receiving one card only.
- Durak uses the 36-card ranks 6 through Ace, 2–4 seats, one trump suit, a maximum of six cards in an attack, and standard Podkidnoy throw-in rules. The defender may beat an attack card or take the complete table. After a successful defense the defender attacks next; after a take, the same attacker attacks again. Drawing after each round is attacker first, then defender, up to six cards.
- A match stake is deducted when it begins. Blackjack results pay according to hand rules. A Durak winner receives `configured players × stake` (the full pot, including the human's ante); the human loses only their own ante on a loss, and draws return it. A new wager at balance 0 grants exactly $100 first. Balance is never allowed below zero.
- Modes are intentionally simple and visible: four Blackjack tables and four Durak tables. Each next mode requires wins in that game and has a higher minimum stake. No hidden debug unlock is shipped.

---

## Phase 1: project shell and visual system

### Task 1: Create the project and plan checkpoint

**Files:**
- Create: `~/Documents/DeckForge/deckforge.html`
- Create: `~/Documents/DeckForge/plan.md` (copy of this plan)
- Create: `~/Documents/DeckForge/.hermes/plans/2026-11-23_172723-deckforge.md`
- Create: `~/Documents/DeckForge/proofs/.gitkeep`

**Acceptance:** project directory exists, the two plan copies are identical, and the runtime file remains absent until the implementation task starts.

### Task 2: Build the responsive shell

**Files:** `deckforge.html`

Add the viewport/meta tags, exact unified dark `:root` palette, sticky header, game tabs, bankroll pill, home screen, game screen, mode cards, felt/table region, action dock, modal/toast layer, and a compact diagnostics panel. Use `<button>` for every interactive action, `touch-action: manipulation`, `env(safe-area-inset-bottom)`, and no inline `onclick` handlers.

**Acceptance:** fresh page renders the home screen at desktop and mobile widths; all declared IDs exist; no overflow at 390×844.

### Task 3: Add card and chip rendering primitives

**Files:** `deckforge.html`

Implement DOM renderers for a card (`rank`, `suit`, `face-down`, selected, disabled), hand fan/grid, chip buttons, badges, and deck audit summary. Add accessible labels and stable `data-testid` attributes.

**Acceptance:** card renderer produces the four suits, red/black colors, face-down state, selection state, and responsive card sizes without layout breakage.

---

## Phase 2: engine-first implementation (TDD order)

### Task 4: Implement random sources and physical decks

**Files:** `deckforge.html`

Implement `RandomSource.crypto()`, `RandomSource.seeded(seed)`, Fisher–Yates, `Card`, `Deck`, `Shoe`, `draw`, `peek`, `discard`, `remaining`, `snapshot`, and `audit`. Use immutable card identity and an explicit zone map. Make the test suite injectable with a deterministic source while production uses crypto randomness.

**Tests:**
- 52-card standard deck has one card for each rank/suit.
- Six-deck shoe has 312 distinct serials and 24 copies per rank/suit.
- A shuffle changes order for a known seed but preserves the multiset.
- `draw(n)` removes exactly `n`, returned serials never recur, and `remaining` decreases.
- Audit catches a deliberately duplicated/missing card.

**Acceptance:** all deck tests pass and a real game state can expose the exact remaining count.

### Task 5: Implement bankroll, transactions, persistence, and progression

**Files:** `deckforge.html`

Implement `Bankroll` with `balance`, `ensureFunds(amount)`, `debit`, `credit`, `grantBankruptcy`, and capped transaction history. Implement separate progression tracks, mode definitions, unlock rules, save/load schema versioning, reset-to-default, and corrupt-save recovery.

**Tests:**
- Debit rejects a wager larger than balance and never produces a negative balance.
- `$0` + new wager grants `$100` exactly once, then debits the wager.
- Blackjack/Durak payouts match their fixed result rules.
- Wins unlock only the next configured mode; locked modes reject start.
- Save/load round-trip preserves balance, wins, unlocks, and transaction history.

**Acceptance:** persistence works after reload and all economy values are visible in UI.

### Task 6: Implement Blackjack state machine

**Files:** `deckforge.html`

Implement shoe setup, wagers, initial deal, dealer hole card, action validation, hit, stand, double, split, insurance, dealer resolution, hand status, natural detection, exact payout settlement, discard, round reset, and shoe reshuffle threshold. Keep the engine UI-independent.

**Tests:**
- Ace total calculation handles soft/hard totals and bust correctly.
- Natural Blackjack pays 3:2; dealer natural pushes a player natural; ordinary 21 is not natural after split.
- Hit/stand/double validation rejects illegal actions; double debits the extra stake.
- Split creates two hands, moves one physical card, and applies the ace restriction.
- Dealer draws to 17 and stops on soft 17.
- Insurance is offered only against Ace and resolves at 2:1.
- All settlement paths conserve money and move used cards to discard.
- A seeded shoe produces the expected exact physical serial sequence.

**Acceptance:** engine can complete every legal action path without UI state leaking into rule logic.

### Task 7: Implement Durak state machine and AI

**Files:** `deckforge.html`

Implement the 36-card deal, trump, lowest-trump opening attacker, table attack objects, rank matching, beat rules, legal throw-in calculation, take, end-attack, draw order, next-attacker rules, terminal win/loss, and a deterministic heuristic AI. AI must never make an illegal move; it must choose the cheapest legal defense or take when none exists.

**Tests:**
- Deal gives six cards per player and a valid trump card/talon.
- Every beat combination (same-suit higher, trump over non-trump, trump-on-trump, off-suit non-trump) is correct.
- Throw-in only accepts ranks already on the table and respects hand/attack limits.
- Take transfers every table card to the defender and preserves card identity.
- Successful defense discards table cards and changes the attacker.
- Drawing occurs attacker first, then defender, never above six.
- AI action sequences remain legal until the match ends.
- Empty hand plus empty talon ends the game with the correct winner.
- Zone audit confirms all 36 cards are accounted for after every tested round.

**Acceptance:** a full AI-vs-AI seeded match terminates, and a player-controlled match can reach both win and loss outcomes.

---

## Phase 3: UI integration and complete playability

### Task 8: Wire home, modes, bankroll, and reset flows

**Files:** `deckforge.html`

Connect game selection, mode selection, bet/stake controls, start buttons, bankruptcy grant messaging, progression display, transaction history, save/reset, and game-over return actions. Keep the selected game/mode in URL-free local state and update all visible values after every mutation.

**Acceptance:** a user can start either game from a fresh page, cannot start a locked mode, and can reset a save without stale UI.

### Task 9: Wire Blackjack controls and table

**Files:** `deckforge.html`

Render dealer/player hands, hidden dealer card, shoe count, current wager, totals (including soft/hard indicator), action buttons, split hands, insurance panel, outcome banner, and next-round control. Disable buttons from engine legality rather than duplicating rule conditions in event handlers.

**Acceptance:** real touch/click flow: choose Blackjack → choose mode → place wager → Deal → Hit/Double/Split/Insurance where legal → Stand → dealer resolves → bankroll/progression updates → next hand.

### Task 10: Wire Durak controls and table

**Files:** `deckforge.html`

Render trump, talon count, player/AI hands, attack cards and target slots, selection state, contextual action buttons (`Attack`, `Throw In`, `Beat`, `Take`, `End Attack`), turn/status text, action log, and match result. AI turns use a short UI delay but remain deterministic through the injected engine.

**Acceptance:** real touch flow can attack, throw in, beat, take, finish a defense, draw, and reach a win or loss. No action button appears enabled when its engine action is illegal.

### Task 11: Add audio-safe microfeedback and mobile polish

**Files:** `deckforge.html`

Add optional synthesized tap/deal/win/loss cues created only in a user gesture, mute toggle persisted locally, card motion classes, focus states, safe-area dock spacing, landscape/iPad layout, and reduced-motion support. No external audio assets.

**Acceptance:** every audio path is exception-safe when unavailable; mute state persists; buttons remain at least 44px high; reduced-motion removes transitions.

---

## Phase 4: proof, repair, and packaging

### Task 12: Add in-app self-test runner and proof export

**Files:** `deckforge.html`, `proofs/` generated outputs

Expose `window.DeckForge.runSelfTests()` and a Diagnostics modal that reports grouped assertions, pass/fail counts, and engine invariants. Include a proof export that serializes the latest test result and current deck audit without changing gameplay data.

**Acceptance:** self-tests run from a fresh page, report zero failures, and include counts for deck, Blackjack, Durak, economy, progression, persistence, and UI helpers.

### Task 13: Static and syntax audit

**Commands:**
```bash
python3 - <<'PY'
import re
from pathlib import Path
html=Path('deckforge.html').read_text()
scripts=re.findall(r'<script[^>]*>(.*?)</script>', html, re.S)
Path('/tmp/deckforge-check.js').write_text('\n'.join(scripts))
print('scripts:', len(scripts), 'external:', bool(re.search(r'<(?:script|link)[^>]+(?:src|href)=', html)))
PY
node --check /tmp/deckforge-check.js
```

Also compare every `getElementById('...')` with an HTML `id`, check no external URLs/assets, check duplicate IDs, and inspect all buttons for listeners.

**Acceptance:** syntax passes, no missing IDs, no duplicate IDs, and the runtime has no external dependency.

### Task 14: Browser live QA and mobile geometry

Use Browser Use on a fresh `file://` page.

- Install error/unhandled-rejection hooks before interaction.
- Test home → Blackjack → deal → action → settlement.
- Test home → Durak → attack/defense/take/draw until terminal result.
- Run `DeckForge.runSelfTests()` in the browser and read back the result.
- Set 390×844 DPR 2 and 1024×768, reload in the same tab, assert no horizontal overflow, action dock inside viewport, and all visible buttons have usable rects.
- Dispatch real touch events to primary action buttons and verify state changes from engine/UI state, not just `click()`.
- Capture unique screenshots for home, Blackjack table, Durak table, diagnostics/test report, and a mobile viewport.

**Acceptance:** zero page errors, all named flows reach visible state changes, no mobile overflow, self-test failures = 0.

### Task 15: Final evidence and handoff

**Files:**
- Create: `proofs/self-test.json`
- Create: `proofs/browser-qa.json`
- Create: `proofs/static-audit.txt`
- Create: `proofs/home.png`, `proofs/blackjack.png`, `proofs/durak.png`, `proofs/mobile.png`
- Update: `plan.md` with measured verification and remaining limits

Record actual counts and measurements only: engine assertion total, browser errors, deck audit status, Blackjack/Durak terminal runs, balance transitions, mobile dimensions/overflow, and screenshot paths. If a physical iPhone/iPad or audio hardware was not available, state that explicitly instead of implying it was tested.

**Acceptance:** the final report points to real files and tool output; no fabricated proof numbers.

---

## Risks and tradeoffs

- Full split/insurance support increases Blackjack edge cases; the engine tests are mandatory before styling is considered complete.
- Durak is a state-heavy ruleset. The UI will use contextual controls over a stable engine rather than duplicating legality logic in the DOM layer.
- Browser automation can prove mobile geometry and touch dispatch but cannot replace a physical iPhone/iPad check; the final report will separate Chromium emulation from real-device validation.
- `localStorage` is intentionally local-only. There is no account, cloud save, or multiplayer networking in this offline first version.
- The app is a playable first release, not a casino-money product: currency is fictional and no payment or gambling integration exists.

## Definition of done

1. `deckforge.html` opens offline and has no external dependencies.
2. Both games can be started and completed through the UI, including player win/loss paths.
3. Physical deck/shoe audits pass and no card is generated ad hoc during play.
4. Bankroll, payouts, bankruptcy grant, progression, and persistence are tested.
5. Static checks, engine self-tests, browser flows, touch probes, and mobile geometry checks pass.
6. Plan and proof artifacts contain measured results and clearly list anything not tested on real hardware.

## Verification evidence (actual run)

- Runtime artifact: `deckforge.html` — 97,967 bytes, 1,071 lines, one inline script, no external assets or network dependency.
- Static audit: `node --check` passed; no duplicate IDs, no missing `$()` DOM references, no inline event attributes, and `crypto.getRandomValues` plus physical `this.cards.pop()` draw are present.
- Engine diagnostics: **26 passed / 0 failed**. Groups: physical shoe 5/5, bankroll/progression 4/4, Blackjack 8/8, Durak 7/7, persistence/UI helpers 2/2.
- Browser Blackjack: real lobby → deal → rendered action flow reached `settled`; the six-deck shoe reported 308 cards after the four-card deal and no page errors.
- Browser persistence/economy: a $10 start changed `$250 → $240` and the same `$240` survived reload; a zero-balance start visibly granted `$100`, debited the `$10` stake, and left `$90` with one grant.
- Browser Durak: real rendered-card/action flow reached `finished`; recorded player losses, a player win, and a draw across seeded-independent UI matches. The engine also completed bounded autonomous matches with physical-card audits on every step.
- Mode selection proof: desktop strip center `512` at 1024px with four 230px cards; mobile strip center `195` at 390px with two centered columns.
- Evidence files: `proofs/self-test.json`, `proofs/browser-qa.json`, `proofs/static-audit.txt`, `proofs/home.png`, `proofs/blackjack.png`, `proofs/durak.png`, `proofs/diagnostics.png`, `proofs/mobile.png`, `proofs/mode-selection.png`.
- Release artifacts: `VERSION` (`0.1.0`), `RELEASE_NOTES_0.1.0.md`, `README.md`, and the release archive are prepared after the final verification gate.
- Not tested: physical iPhone/iPad Safari hardware and real audio output. Chromium mobile emulation and touch-pointer dispatch were tested; currency is fictional and local-only.

### Nachtrag v0.1.0

- **Symptom:** The table-mode cards used the full lobby width instead of reading as a centered mode chooser.
- **Cause:** `.mode-strip` used a four-column full-width grid with no centered group width.
- **Change:** Replaced it with a centered, capped flex strip on desktop and a centered two-column grid on mobile; centered the mode heading and kept each desktop mode card at 230px.
- **Before/after proof:** final desktop strip center `512` at viewport `1024`, four cards at `230px`; final mobile strip center `195` at viewport `390`, four cards in two centered columns; browser self-tests were `26/26` at that release checkpoint.

### Lokaler Polish-Pass nach 0.1.0 (nicht als neuer Release getaggt)

- **Blackjack-Spielgefühl:** Dealer-Natural/Dealer-21 hat eine eigene Outcome-Karte statt generischem `Hand lost`; die doppelte Settlement-Meldung ist entfernt; Spielertotals bleiben auch im Live-State korrekt; `Deal next hand` startet Runde 2 direkt im Tisch und führt nicht mehr ins Hauptmenü.
- **Viewport-Fix:** Gameplay nutzt eine feste `100dvh`-Fläche mit In-Flow-Action-Dock. Final gemessen: `390×844 → scrollWidth 390 / scrollHeight 844`, `1024×768 → scrollWidth 1024 / scrollHeight 768` für Blackjack und Durak.
- **Durak Touch:** Player cards haben echte Pointer-Drag-Hooks, Drop-Zonen für Tisch und Angriffsziele, Ghost-Card, Highlighting und Trump-Karten mit Goldrahmen/Kronenmarkierung. Angriff und Verteidigung wurden über `PointerEvent` mit `pointerType=touch` durchgespielt; beide Pfade blieben legal und fehlerfrei.
- **Visual pass:** Home-CTA und 3-D-Kartenstapel, polierte Kartenflächen, klare Outcome-Karte, kompaktes iPhone-Durak-Layout und sichere Dock-Abstände. Screenshots: `proofs/polish-home-mobile.png`, `proofs/polish-blackjack-dealer21-mobile.png`, `proofs/polish-durak-drag-mobile.png`, `proofs/polish-diagnostics-mobile.png`.
- **Finale Beweise:** `29/29` Engine-Assertions, Static Audit PASS, keine Page Errors, `$250 → $240` über Reload persistent, Bankruptcy `$0 → $100 → $90` mit exakt einem Grant. Vollständige Messwerte: `proofs/polish-qa.json`.
- **Grenzen:** Physisches iPhone/iPad-Safari und echtes Audio wurden nicht getestet. Der Harness konnte keinen CDP-Hardware-Touch-Dispatch ausführen; deshalb ist der Drag-Nachweis ein echter gerenderter PointerEvent-Pfad mit Touch-Pointer, kein physischer Safari-Test. `VERSION`, Release-Archiv und Tag bleiben unverändert bei `0.1.0`.

### Follow-up: Header, Card-Rows und Reaktionspacing

- **Top-Bar:** Die Spielauswahl ist jetzt eine echte mittlere Grid-Spalte. Mobile Tabs werden auf `⌂ / BJ / DU` kompakt, aber vollständig dargestellt; gemessene Zentrierung: `390px → center 195px`, `1024px → center 512px`, jeweils innerhalb ≤ `0.01px`.
- **Blackjack-Cards:** Card rows sind nicht mehr horizontal scrollbar. Sie nutzen `overflow: visible` und `flex-wrap: wrap`; finaler DOM-Nachweis bei `390×844`: `scrollWidth === clientWidth === 328`, keine Scrollbar.
- **Reaktionszeit:** Dealer-Settlement zeigt zuerst einen statischen `Dealer is checking the hand…`-Zustand und wartet zufällig `320–760ms`; Durak-AI nutzt denselben Bereich zwischen Zügen. Nachweis: ein echter Durak-AI-Timer mit `415ms`, anschließend `turn:0`, `phase:defense`, keine Fehler. Keine neue Animation hinzugefügt.
- **Follow-up-QA:** `29/29`, `node --check PASS`, keine DOM-/Runtimefehler, Persistence und Bankruptcy erneut grün. Aktuelle Messwerte und Screenshots: `proofs/polish-qa.json` plus fünf `polish-*.png`-Beweise.
- **Release-Grenze:** Kein Version-Bump, kein Commit, kein neues Archiv und kein Tag.

### Multiplayer und Table-Varianten umgesetzt

- **Lobby-Konfiguration:** Beide Spiele haben eine sichtbare `Table setup`-Karte. Blackjack: `1–4 seats`, Natural `3:2/6:5`, Soft-17 `Stand/Hit`, Insurance und Surrender. Durak: `2–4 seats`, `All players/Neighbors` beim Einwerfen und `Push allowed/off`.
- **Blackjack:** Ein menschlicher Sitz plus AI-Sitze werden physisch aus demselben Shoe gedealt. AI-Sitze spielen vor dem menschlichen Zug; Settlement bewertet alle vier Sitze, aber nur Sitz 0 verändert die Bankroll. 6:5, Soft-17, Insurance und Surrender sind Engine-Regeln, nicht nur UI-Texte.
- **Durak:** 2–4 Sitze, zirkuläre Sitznachbarn, Nachbar-only/all throw-in, optionales gleichrangiges Weiter-Schieben und AI-Turn-Progression. Vier-Sitz-Autoplay endet nach `54` Schritten mit gültigem Audit.
- **Live-UI:** Aktiver Tisch zeigt die Konfiguration im Header/Table-Badge, kompakte AI-Sitz-Lanes, menschliche Hand und variantenspezifische Actions. Gegnerkarten werden in Durak mobil als Kartenrücken plus Kartenanzahl dargestellt; die menschliche Hand nutzt eine lesbare Fan-Anordnung mit Scroll-Fallback, damit Tisch/Hand bei `390×844` sichtbar bleiben.
- **Finale Beweise:** `39/39` Assertions, Static Audit PASS, 4-seat Blackjack `participants:4/results:4/audit:true`, 4-seat Durak `finished/steps:54/audit:true`, 2-seat Push `attacker:1/defender:0/audit:true`, 4-seat-only All-player throw-in, Throw-in Off, every table setting has an English help button, redesigned felt/card-room home preview, Konfigurations-Reload persistent, Drag-Angriff weiterhin `table:1/hand:5/phase:defense`, keine Browserfehler. Vollständige Daten: `proofs/multiplayer-variants-qa.json`.
- **Release status:** `VERSION` is `0.2.3`; the user explicitly requested the commit, tag, public GitHub repository, and Pages deployment after the final verification gate.

### Blackjack single-hand outcome fix

- **Symptom:** A completed one-hand Blackjack round showed the same outcome banner twice.
- **Cause:** `renderBlackjack()` rendered a per-hand outcome and the overall settlement banner for the same single hand.
- **Change:** Keep per-hand banners for split hands; use only the overall settlement banner for a single hand.
- **Before/after proof:** At `390×844`, a real deal/stand/settle flow changed `.outcome-banner` count from `2` to `1`; action buttons remained visible, document size stayed `390×844`, browser errors `0`, diagnostics `39/39`, `node --check` PASS.
- **Release:** After the fix commit, the user approved release `0.2.3`; `VERSION`, `RELEASE_NOTES_0.2.3.md`, tag `v0.2.3`, and `releases/DeckForge-0.2.3.zip` are the release artifacts.

### Blackjack split/double repair — 0.2.4

- **Baseline defect:** In v0.2.3, seed `52` left a doubled split hand at `stood=false`, `canAct=true`, `activeIndex=0`; Hit/Stand remained available and Hand 2 never became active.
- **Engine fix:** Double now draws exactly one physical card, marks that hand closed, and advances to the next playable split hand or settles the round. Added tests for below-21 double, illegal post-double actions, split hand advancement, split Aces, split 21 payout, and the one-split rule.
- **UI fix:** Split hands carry explicit `YOUR TURN`, `UP NEXT`, `DOUBLED`, `STOOD`, `WON`, `LOST`, or `PUSH` badges; the active hand is highlighted and the action prompt names its hand. Mobile stacks hands with compact per-hand net badges; tablet uses two columns. Settlement keeps one aggregate banner.
- **Final verification:** 44/44 diagnostics; `node --check` PASS; static audit `171507` bytes / `1589` lines, zero external assets, duplicate IDs, missing DOM references, inline handlers, and browser errors. Browser QA passed solo double and split→double→Hand 2→settlement at `390×844`, 4-seat split at `390×844`, and tablet split at `1024×768`; no clipping or document overflow.
- **Evidence:** `proofs/blackjack-split-double-qa.json` plus mobile, multi-seat, and tablet screenshots.
- **Release:** `VERSION` / `APP_VERSION` `0.2.4`; notes `RELEASE_NOTES_0.2.4.md`; archive `releases/DeckForge-0.2.4.zip`; tag `v0.2.4`.

### DeckForge 0.2.5 — multiplayer correctness, Durak feel, visible version

- **Blackjack:** Fixed the multi-seat scheduler handoff after either insurance choice. Fresh UI runs passed for 3-seat Take/No-thanks and a 4-seat table through all three AI turns, a human action, settlement, and shoe audit.
- **Durak rules:** Corrected clockwise attacker/defender transitions after defense, take and push; refill order; empty-seat handling; end-of-match Durak/draw outcomes; and one-pass-per-bout throw-in tracking. The latter prevents `all players` from ping-ponging forever among seats with no matching rank. Empty AI turns now advance, and any remaining no-op is surfaced as a paused state instead of a repeated timer.
- **Durak UI:** Human hand display sorts by suit/rank with trumps last without mutating the engine hand. Table cards, red suits, compact trump/talon, defense target and drop highlight were improved. The exposed trump is no longer duplicated after the talon is empty.
- **Visible release version:** Header badge and accessible brand label show `v0.2.5` on home and live Blackjack/Durak screens, including 390px mobile width.
- **Engine evidence:** `proofs/self-test.json` records 60/60 assertions. `proofs/browser-qa.json` records 100 autonomous matches across five 2–4-seat variants, full rendered-UI matches for 2/3/4 seats, insurance paths, drag/drop behavior, reaction measurements and viewport geometry.
- **Timing evidence:** UI action-to-first-AI callback measured 328ms (low), 546/547ms (mid) and 775ms (high including browser/timer dispatch overhead); scheduled delay remains 320–760ms.
- **Responsive evidence:** At 390×844 and 1024×768, six-pair Durak fixtures had no document overflow and every action control stayed within the viewport.
- **Limits:** Physical iPhone/iPad Safari and real audio were not tested. CDP hardware-touch dispatch timed out; synthetic `PointerEvent` touch-pointer handlers were exercised. QA browser contexts were isolated; no test bankroll or active game is included in repository artifacts.
- **Release artifacts:** `VERSION` / `APP_VERSION` `0.2.5`; notes `RELEASE_NOTES_0.2.5.md`; archive `releases/DeckForge-0.2.5.zip`; annotated tag `v0.2.5`. The Pages workflow deploys the release tag.

### DeckForge 0.2.6 — Durak landscape, queued throw-ins, and next-round stake

- **Durak action dock:** Compact landscape layout covers six attack pairs and 2–4 seats. Final Chromium geometry: `390×844`, `1024×768`, `1024×600`, `1194×834`, `1194×700`, `1366×768`, `1366×600`; all buttons/dock inside the viewport, hand above dock, document dimensions equal the viewport. Full measurements: `proofs/durak-0.2.6-layout.json`.
- **AI reaction / pile targeting:** Human can select a matching card and a specific covered target while an eligible AI acts, queue it without changing turn or hand, cancel/continue, and execute only on the human turn. A valid touch-pointer drag queues the selected pile. Stale targets clear safely. Details: `proofs/durak-0.2.6-throw-in-qa.json`.
- **Optional push:** `Push onward` appears enabled before a card is selected when legal; defense, push, and take remain separate actions. Proof: `proofs/durak-0.2.6-push-qa.json`.
- **Next-round stake:** Blackjack and Durak result screens offer table-bounded next-stake presets beside the rematch action. Browser QA starts both next rounds at `$20` and confirms a finished Durak result remains `$10` after changing the next wager. Proof: `proofs/durak-0.2.6-next-wager-qa.json` and `proofs/durak-0.2.6-next-wager-layout.json`.
- **Endgame safety:** Seed `261201` previously repeated a complete four-seat `all players`, no-push position. It now resolves at step `77` as a visible stalemate draw, returns the `$10` wager exactly, and preserves the 36-card audit. Proof: `proofs/durak-0.2.6-stalemate.json`.
- **Final tests:** 69/69 in-app assertions; 168 seeded autonomous matches across 14 valid 2–4-seat policy/push combinations, all audited each action and terminal-checked; complete rendered-UI matches for 2/3/4 seats; Blackjack deal/stand/settlement/next-wager flow; `node --check` PASS; static audit PASS (`230717` bytes, `1915` lines, one inline script, no external refs/duplicate IDs/missing DOM refs/inline handlers); `git diff --check` PASS.
- **AI timing:** scheduled planning window `1400–2200ms`, normal/queued response `320–760ms`; observed callback examples `1651ms`, `2585ms`, `2293ms`, and queued `1345ms` including headless timer jitter. Continue AI and the 12-second timeout both resumed correctly. Proof: `proofs/durak-0.2.6-ai-timing.json`.
- **Visible version and screenshots:** header shows `v0.2.6` on home, Blackjack, and Durak; screenshot proofs are `proofs/deckforge-0.2.6-*.png`.
- **Limits:** physical iPad Safari and real audio were not tested. Chromium device emulation and synthetic touch-pointer events were tested.
- **Release artifacts:** `VERSION` / `APP_VERSION` `0.2.6`; notes `RELEASE_NOTES_0.2.6.md`; archive `releases/DeckForge-0.2.6.zip`; annotated tag `v0.2.6`. The final handoff records the commit SHA and verified remote deployment.

### DeckForge 0.2.6.1 — Durak hand sizing, batch attacks, and all-seat pot

- **Hand layout:** Six-card Durak hands use small whole cards with visible gaps; larger hands scroll rather than overlap. Vision and DOM checks passed at iPhone-emulated `390×844` (DPR3) and iPad-landscape-emulated `1194×834` (DPR2).
- **Multi-card attacks:** Tap same-rank cards to add/remove a batch, then attack together. Batches also work by dragging a selected card onto the table. Targeted throw-in batches insert beside the selected covered pile, preserve links, and pass the 36-card audit. Single-card actions remain available.
- **Push drag:** The enabled `Push onward` button is a real drop target with a high-contrast amber hover state. Tap and drag paths both transferred a legal card and preserved the 36-card audit.
- **Pot settlement:** The Durak winner receives `players × stake`; loss forfeits only the human ante and draw/stalemate refunds it. A four-seat `$10` winner was settled through the rendered action: `$40` credited, `$30` net, `$280` bankroll header after starting at `$250` and deducting the ante.
- **Final tests:** 75/75 in-app assertions; 168/168 seeded matches across 14 2–4-seat configurations with a card audit on every move; a real two-seat lobby-to-settlement browser flow; queue/continue/timeout and batch/tap/drag UI paths; no browser errors.
- **Layout matrix:** `390×844`, `1024×768`, `1024×600`, `1194×834`, `1194×700`, `1366×768`, `1366×600`; every hand/dock stayed within its bounds, no card overlap/clipping, document equals viewport. Evidence: `proofs/durak-0.2.6.1-layout.json`.
- **Vision proofs:** iPhone, iPad, Push onward drop highlight, and four-seat payout screenshots are in `proofs/deckforge-0.2.6.1-*.png`; structured results are in `proofs/browser-qa-0.2.6.1.json`.
- **Limits:** Physical iPhone/iPad Safari and real audio hardware were not tested; checks used Chromium emulation and synthetic touch-pointer dispatch.
- **Release artifacts:** `VERSION` / `APP_VERSION` `0.2.6.1`; notes `RELEASE_NOTES_0.2.6.1.md`; archive `releases/DeckForge-0.2.6.1.zip`; annotated tag `v0.2.6.1`. Acceptance: read back the published tag/asset, Pages deploy/build, live runtime bytes/version, and clean worktree.
