# CardForge Implementation Plan

> **For Hermes:** Execute task-by-task, keep the HTML self-contained, and do not report completion before the browser and engine proofs are green.

**Goal:** Build an offline, single-file iPhone/iPad card-game app with a physically simulated deck/shoe, a real playable Blackjack mode, a real playable two-player Durak mode, persistent bankroll/progression, and a $100 bankruptcy grant.

**Architecture:** `index.html` is the only runtime artifact and contains all CSS and JavaScript inline. The engine is separated into small classes/functions inside the script (`Deck`, `Shoe`, `BlackjackGame`, `DurakGame`, `Bankroll`, `Progression`, persistence, UI). Every draw removes a concrete card object from a shuffled array; no game asks RNG for a fresh rank/suit during play. `window.CardForge` exposes the engine and a deterministic self-test suite for browser proofs without adding runtime dependencies.

**Tech Stack:** HTML5, CSS, vanilla ES modules-style code in one inline script, CSS playing cards, `localStorage`, `crypto.getRandomValues` with injectable seeded RNG for tests. No CDN, fonts, images, build step, or network dependency.

---

## Requirements ledger

| Requirement | Implementation | Acceptance proof |
|---|---|---|
| One offline single file | `index.html`; inline CSS/JS only | no external `<script>`, `<link>`, image or network request; `node --check` passes |
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
- Durak uses the 36-card ranks 6 through Ace, two players, one trump suit, a maximum of six cards in an attack, and standard Podkidnoy throw-in rules. The defender may beat an attack card or take the complete table. After a successful defense the defender attacks next; after a take, the same attacker attacks again. Drawing after each round is attacker first, then defender, up to six cards.
- A match stake is deducted when it begins. A Blackjack result pays the wager according to the hand result; a Durak win pays 2× the stake (stake included), a loss pays 0. A new wager at balance 0 grants exactly $100 first. Balance is never allowed below zero.
- Modes are intentionally simple and visible: four Blackjack tables and four Durak tables. Each next mode requires wins in that game and has a higher minimum stake. No hidden debug unlock is shipped.

---

## Phase 1: project shell and visual system

### Task 1: Create the project and plan checkpoint

**Files:**
- Create: `~/Documents/CardForge/index.html`
- Create: `~/Documents/CardForge/plan.md` (copy of this plan)
- Create: `~/Documents/CardForge/.hermes/plans/2026-11-23_172723-cardforge.md`
- Create: `~/Documents/CardForge/proofs/.gitkeep`

**Acceptance:** project directory exists, the two plan copies are identical, and the runtime file remains absent until the implementation task starts.

### Task 2: Build the responsive shell

**Files:** `index.html`

Add the viewport/meta tags, exact unified dark `:root` palette, sticky header, game tabs, bankroll pill, home screen, game screen, mode cards, felt/table region, action dock, modal/toast layer, and a compact diagnostics panel. Use `<button>` for every interactive action, `touch-action: manipulation`, `env(safe-area-inset-bottom)`, and no inline `onclick` handlers.

**Acceptance:** fresh page renders the home screen at desktop and mobile widths; all declared IDs exist; no overflow at 390×844.

### Task 3: Add card and chip rendering primitives

**Files:** `index.html`

Implement DOM renderers for a card (`rank`, `suit`, `face-down`, selected, disabled), hand fan/grid, chip buttons, badges, and deck audit summary. Add accessible labels and stable `data-testid` attributes.

**Acceptance:** card renderer produces the four suits, red/black colors, face-down state, selection state, and responsive card sizes without layout breakage.

---

## Phase 2: engine-first implementation (TDD order)

### Task 4: Implement random sources and physical decks

**Files:** `index.html`

Implement `RandomSource.crypto()`, `RandomSource.seeded(seed)`, Fisher–Yates, `Card`, `Deck`, `Shoe`, `draw`, `peek`, `discard`, `remaining`, `snapshot`, and `audit`. Use immutable card identity and an explicit zone map. Make the test suite injectable with a deterministic source while production uses crypto randomness.

**Tests:**
- 52-card standard deck has one card for each rank/suit.
- Six-deck shoe has 312 distinct serials and 24 copies per rank/suit.
- A shuffle changes order for a known seed but preserves the multiset.
- `draw(n)` removes exactly `n`, returned serials never recur, and `remaining` decreases.
- Audit catches a deliberately duplicated/missing card.

**Acceptance:** all deck tests pass and a real game state can expose the exact remaining count.

### Task 5: Implement bankroll, transactions, persistence, and progression

**Files:** `index.html`

Implement `Bankroll` with `balance`, `ensureFunds(amount)`, `debit`, `credit`, `grantBankruptcy`, and capped transaction history. Implement separate progression tracks, mode definitions, unlock rules, save/load schema versioning, reset-to-default, and corrupt-save recovery.

