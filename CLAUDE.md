# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-file FIRE (Financial Independence, Retire Early) retirement planning calculator. Everything — HTML, CSS, and JavaScript — lives in one file: `index.html`. There is no build step, no npm, no bundler, and no test suite. Open the file directly in a browser to run it.

The only external dependency is Chart.js 4.4.1, loaded from a CDN. All state is held in DOM inputs and `localStorage`.

## Development

To work on this project, open `index.html` in a browser. Changes take effect on reload. There are no commands to run.

## Architecture

### Data flow

```
sidebar inputs → getInputs() → calc functions → render (r*) functions → DOM / Chart.js
```

`recalc()` is the master orchestrator — it calls `getInputs()` and then every render function. It fires on any input change.

### Key functions

**Inputs**
- `getInputs()` — reads all sidebar inputs, resolves ASAP retirement age, returns a flat `inp` object used by every calc and render function
- `V(id)` — reads a numeric value from an input element (handles money formatting)

**Calculations** (pure functions, return data — no DOM side effects)
- `findFire(inp)` / `findFireRaw(inp)` — iterates year-by-year to find when portfolio reaches the FIRE number
- `findFireAtSpend(inp, annualSpend)` — like `findFire` but with a custom spending level (used for the spending-adjustment grid)
- `calcProj(inp)` — returns a year-by-year array of `{year, age, bal, principal, growth, fireNum, barista, ...}` rows
- `calcCoast(inp, targetAge)` — calculates Coast FIRE needed and status for a given coast-to age
- `calcBarista(inp)` — finds when portfolio reaches 50% of FIRE number
- `calcBuckets(inp)` — projects tax-deferred / tax-free / after-tax buckets separately through retirement age
- `runMC(inp, startBal, annualWD)` — Monte Carlo simulation using Box-Muller normal random draws
- `runHist(inp, startBal, annualWD)` — regime-switching historical backtest (bull/bear/crisis/recovery)
- `calcWD(inp, wr)` — deterministic drawdown curve for a single withdrawal rate
- `calcWRSuccess(inp)` — runs 1,000 MC simulations per withdrawal rate (2–8%) for the success-rate bar chart
- `calcSR(inp)` — savings rate vs. years-to-FIRE table

**Render functions** (all prefixed `r`, read from DOM, write to DOM/charts)
- `rKPI(i)` — top KPI cards
- `rEarliest(i)` — ASAP card with spending-adjustment grid
- `rOverview(i)` — FIRE milestone cards (Coast 65, Coast 55, Barista, Full FIRE)
- `rGrowth(i)` — stacked area growth projection chart
- `rBuckets(i)` — tax bucket charts and withdrawal tax analysis
- `renderMC()` — Monte Carlo / Historical Backtest tab (reads `mcMode` global)
- `rWD(i)` — withdrawal strategy drawdown curves and success-rate bar chart
- `rSens(i)` — savings rate chart and reference grid
- `rTable(i)` — year-by-year data table

### Global state
- `charts` — object holding Chart.js instances by key; each render function calls `.destroy()` before recreating
- `contribMode` / `portfolioMode` — `'simple'` or `'detail'` (controls whether tax-bucket sub-fields are shown)
- `mcMode` — `'mc'` or `'hist'` (Monte Carlo vs. Historical Backtest toggle)
- `CY` — current calendar year constant

### Utility functions
- `fmt(n)` — compact dollar format (`$1.23M`, `$456k`, `$789`)
- `fmtF(n)` — full dollar format with commas
- `pct(n)` — percentage string
- `nRand(mean, stddev)` — Box-Muller normal random number generator
- `wRand(weights)` — weighted random index selector (used for regime switching)
- `cOpts()` — shared Chart.js options (grid colors, font, tooltip style)

### CSS conventions
Short class names are used throughout for compactness (`.ig`, `.fld`, `.kpi`, `.mile`, `.pnl`, `.tab`). Color tokens are defined as CSS custom properties on `:root` (e.g., `--green`, `--red`, `--gold`).
