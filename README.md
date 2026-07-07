# Portfolio MACD Strategy Analysis

A backtest comparing a **MACD crossover strategy** against a **weighted Buy & Hold benchmark**
for a 5-stock portfolio, using [QuantStats](https://github.com/ranaroussi/quantstats) for
performance analytics and [TA-Lib](https://ta-lib.org/) for the MACD indicator.

## Portfolio

| Ticker | Weight |
|--------|--------|
| AAPL   | 20%    |
| NVDA   | 30%    |
| TSLA   | 15%    |
| MSFT   | 20%    |
| META   | 15%    |

## Strategy

For each ticker, a 12/26/9 MACD is computed on daily closing prices. The portfolio is "in
the market" for a given ticker on any day where the MACD line was above the signal line on
the *previous* trading day (avoiding look-ahead bias). Per-ticker strategy returns are then
combined using the portfolio weights above and compared against a matching weighted Buy &
Hold benchmark.

## Project structure

```
.
├── portfolioAnalysis.ipynb   # Main notebook: data download, MACD signal, backtest, tearsheet
├── requirements.txt          # Python dependencies
├── LICENSE
└── README.md
```

## Setup

```bash
git clone https://github.com/<your-username>/portfolio-macd-analysis.git
cd portfolio-macd-analysis
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

> **Note on TA-Lib:** the Python `TA-Lib` wheel wraps a C library that must be installed
> separately on some systems.
> - **macOS:** `brew install ta-lib`
> - **Ubuntu/Debian:** `sudo apt-get install ta-lib` (or build from source if unavailable)
> - **Windows:** use a prebuilt wheel from https://github.com/cgohlke/talib-build/releases
>
> Then `pip install TA-Lib`.

## Usage

```bash
jupyter notebook portfolioAnalysis.ipynb
```

Running the notebook downloads price history via `yfinance`, computes the MACD-based
positions per ticker, and generates an HTML tearsheet (`macd_report_new.html`) comparing
the strategy to Buy & Hold.

## Known limitations

- Portfolio backtest window is limited by the shortest-listed ticker (`dropna()` aligns all
  5 tickers to their common trading history).
- No transaction costs or slippage are modeled.
- Long/flat only — the strategy does not take short positions on bearish crossovers.

## License

MIT — see [LICENSE](LICENSE).
