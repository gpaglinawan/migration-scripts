# Personal portfolio risk check — setup guide

A small Python tool that shows the value, risk and diversification of a personal stock portfolio, using free public price data from Yahoo Finance.

> **Use this on your personal computer, not a work machine.** Educational and self-checking only — **not investment advice**. Past data does not predict future returns, and free data can occasionally be wrong.

---

## 1. One-time setup

1. Install **Python 3** from [python.org](https://www.python.org/downloads/) (on Windows, tick **"Add Python to PATH"** during setup).
2. Open a terminal (Command Prompt / PowerShell / Terminal) and run:

   ```bash
   pip install yfinance pandas
   ```

3. Create a folder for the tool, e.g. `portfolio-risk`.

---

## 2. List your holdings — `holdings.csv`

Create `holdings.csv` in that folder. Use **Yahoo Finance tickers** (e.g. `VOO`, `AAPL`; non-US listings have a suffix such as `.L` for London).

```csv
ticker,shares
AAPL,10
MSFT,5
VOO,8
```

---

## 3. The script — `portfolio_risk.py`

Save this in the same folder:

```python
# Personal portfolio risk check - uses free Yahoo Finance data. Educational only, not investment advice.
import pandas as pd, yfinance as yf

h = pd.read_csv("holdings.csv")
px = yf.download(list(h.ticker), period="2y", auto_adjust=True, progress=False)["Close"]
if isinstance(px, pd.Series): px = px.to_frame(h.ticker[0])
px = px.dropna(how="all").ffill()

last = px.iloc[-1]
value = (h.set_index("ticker").shares * last).dropna()
total = value.sum(); weights = value / total
rets = px.pct_change().dropna()
port = (rets[weights.index] * weights).sum(axis=1)          # daily portfolio return

vol = port.std() * 252 ** 0.5                                # annual volatility
var95 = -port.quantile(0.05) * total                         # 1-day 95% historical VaR
curve = (1 + port).cumprod(); dd = (curve / curve.cummax() - 1).min()   # worst drawdown

print(f"Portfolio value: {total:,.2f}")
print("\nWeights:"); print((weights * 100).round(1).sort_values(ascending=False).astype(str) + " %")
print(f"\nAnnual volatility:          {vol:.1%}")
print(f"1-day loss not exceeded 95% of days (VaR): {var95:,.2f}")
print(f"Worst drawdown (2 years):   {dd:.1%}")
big = weights[weights > 0.25]
if len(big): print(f"\nConcentration warning: {', '.join(big.index)} above 25% of the portfolio")
print("\nCorrelation of daily returns (closer to 1 = moves together, less diversification):")
print(rets[weights.index].corr().round(2))
```

---

## 4. Run it

```bash
cd portfolio-risk
python portfolio_risk.py
```

---

## 5. How to read the results

| Output | What it tells you | Rule of thumb |
|---|---|---|
| **Weights** | How concentrated the portfolio is | One stock above ~25% dominates your risk |
| **Annual volatility** | How much the portfolio typically swings in a year | ~15% is roughly a broad stock index |
| **VaR (95%, 1 day)** | On 19 of 20 days your one-day loss should not exceed this amount — the 20th day it can | Compare with what you could tolerate losing in a day |
| **Worst drawdown** | Biggest fall from a peak over the last 2 years | Ask: could I sit through this again? |
| **Correlation** | Whether holdings move together | Pairs near 1.0 add little diversification |

---

## 6. Ideas for next steps

- **Benchmark:** add `VOO` (S&P 500) to the download and compare volatility and drawdown with your portfolio.
- **Stress test:** multiply each weight by a scenario move (e.g. market −20%, tech −30%) to see the loss in money.
- **Position sizing:** decide a maximum weight per stock and a loss level at which you would reduce a position — before you need it.
- **Track monthly:** save the output (or paste the key numbers into a spreadsheet) once a month to see how risk changes.
- **Options:** if you trade options, check the "Greeks" (delta, theta…) your broker shows; the open-source **QuantLib** library has the pricing maths if you want to go deeper.
- **Backtesting:** test a strategy on past data first with libraries such as `backtrader` or `vectorbt`.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `pip` or `python` not found | Reinstall Python with "Add to PATH" ticked, or use `py` instead of `python` on Windows |
| A ticker shows no data | Check the exact symbol on finance.yahoo.com (suffixes for non-US exchanges) |
| Download errors | Try again later — Yahoo's free service occasionally rate-limits; `pip install -U yfinance` |
