# Projection math (verify before reporting)

## Conventions
- **Contributions at the start of each year (annuity-due).** Each year's
  contribution compounds for the full remaining term.
- Support **flat** and **stepped-up** (contribution grows g% per year) modes.
- Each holding gets its own ₹ amount/year and its own category CAGR.
- Report per-fund and portfolio totals at 10/15/20 (and any slider year 5-40).

## Future value formulas

Flat, contribution C, annual return r, n years (annuity-due):
```
FV_flat = Σ_{i=1..n}  C * (1+r)^(n-i+1)
```

Stepped-up, base contribution C, step g, return r, n years:
```
FV_step = Σ_{t=1..n}  C*(1+g)^(t-1) * (1+r)^(n-t+1)
```

Total invested (own capital):
```
flat:    invested = C * n
step-up: invested = Σ_{t=0..n-1} C*(1+g)^t
```

## Effective return generated (money-weighted / XIRR)
The blended annualised return the whole portfolio earns given staggered
contributions. Solve for r such that the FV of the contribution stream equals the
projected corpus (bisection is robust):
```
find r in [-0.5, 1.0] such that
   Σ_{t=1..n} contribution_t * (1+r)^(n-t+1) = corpus
```
This differs from a simple average of the category CAGRs because money goes in
gradually and categories differ — report it, since it's the honest "what return
am I actually getting" number.

## INR formatting
- Group Indian-style; abbreviate with lakh (₹1,00,000) and crore (₹1,00,00,000).
- Compact helper: ≥1e7 → "₹X.XX Cr"; ≥1e5 → "₹X.XX L"; else grouped rupees.

## Tax note (for switch recommendations)
Equity LTCG is **12.5% on gains above ₹1.25L per year**; short-term (<1 yr) is
higher. Switching funds realises gains — prefer redirecting *future*
contributions and staggering switches across tax years.

## Verification step (do not skip)
Before finalising the report, reproduce the numbers in a quick script and confirm
they match the report's JavaScript. Example (node):
```js
function fvFlat(C,r,n){let s=0;for(let i=1;i<=n;i++)s+=C*Math.pow(1+r,n-i+1);return s;}
function fvStep(C,r,n,g){let s=0;for(let t=1;t<=n;t++)s+=C*Math.pow(1+g,t-1)*Math.pow(1+r,n-t+1);return s;}
// sanity: fvFlat(50000,0.13,20) ≈ 4,573,496
```
Confirm at least one per-fund value and one portfolio total against the rendered
report so the two never drift apart.
