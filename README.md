# Moving Average Crossover Backtest Dashboard

A lightweight, interactive Jupyter dashboard for backtesting simple SMA/EMA crossover strategies on any ticker using daily OHLCV data from Yahoo Finance. Built with ipywidgets, pandas, NumPy, yfinance, seaborn, and Matplotlib.

---

## Features

- SMA or EMA crossover selection with adjustable windows (short/long).  
- Symbol, date range, initial capital, allocation, commission, and slippage inputs via a clean widget panel.  
- Always-in long/short signal engine with position sizing based on alloc% of capital per side.  
- Clear outputs: equity curve, drawdown, buy/sell markers, performance metrics.  
- Prints initial capital and final returned value (final equity) for quick assessment.  

---

## What’s included

- Interactive control panel (symbol, dates, MA kind, windows, capital, alloc%, costs).  
- Indicator engine (SMA via rolling mean, EMA via exponentially weighted mean).  
- Backtester with simple slippage and commission modeling.  
- Aesthetic charts: price with MAs and signals, equity curve, and drawdown.  
- Safety checks and helpful error messages in the widget callback.

---

## Requirements

- Python 3.9+  
- Jupyter Notebook or JupyterLab  
- Packages:
  - numpy
  - pandas
  - yfinance
  - matplotlib
  - seaborn
  - ipywidgets

Install:
pip install numpy pandas yfinance matplotlib seaborn ipywidgets

text

Enable widgets (classic Jupyter):
jupyter nbextension enable --py widgetsnbextension

text

---

## Quick start

1. Launch a Jupyter notebook.  
2. Paste the dashboard cell into a new notebook cell.  
3. Run the cell to render the widget panel.  
4. Enter a ticker (e.g., AAPL), pick Start/End dates, choose SMA or EMA, adjust windows/costs, and Run Backtest.  

---

## Dashboard controls

- Symbol: Ticker string (e.g., AAPL, MSFT, TCS.NS).  
- Start / End: Date range for backtest (daily intervals).  
- MA Kind: SMA or EMA crossover.  
- Short MA / Long MA: Window lengths (e.g., 50/200).  
- Capital: Starting cash (used to compute position sizing).  
- Alloc %: Fraction of capital deployed per side; position size = floor((capital × alloc%) / Close).  
- Comm %: Commission rate applied to traded notional.  
- Slip %: Slippage applied to trade fill via a simple multiplicative factor on price.

---

## Outputs
<img width="1091" height="216" alt="image" src="https://github.com/user-attachments/assets/322f17ec-051d-433c-96b1-90494b2eaab1" />
<img width="864" height="316" alt="image" src="https://github.com/user-attachments/assets/3e4bace7-998b-45b3-9a25-b20e49c1648c" />
<img width="863" height="544" alt="image" src="https://github.com/user-attachments/assets/8c3c8818-98fb-491f-8a3f-55bec918626c" />

- Performance Metrics:
  - Annual Return, Annual Volatility, Sharpe Ratio, Max Drawdown, Win Rate.  
- Equity Curve:
  - Portfolio equity vs. time and a baseline initial capital line.  
- Drawdown:
  - Peak-to-trough drawdown (%) over time.  
- Signals:
  - Buy (up) and Sell (down) markers shown at regime flips.  
- Summary Prints:
  - Portfolio Initial and Portfolio Return (Final Equity).

---

## Strategy logic

- Signals (always-in):  
  - Long when short MA > long MA; short otherwise.  
  - SMA uses `rolling(window).mean()`.  
  - EMA uses `ewm(span, adjust=False).mean()`.  
- Position sizing:  
  - Shares = floor((initial_capital × allocation_pct) / Close).  
- Trading costs:  
  - Commission = commission% × trade notional; Slippage adjusts fill price by sign of trade.

---

## Example snippet (printing initial and final equity)

Within the run handler after the backtest:
final_value = float(portfolio['total'].iloc[-1])
print(f"Portfolio Initial: ${initial_capital:,.2f}")
print(f"Portfolio Return (Final Equity): ${final_value:,.2f}")

text

---

## Troubleshooting

- “No data fetched”: Verify symbol and date range, ensure internet access, try a different ticker or shorter range.  
- “DatetimeIndex date error”: Use `data.index[0].date()` and `data.index[-1].date()`; avoid calling `data.index.date()` as a function.  
- “ndarray not callable”: Replace any accidental array calls with proper indexing (use `[]` not `()`).  
- Flat lines or no signals: Ensure long window > short window and sufficient history exists for the selected windows.

---

## Notes and tips

- The notebook uses unadjusted prices (`auto_adjust=False`). If adjusted prices are preferred, set `auto_adjust=True` in the data loader.  
- For consistent visuals, seaborn styles and Matplotlib settings are applied at the top of the cell.  
- This is a simple reference implementation; real-world execution, risk, and accounting may differ significantly.

---

## License

MIT (or preferred license).

---

## Acknowledgments

- Jupyter, ipywidgets, pandas, NumPy, yfinance, seaborn, Matplotlib communities.  
