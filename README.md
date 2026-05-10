# Simple FIRE Dashboard

A single-file, portable retirement planning calculator for the FIRE (Financial Independence, Retire Early) community. No server, no dependencies to install — just open the HTML file in any modern browser.

## Overview

This dashboard helps you model your path to financial independence with configurable inputs, multiple visualization tabs, and Monte Carlo simulation. It's styled after Fidelity Investments' clean, professional design language.

## Features

### Inputs (Sidebar)

- **Personal:** Current age, retirement age (or "As soon as possible" mode)
- **Income & Spending:** Gross income, annual spending
- **Portfolio:** Total value, with optional breakdown by tax bucket (tax-deferred, tax-free, after-tax)
- **Contributions:** Total annual contributions, with optional breakdown by tax bucket (pre-tax, tax-free, after-tax) — defaults to common 2026 IRS limits when expanded
- **Assumptions:** Nominal return, inflation, withdrawal rate (SWR), spending growth, effective retirement tax rate
- **Simulation:** Number of Monte Carlo runs, volatility, retirement duration

All dollar inputs display with comma formatting and support double-click to select.

### Dashboard Tabs

1. **Overview** — FIRE milestone cards (Coast FIRE 65, Coast FIRE 55, Barista FIRE, Full FIRE) with plain-English explanations, color-coded progress (green/yellow/red), predicted achievement dates, and full FIRE details. When "As soon as possible" is selected, shows earliest possible retirement age and a spending-adjustment grid.

2. **Growth Projection** — Stacked area chart showing principal vs. investment growth over time, with FIRE number and Barista FIRE threshold lines. Includes a summary highlighting what percentage of the final portfolio is growth vs. contributed principal.

3. **Tax Buckets** — Projected balance in each tax bucket at retirement age, growth chart by bucket over time, and retirement withdrawal tax impact analysis (estimated annual tax, effective rate, tax-free withdrawal share). Requires portfolio or contributions to be set to "By Tax Bucket" mode.

4. **Simulation** — Monte Carlo and Historical Backtest modes with a starting-portfolio selector (choose portfolio at retirement age, ±5 years, at FIRE, or current). Shows success rate ring, percentile band chart (10th–90th), and sample paths.

5. **Withdrawal Strategy** — Drawdown curves at 3–6% withdrawal rates, and a bar chart of withdrawal rate vs. Monte Carlo success probability (1,000 simulations each).

6. **Savings Rate** — Bar chart of savings rate vs. years to FIRE, plus a quick-reference grid with your current rate highlighted.

7. **Year-by-Year** — Full data table with portfolio, contributions, spending, FIRE number, progress percentage, and status for each year.

### Other Features

- **Save Locally** — Persists all inputs to `localStorage` so your data survives closing and reopening the file
- **Share Link** — Copies a URL with all parameters encoded for sharing your scenario
- **Print / Export** — Print-optimized CSS for PDF export
- **Reset to Defaults** — Clears saved data and restores default values
- **Responsive** — Sidebar collapses on narrow screens
- **Auto-recalculate** — Dashboard updates on any input change

## Quick Start

1. Open `fire_dashboard.html` in any modern browser (Chrome, Firefox, Safari, Edge)
2. Adjust the inputs in the sidebar to match your financial situation
3. Explore each tab to understand your path to FIRE
4. Click **Save Locally** to persist your inputs across sessions

## Technical Details

- **Single file:** All HTML, CSS, and JavaScript in one portable `.html` file
- **External dependency:** Chart.js 4.4.1 (loaded from cdnjs CDN — requires internet on first load, then cached by browser)
- **Storage:** `localStorage` for persistence (no cookies, no server)
- **No build step:** No npm, no bundler, no compilation — just a file

## Calculations

- **FIRE Number:** Annual spending (inflation-adjusted) ÷ withdrawal rate
- **Coast FIRE:** The portfolio value needed today such that compound growth alone reaches your FIRE number by a target age, with no further contributions
- **Barista FIRE:** 50% of FIRE number — portfolio covers half your expenses, part-time work covers the rest
- **Savings Rate:** Annual contributions ÷ gross income
- **Real Return:** Nominal return − inflation
- **Monte Carlo:** Randomized annual returns drawn from a normal distribution (configurable mean and volatility) applied over the retirement duration
- **Historical Backtest:** Regime-switching model approximating bull/bear/crisis/recovery market cycles

## Disclaimer

These projections are hypothetical and do not reflect actual investment results. They are not guarantees of future results. All values are shown in today's dollars unless otherwise noted. Consult a qualified financial advisor and tax professional before making investment decisions.

## License

Free to use, modify, and distribute.
