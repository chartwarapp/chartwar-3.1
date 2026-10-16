# ChartWar 3.1

Bull vs Bear market-driven fighting game powered by **METE (Market Event Translation Engine)**,
invented by Long Tran.

**This is the standalone preview** — the full game running 100% in your browser.
No server, no sign-up, no real money.

## Play

Host this directory on any static host (GitHub Pages, Netlify, IPFS) and open `index.html`.
Live at: `https://chartwarapp.github.io/chartwar-3.1/`

- 🐂 **Bull wins** if the price closes **ABOVE** the round open.
- 🐻 **Bear wins** if it closes **BELOW** the round open.
- Exact tie = **draw** (stake refunded minus a small fee).
- Pick a side, set your stake, beat the close. You can **protect** an open
  position mid-round: 75% of stake refunded, 25% fee.
- First visit shows an onboarding card — *Play tutorial round* jumps straight in.

Everything is simulated and clearly labeled **DEMO**: a seeded demo market drives
the fight, and all money is simulation credits (SIM).

## What's inside

- `index.html` + `assets/` — the game (Vite + TypeScript + Canvas 2D, mobile-first).
- `manifest.json` — SFX pack manifest. Sound slots ship **silent by design**;
  drop real MP3 recordings next to it and list them in the manifest to enable audio.
  There are no synthesized fallbacks, ever.
- `.nojekyll` — tells GitHub Pages to serve `assets/` as-is.

## How it's built

In-browser pipeline, all client-side on the framework-free `mete-core` library:

```
seeded demo ticks → TickNormalizer → SignalInterpreter → METEEventTranslator
  → RoundEngine → SettlementEngine → LedgerService → ProtectTradeService
```

Market direction and intensity drive Bull/Bear attacks, guards, dodges, counters,
knockbacks and auras. Settlement is deterministic from the round's authoritative
open/close prices; cosmetic animation variants never touch outcomes. Fees are
conserved exactly (win 3% / loss 3% / draw 5% pool-share model).

**Simulation credits only.** No real-money code paths exist anywhere in this build.
