# Hierarchical Bayesian Market Model — Case Study Walkthrough

A practice notebook exploring how a hierarchical Bayesian model can estimate a
hard-to-observe market quantity (orders per buyer) when data is scarce for some
markets — and what happens when a re-fit breaks.

## Key ideas used throughout

- **Bayesian updating**: combining a prior belief with new data via a
  precision-weighted average (closed-form for a Normal prior + Normal
  likelihood).
- **Log scale**: revenue decomposes into Buyers × Orders per Buyer × Average
  Order Value, a multiplicative identity — so everything is modeled in log
  space, where the product becomes a sum and every quantity stays positive.
- **Partial pooling**: groups with little data borrow strength from their
  peers or from the wider hierarchy, instead of being estimated alone or
  forced to match one global value.
- **Non-centered parameterization**: a reparameterization that removes a
  funnel-shaped dependency in the posterior, so NUTS can sample without
  getting stuck.

## What's inside

**Q1 — Data-poor market walkthrough**
Building a prior for a market with almost no data of its own, using a covariate
bridge (a GDP-based regression, fit by hand with ordinary least squares) and a
peer-group adjustment, then updating it with a single noisy observation.
Compares the result to what would happen with a lot of data instead.

**Q2 — Incorporating a new data source**
Adding new, variable-quality observations for peer markets, updating the
peer-based prior with precision weighting (noisier sources count less), and
tracing the effect through to a downstream revenue estimate.

**Q3 — Debugging a broken re-fit**
Given a scenario where a hierarchical re-fit shows a high R-hat and many
divergences, working through hypotheses in order of likelihood and cost, and
reproducing the classic "funnel" problem — plus its fix, non-centered
parameterization — with a small, runnable example.

**Q4 — Minimal hierarchical model**
A ~10-group hierarchical model with very uneven sample sizes, fit with
PyMC/NUTS, showing partial pooling (shrinkage) in action.

## Tools

- Python, NumPy, pandas, Matplotlib
- [PyMC](https://www.pymc.io/) for MCMC sampling (NUTS)
- [ArviZ](https://python.arviz.org/) for diagnostics (R-hat, divergences)

## Note

All data here is simulated/toy data for illustration. This is a personal
learning exercise, not a production model.
