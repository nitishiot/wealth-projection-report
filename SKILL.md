---
name: wealth-projection-report
description: >-
  Analyse an investment portfolio (mutual funds, SIPs, stocks) and produce a
  detailed, interactive single-file HTML report projecting how it grows toward
  the user's financial goals over 10/15/20+ years, in Indian Rupees. Use this
  skill whenever the user wants to know how much their investments/SIPs will be
  worth over time, project a retirement or goal corpus, analyse or compare
  mutual funds, decide whether to keep/replace a fund, check a fund for red
  flags (manager change, AUM bloat, concentration), stress-test returns against
  macro conditions (GDP, inflation, valuations), or asks for an "investment
  report", "portfolio projection", "SIP calculator", "goal planning", "which
  fund is better", or "how much return will I get". Trigger even when the user
  only names funds and an amount without explicitly saying "report" — the
  default deliverable is the interactive HTML report. India/INR focused (SEBI
  norms, LTCG tax, RBI macro, Nifty valuations, fund-category conventions).
---

# Wealth Projection Report

## What this skill does

Turn a list of holdings + contributions + a time horizon into an honest,
decision-useful, interactive HTML report. The report projects corpus growth
across conservative/base/optimistic scenarios, shows the effective return
generated, adjusts for current macro conditions, and supports fund
due-diligence and replacement analysis. Everything is in Indian Rupees.

The goal is not a single false-precise number. It is to give the user a
defensible *range*, expose the assumptions, and let them change any input and
watch the whole model recalculate.

## Core principles (read before building anything)

1. **Currency is always ₹ (Indian Rupees).** Format large numbers the Indian
   way — lakh (₹1,00,000) and crore (₹1,00,00,000). Never use $ or generic
   number grouping.

2. **Search for current data before any factual claim.** Fund returns, AUM,
   fund managers, expense ratios, index valuations, GDP/inflation/rates all
   change. Never rely on memory — search (Groww, Value Research, Paytm Money,
   INDmoney, Advisorkhoj, Morningstar, AMC sites, RBI, index PE sites) and cite.

3. **Temper history — never project trailing returns straight-line.** A fund's
   20-25% trailing 5-yr CAGR reflects a specific bull run and cannot persist for
   20 years. Set base/optimistic bands *below* trailing history and anchor them
   to nominal GDP (~11%). See `references/scenario_and_macro.md`.

4. **Risk-adjusted, not raw.** "Better return at the same risk" is nearly a
   free lunch and rarely exists. A fund with higher returns but higher
   volatility is not better. Compare on Sharpe / drawdown / consistency, and
   only call something an upgrade if it improves risk-adjusted return or fixes a
   structural flaw. See `references/fund_due_diligence.md`.

5. **Verify the math programmatically.** Before putting numbers in the report,
   reproduce the future-value and XIRR calculations in a quick script (Python or
   node) and confirm they match the report's JS. See
   `references/projection_math.md`.

6. **Always disclaim, never advise.** End every deliverable with: mutual funds
   are subject to market risk; past performance does not guarantee future
   returns (SEBI-mandated); this is illustrative analysis, not personalised
   investment/tax advice; consult a SEBI-registered adviser. Present opposing
   considerations honestly rather than cheerleading a pick.

## Workflow

Work through these steps. Skip or compress steps the user has already answered,
but do not skip research (step 2) or verification (step 5).

### 1. Clarify the brief

Confirm, asking only what's genuinely missing: the holdings (fund names /
tickers), the contribution per holding and cadence (e.g. ₹50,000/fund/year),
whether contributions are flat or stepped-up, the horizon(s), and the goal
(retirement corpus, house, child, general wealth). Default deliverable is the
interactive HTML report unless they ask otherwise.

If the user's first ask is "list the factors that predict ROI", open with the
factor checklist in `references/roi_factors.md` before any numbers — it sets
honest expectations that history alone can't predict long-run ROI.

### 2. Research current data (search — do not use memory)

For each holding gather: trailing returns (3/5-yr CAGR, since inception), AUM,
fund manager + tenure, expense ratio, category, and any recent news (manager
exits, style drift). For the macro layer gather: India real GDP growth, CPI
inflation, RBI repo rate, and index valuations (Nifty 50 / Midcap 150 /
Smallcap 250 P/E vs their historical medians). Cite every source.

### 3. Fund due-diligence & red-flag scan

For each fund assess manager quality/tenure, style (concentrated/momentum vs
diversified), AUM vs category capacity, and risk-adjusted metrics. Flag the red
flags that actually matter over a long horizon — recent manager departure,
AUM bloat (especially small-caps), extreme concentration, style drift, and
key-person concentration across multiple of the user's funds. Full framework
and replacement logic in `references/fund_due_diligence.md`.

### 4. Set scenario assumptions

Assign conservative/base/optimistic annual CAGR bands *per category*, tempered
below trailing history. Then build a macro-adjusted layer that reflects today's
valuations (rich small/mid caps → front-loaded de-rating drag) against the
~11% nominal-GDP anchor. Method and default bands in
`references/scenario_and_macro.md`.

### 5. Compute and verify projections

Use annuity-due future value (contributions at the start of each year), support
both flat and stepped-up contributions, and compute the effective annualised
return (money-weighted / XIRR) that the whole portfolio generates. Reproduce
the numbers in a script and confirm they match before building the report.
Formulas in `references/projection_math.md`.

### 6. Build the interactive HTML report

Copy `assets/report_template.html` and populate the clearly-marked data block
(`FUNDS`, scenario bands, `MACRO`, historical-data rows). The template is a
self-contained single file (Chart.js from CDN) with: an editable assumptions
panel, a year slider (5-40), KPI cards showing corpus **and** the effective
CAGR generated per scenario, a growth chart, a scenario chart, a portfolio
table, per-fund tables, a macro-adjusted section, a historical-data table, and
the disclaimer + sources. Everything recalculates live from the inputs.
Save the final report to the user's working folder.

Add optional comparison sections when the analysis calls for them (e.g.
current-vs-proposed portfolio, or keep-vs-replace a fund) — the template shows
the pattern.

### 7. Deliver

Present the HTML file with the file-sharing tool. Give a short summary: the
headline range (conservative-base-optimistic at the key horizon), the single
most important insight, and the honest caveats. Then offer sensible next steps
(step-up version, tax/inflation layer, turmoil stress test, a fund swap, or a
periodic review reminder). Keep the prose tight; let the report carry the
detail.

## Reference files

- `references/roi_factors.md` — the full checklist of factors that drive/predict
  long-run ROI. Use for the "what predicts returns" framing and to keep the
  analysis honest about history's limits.
- `references/fund_due_diligence.md` — how to vet a fund (manager, style, AUM,
  risk metrics), the red-flag checklist, and keep/replace/upgrade logic.
- `references/scenario_and_macro.md` — default category CAGR bands, the
  temper-below-history principle, and the macro-adjustment method.
- `references/projection_math.md` — exact FV/step-up/XIRR formulas, INR
  formatting, LTCG note, and the verification step.

## Non-negotiables

Currency ₹ always. Search + cite current data. Temper history. Compare on
risk-adjusted terms. Verify math. Disclaim and never give personalised advice.