**Tests:**
- Debit rejects a wager larger than balance and never produces a negative balance.
- `$0` + new wager grants `$100` exactly once, then debits the wager.
- Blackjack/Durak payouts match their fixed result rules.
- Wins unlock only the next configured mode; locked modes reject start.
- Save/load round-trip preserves balance, wins, unlocks, and transaction history.

**Acceptance:** persistence works after reload and all economy values are visible in UI.

### Task 6: Implement Blackjack state machine

**Files:** `index.html`

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

**Files:** `index.html`

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

**Files:** `index.html`

Connect game selection, mode selection, bet/stake controls, start buttons, bankruptcy grant messaging, progression display, transaction history, save/reset, and game-over return actions. Keep the selected game/mode in URL-free local state and update all visible values after every mutation.

**Acceptance:** a user can start either game from a fresh page, cannot start a locked mode, and can reset a save without stale UI.

### Task 9: Wire Blackjack controls and table

**Files:** `index.html`

Render dealer/player hands, hidden dealer card, shoe count, current wager, totals (including soft/hard indicator), action buttons, split hands, insurance panel, outcome banner, and next-round control. Disable buttons from engine legality rather than duplicating rule conditions in event handlers.

**Acceptance:** real touch/click flow: choose Blackjack → choose mode → place wager → Deal → Hit/Double/Split/Insurance where legal → Stand → dealer resolves → bankroll/progression updates → next hand.

### Task 10: Wire Durak controls and table

**Files:** `index.html`

Render trump, talon count, player/AI hands, attack cards and target slots, selection state, contextual action buttons (`Attack`, `Throw In`, `Beat`, `Take`, `End Attack`), turn/status text, action log, and match result. AI turns use a short UI delay but remain deterministic through the injected engine.

**Acceptance:** real touch flow can attack, throw in, beat, take, finish a defense, draw, and reach a win or loss. No action button appears enabled when its engine action is illegal.

### Task 11: Add audio-safe microfeedback and mobile polish

**Files:** `index.html`

Add optional synthesized tap/deal/win/loss cues created only in a user gesture, mute toggle persisted locally, card motion classes, focus states, safe-area dock spacing, landscape/iPad layout, and reduced-motion support. No external audio assets.

**Acceptance:** every audio path is exception-safe when unavailable; mute state persists; buttons remain at least 44px high; reduced-motion removes transitions.

---

## Phase 4: proof, repair, and packaging

### Task 12: Add in-app self-test runner and proof export

**Files:** `index.html`, `proofs/` generated outputs

Expose `window.CardForge.runSelfTests()` and a Diagnostics modal that reports grouped assertions, pass/fail counts, and engine invariants. Include a proof export that serializes the latest test result and current deck audit without changing gameplay data.

**Acceptance:** self-tests run from a fresh page, report zero failures, and include counts for deck, Blackjack, Durak, economy, progression, persistence, and UI helpers.

### Task 13: Static and syntax audit

**Commands:**
```bash
python3 - <<'PY'
import re
from pathlib import Path
html=Path('index.html').read_text()
scripts=re.findall(r'<script[^>]*>(.*?)</script>', html, re.S)
Path('/tmp/cardforge-check.js').write_text('\n'.join(scripts))
print('scripts:', len(scripts), 'external:', bool(re.search(r'<(?:script|link)[^>]+(?:src|href)=', html)))
PY
node --check /tmp/cardforge-check.js
```

Also compare every `getElementById('...')` with an HTML `id`, check no external URLs/assets, check duplicate IDs, and inspect all buttons for listeners.

**Acceptance:** syntax passes, no missing IDs, no duplicate IDs, and the runtime has no external dependency.

### Task 14: Browser live QA and mobile geometry

Use Browser Use on a fresh `file://` page.

- Install error/unhandled-rejection hooks before interaction.
- Test home → Blackjack → deal → action → settlement.
- Test home → Durak → attack/defense/take/draw until terminal result.
- Run `CardForge.runSelfTests()` in the browser and read back the result.
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

1. `index.html` opens offline and has no external dependencies.
2. Both games can be started and completed through the UI, including player win/loss paths.
3. Physical deck/shoe audits pass and no card is generated ad hoc during play.
4. Bankroll, payouts, bankruptcy grant, progression, and persistence are tested.
5. Static checks, engine self-tests, browser flows, touch probes, and mobile geometry checks pass.
6. Plan and proof artifacts contain measured results and clearly list anything not tested on real hardware.

## Verification evidence (actual run)

- Runtime artifact: `index.html` — 97,967 bytes, 1,071 lines, one inline script, no external assets or network dependency.
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
- **Before/after proof:** final desktop strip center `512` at viewport `1024`, four cards at `230px`; final mobile strip center `195` at viewport `390`, four cards in two centered columns; browser self-tests remain `26/26`.
