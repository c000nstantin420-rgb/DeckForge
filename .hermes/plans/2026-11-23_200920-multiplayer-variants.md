# Multiplayer and Table Variants Implementation Plan

> **For Hermes:** Execute this plan in the current local workspace. Do not commit, tag, bump `VERSION`, or rebuild a release archive unless explicitly requested.

**Goal:** Let DeckForge configure player count and meaningful Blackjack/Durak table variants in the lobby, carry those rules into the live table, and keep the experience readable on mobile.

**Architecture:** Keep the single-file/offline architecture. Extend the rule engines with explicit table configuration objects, preserve the current two-player/standard-rule defaults for compatibility, and use one human seat (seat 0) plus AI seats for additional players. The lobby becomes the source of truth for the table config; the active table renders a compact rules/seat summary and exposes the relevant participant state.

**Tech Stack:** One inline HTML/CSS/ES-module-style script, no external assets, existing `DeckForge` API and in-app self-tests, browser viewport/pointer QA.

---

## Requirements ledger / decisions

- Both games expose a player-count selector in the lobby. Default remains the current behavior: Blackjack 1 human seat, Durak 2 seats.
- Blackjack supports 1–4 seats total, with seat 0 controlled by the user and additional seats controlled by short-delay AI.
- Blackjack table variants are explicit and actually used: blackjack payout `3:2` or `6:5`, dealer soft-17 `stand` or `hit`, insurance `on` or `off`, and surrender `on` or `off`. Defaults preserve current rules: `3:2`, stand, insurance on, surrender off.
- Durak supports 2–4 seats total, with seat 0 controlled by the user and additional seats controlled by AI.
- Durak throw-in variants: `All players` or `Neighbors only`. “Neighbors” means the circular seats directly adjacent to the current defender; the current attacker remains eligible in the two-player default.
- Durak transfer/push is configurable: `Push allowed` or `Push disabled`. A push requires a same-rank card and transfers defense to the next seat without discarding the table.
- Settings are persisted as versioned table preferences, but active matches are not persisted.
- The lobby must show the effective rules in plain language. The active table must show seat count and a compact variant summary; mobile layout must not reintroduce horizontal card scrolling or viewport overflow.
- No new animation is required; timing feedback may use static “AI is thinking” states.

## Task 1: Add failing engine/config tests

**Files:** `deckforge.html`

- Extend `runSelfTests()` with failing assertions for:
  - normalized Blackjack config defaults and custom variants;
  - Blackjack multi-seat deal identity and AI-seat state;
  - 6:5 payout and hit-soft-17 branch;
  - Blackjack insurance/surrender disabled state;
  - Durak 2–4 seat deal and circular next-seat helper;
  - all-player vs neighbor-only throw-in legality;
  - push/transfer eligibility and disabled push rejection;
  - table settings persistence shape without active game objects.
- Run the in-app diagnostics and confirm the new tests fail before implementation.

## Task 2: Build normalized table configuration

**Files:** `deckforge.html`

- Add constants for supported player counts and variant definitions.
- Add `normalizeBlackjackConfig`, `normalizeDurakConfig`, and `tableConfigSummary` helpers.
- Add table preferences to the saved settings schema with backwards-compatible defaults.
- Keep `BlackjackGame({})` and `DurakGame({})` behavior identical to the current default rules.
- Wire lobby control state through `appState.tableConfig` and ensure switching games loads that game’s saved preferences.
- Make the configuration visible to the `DeckForge` test API for deterministic browser probes.

## Task 3: Extend Blackjack seats and rules

**Files:** `deckforge.html`

- Extend `BlackjackGame` with a normalized config, participant seats, human seat 0, and AI participant metadata while retaining `playerHands` as the human compatibility alias.
- Deal one physical shoe to all configured seats and the dealer; audit every physical serial.
- Add an AI-seat step/strategy that hits below 17 and stands at 17+, with UI scheduling between actions.
- Resolve every seat against the dealer but expose `humanPayout`/human results for the bankroll. AI stakes remain table-only and cannot mutate the user bankroll.
- Apply payout variant (3:2 or 6:5), soft-17 behavior, insurance toggle, and surrender toggle in the engine—not only in labels.
- Keep existing one-seat self-tests and action methods legal under the default config.

## Task 4: Extend Durak seats and variants

**Files:** `deckforge.html`

- Generalize the deck deal and player list to 2–4 seats while preserving `playerHand` and `aiHand` compatibility aliases.
- Add circular seat helpers and AI turn progression for attack, defense, draw-up, and match termination.
- Implement throw-in policy:
  - `all`: any non-defender seat may legally throw in when the table rank permits;
  - `neighbors`: only seats adjacent to the defender may throw in.
- Implement optional push/transfer: the defender may play a same-rank card to move defense to the next seat; keep the table cards intact and update defender/turn state.
- Ensure the human seat can act when it is attacker, throw-in player, defender, or new defender after a push; AI seats resolve with the existing random reaction delay.
- Keep default 2-player behavior equivalent to the current match rules.

## Task 5: Build the lobby configuration UI

**Files:** `deckforge.html`

- Add a clean `Table setup` panel next to the existing wager controls.
- Use segmented controls/cards for player count and compact variant toggles/selectors, not dense raw `<select>` elements.
- Make Blackjack show seats + payout + soft-17 + insurance + surrender.
- Make Durak show seats + throw-in policy + push/transfer.
- Display a one-line “This table” summary and disable impossible/locked combinations clearly.
- Persist changes immediately through the versioned settings save, without saving an active match.
- Keep desktop centered and mobile stacked, with accessible labels and touch targets.

## Task 6: Render configured live tables

**Files:** `deckforge.html`

- Blackjack: show opponent seat lanes, names/status, visible cards, and a compact rules badge. Keep the human action dock visually dominant.
- Durak: show seat labels/order, current defender/attacker, variant badge, push action when legal, and a concise table log entry for policy/push events.
- Avoid scrollable card rows; use wrapping and measured card sizes. Verify the table summary and player lanes at `390×844` and `1024×768`.
- Keep “AI is thinking” static and readable; use the existing random delay instead of adding a new animation.

## Task 7: Browser acceptance and proof artifacts

**Files:** `proofs/multiplayer-variants-qa.json`, `proofs/multiplayer-variants-static-audit.txt`, screenshots under `proofs/`, `plan.md`, `.hermes/plans/2026-11-23_172723-deckforge.md`

- Run syntax/static audit and in-app diagnostics.
- Browser-probe lobby configuration for both games, reload persistence, and active-table summaries.
- Start Blackjack with 1 and 4 seats; verify physical serial audit, AI turn progression, custom payout/soft-17 behavior, and next-hand flow.
- Start Durak with 2 and 4 seats; verify all-player and neighbor-only throw-in legality, push enabled/disabled, human drag attack/defense, AI progression, and terminal audit.
- Verify no horizontal or vertical document overflow and no scrollable card row at `390×844` and `1024×768`.
- Capture updated home/lobby/Blackjack/Durak/diagnostics screenshots and record exact values. Confirm `VERSION` unchanged and `git diff -- VERSION` empty.

## Risks / tradeoffs

- Multi-seat AI increases asynchronous state transitions; every timer must be cleared on navigation and every terminal payout must be idempotent.
- Generalizing Durak turn order can regress the current 2-player rules; preserve compatibility aliases and run the old autonomous match assertions unchanged.
- More configuration increases mobile density; use progressive disclosure/stacked setup cards rather than shrinking controls below touch-safe sizes.
- Blackjack payout variants affect economy only at settlement; tests must assert exact human payout and ensure AI seats never credit the user wallet.
