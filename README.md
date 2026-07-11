# Wealth Projection Report

A Claude Skill that turns a list of mutual fund/SIP holdings and a time
horizon into an interactive, single-file HTML report projecting corpus
growth toward a financial goal — in Indian Rupees, grounded in current
Indian market conditions.

## What it does
- Projects corpus growth across conservative / base / optimistic scenarios
  rather than a single false-precise number
- Tempers trailing fund returns instead of straight-lining a bull-market CAGR,
  anchoring realistic bands to nominal GDP growth
- Compares funds on risk-adjusted terms (Sharpe, drawdown, consistency), not
  raw returns
- Searches current data (fund AUM, expense ratios, index valuations, RBI/GDP
  figures) rather than relying on stale memory, and cites sources
- Verifies all projection math programmatically before it goes in the report
- Always disclaims — SEBI market-risk language, never personalised advice

## Structure
- `SKILL.md` — the skill definition and workflow
- `references/` — projection math, fund due-diligence criteria, scenario/macro
  assumptions, ROI factors
- `assets/report_template.html` — the interactive report shell

## Status
Personal tool for portfolio due-diligence and goal planning (India/INR
focused: SEBI norms, LTCG tax, RBI macro conventions).
