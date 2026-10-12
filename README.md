# Table Ledger

A single-file, offline **blackjack earnings calculator** (fake money) — not the game itself.
It keeps the books for a table of players: entry fees, per-player bets, escalating raises,
tie splits, carry-over pots, and loans with interest.

Open `Table Ledger.html` in any browser. No install, no server, no internet needed
(the Google Fonts link degrades gracefully offline). Your data is saved in the
browser's `localStorage`.

## Rules the calculator implements

1. **Entry fee** — everyone at the table pays the same entry fee into the pot.
2. **After the first 2 cards** — each player **bets their own amount** (amounts can
   differ per player) or **folds**. Folding forfeits everything paid so far.
3. **Extra-card rounds (raises)** — the first player to bet (the *raiser*) sets the
   raise and pays it. Every other player still in must **add at least that raise**
   or fold. Blank amounts auto-call the raise.
4. **Escalating raises are enforced** — each raise must be at least the previous
   round's highest bet. A lower raise is snapped up to the minimum on blur, and the
   **Settle button stays disabled** while any rule violation exists. The
   **Auto-fix raises** button repairs everything in one tap.
5. **Highest total of 21 or less wins the pot.** Over 21 is a bust.
6. **Ties split the pot** equally (odd cent carries forward to the next game).
7. If **one player is left**, they win without showing a hand.
8. If **everyone is out or bust**, the whole pot carries to the next game.

**Net** = what a player gets back minus everything they paid in (entry + bets).

## Features

- **Per-player betting** with a shared entry fee; fold/undo per round.
- **Strict escalating-raise rule** with live violation warnings and an auto-fix.
- **Collapsible sections** on the Table tab (Who is playing / Betting rounds /
  Final hands / Result) with live status coins — less scrolling on phones.
- **Mobile-first UI**: bottom tab bar, 44px+ touch targets, safe-area insets,
  light/dark theme following the system.
- **Loans**: take loans, interest accrues on every settled game (simple or
  compound), repayments clear interest first then the oldest loan.
- **History**: every settled game with pot, summary, per-player paid-in/hand/net,
  carry-over and interest notes.
- **Undo** the last settled game; double-tap-to-confirm on destructive actions.
- **Currency symbol** (optional) shown on all amounts (Players → Defaults).

## Self-test (offline test harness)

Players tab → **Run 200 random games**. It plays 200 randomized games (random
players, fees, bets, folds, raises, hands, loans) through the same engine the table
uses and checks:

- pot conservation: `entry fees + bets + carry-in = payouts + carry-out`
- each player's balance moves by exactly their net
- the right players win (ties included, no busting winner)
- carry advances correctly between games

Your real ledger is snapshotted and restored afterwards — nothing is kept.
The report appears in the card; you can also open
`Table Ledger.html#selftest` to auto-run it on page load.

## File layout

Everything lives in one file:

```
Table Ledger.html   — the whole app (HTML + CSS + JS), self-contained
README.md           — this file
```

No build step, no dependencies. For GitHub, push both files; if you want GitHub
Pages, rename `Table Ledger.html` to `index.html` on a branch root.

## Data & reset

- Data is stored per-browser in `localStorage` under the key `table-ledger-v2`.
- Players tab → **Reset everything** (tap twice to confirm) clears players, loans,
  history and settings.
- Clearing site data in the browser also clears the ledger — export anything you
  need first (print to PDF from the History tab if you want a copy).
