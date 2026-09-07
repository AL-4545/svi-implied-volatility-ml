# Fixed-Expiry SVI Implied Volatility Calibration

This project fits SVI volatility smiles to SPX options data for one fixed expiration observed across multiple dates. The goal is to build a smooth and temporally stable representation of the market-implied volatility smile.

## Motivation

Traders need a smooth implied volatility curve to price less liquid options, compare relative value across strikes, and compute risk/hedging quantities. Individual option quotes can be noisy, sparse, or inconsistent, so this project uses SVI as a parametric model for fitting a stable volatility smile.

## Data

- Underlying: SPX options
- Source: Bloomberg
- Expiration: December 18, 2026
- Observation dates: 103
- Final observations: 22,117 OTM option quotes

The raw Bloomberg data are not included in this public repository.

## Methodology

The pipeline is:

1. Filter to out-of-the-money puts and calls.
2. Convert strikes to log-forward moneyness.
3. Convert implied volatility to total implied variance.
4. Fit raw SVI curves independently by date using multi-start nonlinear optimization.
5. Reparameterize the model by minimum total variance to enforce positive variance.
6. Add wing-slope constraints to prevent unstable extrapolation.
7. Use a weighted fitting loss to reduce domination by high-variance far-wing observations.
8. Add temporal ridge regularization to stabilize parameter paths across adjacent dates.

## Main results

The final model fit all 103 daily smiles successfully, with no arbitrary optimizer bound hits. Temporal regularization reduced RMS parameter jumps by about 79% while increasing pooled implied-volatility RMSE by less than 0.5% relative to the independently fitted baseline.

## Repository structure

```text
notebooks/
  svi_fixed_expiry_calibration.ipynb

data/
  README.md

outputs/
  figures/
  tables/