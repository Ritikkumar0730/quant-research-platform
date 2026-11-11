# Quant Research Platform

A quantitative research platform covering the full pipeline from raw price data to portfolio construction and execution. Written in Python, tested end to end, with no external data vendor required.

---

## What It Covers

### Portfolio Construction
Four allocation strategies implemented from scratch:
- **Markowitz MVO** — mean-variance optimisation with long-only and weight constraints
- **Black-Litterman** — investor views blended into equilibrium returns
- **Hierarchical Risk Parity (HRP)** — clustering-based allocation that avoids matrix inversion
- **Risk Parity** — equalises risk contribution across assets

Covariance estimation uses Ledoit-Wolf shrinkage and random-matrix-theory denoising to stabilise estimates on short history.

### Alpha Research
A LightGBM model trained on 12 technical and fundamental factors:
- Momentum (12-1 month, 6-1 month, 1-month reversal)
- Volatility (21-day, 63-day, idiosyncratic)
- RSI, skewness, 52-week z-score, market correlation
- Price acceleration, volume trend

Factor quality evaluated with IC, ICIR, and Benjamini-Hochberg FDR correction to guard against overfitting on multiple signals.

### Backtesting
Walk-forward backtest with **combinatorial purged cross-validation (CPCV)** to prevent lookahead bias and reduce backtest overfitting. Performance reported with Deflated Sharpe Ratio and Newey-West HAC standard errors.

### Execution
Almgren-Chriss optimal execution model: minimises market impact cost for a given urgency, with closed-form trajectory and cost estimates.

---

## Results

| Strategy | Sharpe | Max DD | Notes |
|---|---|---|---|
| MVO | 0.81 | -18.4% | Ledoit-Wolf covariance |
| Black-Litterman | 0.94 | -15.2% | Momentum views |
| **HRP** | **1.12** | **-12.8%** | Best risk-adjusted |
| Risk Parity | 1.05 | -13.6% | Equal risk contribution |

---

## Project Structure

```
sae/
├── data/           # Data loading and preprocessing
├── factors/        # Factor computation (12 signals)
├── models/         # LightGBM alpha model
├── portfolio/      # MVO, Black-Litterman, HRP, Risk Parity
├── backtest/       # Walk-forward engine, CPCV, metrics
├── execution/      # Almgren-Chriss execution model
└── reporting/      # Performance attribution, charts
tests/              # 46 unit tests
```

---

## Quickstart

```bash
git clone https://github.com/Ritikkumar0730/quant-research-platform
cd quant-research-platform
pip install -r requirements.txt
python scripts/run_full_pipeline.py
```

### Tests

```bash
pytest tests/ -v
```

---

## Dependencies

- `numpy`, `scipy`, `pandas` — numerical core
- `scikit-learn` — preprocessing, cross-validation
- `lightgbm` — alpha model
- `matplotlib` — reporting charts
- `statsmodels` — Newey-West HAC
