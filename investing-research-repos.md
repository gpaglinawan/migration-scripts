# Open-source repositories for personal investing research

A short list of **public, open-source** GitHub projects for researching and checking a personal stock portfolio.

> **Use public projects only** — not work/company code (e.g. the fin-tcm repositories), which is confidential and not for personal use.
> Use these on your personal computer. Educational information only — **not investment advice**. Check each project's README, licence and maintenance status before relying on it.

---

## Data and basics

| Repository | What it does | Link |
|---|---|---|
| **yfinance** | Free price and basic market data from Yahoo Finance (used by the portfolio risk script) | https://github.com/ranaroussi/yfinance |
| **pandas** | The standard Python library for tables and time series | https://github.com/pandas-dev/pandas |
| **OpenBB** | Open-source investment research platform: market data, fundamentals, screeners, charts | https://github.com/OpenBB-finance/OpenBB |

## Portfolio and risk analysis

| Repository | What it does | Link |
|---|---|---|
| **QuantStats** | One-line performance and risk reports — Sharpe, drawdowns, VaR, comparison with a benchmark | https://github.com/ranaroussi/quantstats |
| **PyPortfolioOpt** | Portfolio optimisation: mean-variance, risk parity, suggested weights | https://github.com/robertmartin8/PyPortfolioOpt |
| **Riskfolio-Lib** | Advanced portfolio/risk optimisation, e.g. CVaR- or drawdown-based allocations | https://github.com/dcajasn/Riskfolio-Lib |

## Backtesting strategies

| Repository | What it does | Link |
|---|---|---|
| **vectorbt** | Fast backtesting — test many ideas and parameters quickly | https://github.com/polakowo/vectorbt |
| **backtrader** | Classic event-driven backtester, easy to follow | https://github.com/mementum/backtrader |
| **zipline-reloaded** | Maintained version of Quantopian's Zipline engine | https://github.com/stefan-jansen/zipline-reloaded |

## Pricing and quant maths

| Repository | What it does | Link |
|---|---|---|
| **QuantLib** (+ Python bindings) | Open-source library for pricing bonds, options, curves and risk measures | https://github.com/lballabio/QuantLib |

## Learning resources

| Repository | What it is | Link |
|---|---|---|
| **machine-learning-for-trading** | Book-length set of notebooks from factors to machine learning, with working code | https://github.com/stefan-jansen/machine-learning-for-trading |
| **awesome-quant** | Curated list of quant-finance libraries and resources — the best place to discover more | https://github.com/wilsonfreitas/awesome-quant |

---

## Suggested path

1. **Start with your own portfolio:** run the portfolio risk script (`portfolio-risk-guide.md`).
2. **Add a full risk report:** use **QuantStats** to compare your portfolio with the S&P 500 (`VOO`):
   ```python
   import quantstats as qs
   qs.reports.html(port, benchmark="VOO", output="report.html")   # port = daily portfolio returns
   ```
3. **Check your weights:** try **PyPortfolioOpt** to see what a more diversified allocation could look like — as a comparison, not an instruction.
4. **Test before you trade:** backtest any trading rule with **vectorbt** or **backtrader** on past data before using real money.
5. **Go deeper:** browse **awesome-quant** and the **machine-learning-for-trading** notebooks.

Install any of them with `pip`, e.g. `pip install quantstats PyPortfolioOpt vectorbt`.
