# Financial Sector Stock Market Analytics: Real-Time EDA, Risk & Return Insights using Python and Power BI

Real-Time Exploratory Data Analysis of Indian Banking, NBFC and Insurance stocks using Python and Power BI.

## Description

Financial markets generate continuous data on stock prices, trading volumes, and volatility. Investors and analysts need a repeatable way to compare **live price trends, returns, risk, and correlation** across Indian **Banking, NBFC, and Insurance** stocks instead of judging names by rupee Close alone.

This project is an end-to-end analytics pipeline, implemented in the notebook `update_stage_4.ipynb`:

1. **Stage 1** - Pull a rolling 5-year daily OHLCV history from Yahoo Finance (`yfinance`) for **15 NSE-listed** financial-sector companies, save a snapshot, and run initial EDA.
2. **Stage 2** - Clean the table (types, duplicates, invalid OHLC, missing values), then engineer `Daily_Return_Pct`, `Price_Range`, calendar fields, and `Rolling_Volatility_30d`.
3. **Stage 3** - Rank stocks by **risk-adjusted score** (average daily return % ÷ volatility), classify **SMA 50 / SMA 200** trend, and visualize price, volume, risk, return, and correlation.
4. **Stage 4** - Document the findings, paste three **Power BI** dashboards (Price & Volume, Returns & Risk, Scorecard), and give recommendations.

On the latest notebook run the cleaned file has **18,441 rows and 18 columns** covering **20 Sep 2021 – 09 Sep 2026**. **Bank of Baroda** ranks #1 on risk-adjusted score (**0.059**) but is in a **Downtrend**; **PNB** ranks #2 (**0.053**) and is in an **Uptrend**.

GitHub repository: [veenacareer26/MAIN_PROJECT-Financial-Sector-Stock-Market-Analytics](https://github.com/veenacareer26/MAIN_PROJECT-Financial-Sector-Stock-Market-Analytics)

## Business problem

Retail investors and analysts need a repeatable way to compare live price trends, returns, volatility, and correlation across Indian Banking, NBFC, and Insurance stocks.

**Objectives**

1. Gather, clean, transform, and preprocess historical stock data for selected banking, NBFC, and insurance companies.
2. Run EDA in Python on price movements, trading volume, returns, volatility, and market fluctuations over about five years.
3. Compare risk, return, and correlation across segments, and identify top-performing names and diversification limits.
4. Build Power BI dashboards and write actionable recommendations for stakeholders.

## Dataset

- **Source:** [Yahoo Finance](https://finance.yahoo.com/) via the `yfinance` Python package (free, no API key). Every fetch uses `period="5y"` and `interval="1d"`.
- **Reproducible snapshots on GitHub:**
  - `financial_sector_stock_data.csv` — raw combined OHLCV
  - `financial_sector_stock_data_cleaned.csv` — cleaned + engineered features
  - `statistical_summary.csv` — Rank, return, volatility, risk-adjusted score, SMA trend
- **Universe (15 NSE tickers):**
  - **Banks:** HDFC Bank, ICICI Bank, State Bank of India, Kotak Mahindra Bank, Axis Bank, IndusInd Bank, Punjab National Bank, Bank of Baroda, IDFC First Bank
  - **NBFCs:** Bajaj Finance, Bajaj Finserv
  - **Insurance:** HDFC Life, SBI Life, ICICI Prudential Life, ICICI Lombard
- **Original columns:** Date, Open, High, Low, Close, Volume, Ticker, Company_Name, Segment, Exchange, Currency
- **Engineered columns:** Daily_Return_Pct, Price_Range, Year, Month, Month_Name, Weekday, Rolling_Volatility_30d, SMA_50, SMA_200, Risk_Adjusted_Score, Trend

## Getting Started

### Dependencies

- Windows 10/11 (or any OS that can run Python 3.9+ and Jupyter / Google Colab)
- Python 3.9+
- `yfinance`, `pandas`, `numpy`, `matplotlib`, `seaborn`
- `PyGithub` only if you push CSVs from Colab to GitHub
- Internet access for a live Yahoo Finance pull (the GitHub CSVs are the offline fallback)
- Power BI Desktop (optional) to open the three-page `.pbix` built from the cleaned CSV and statistical summary

### Installing

```bash
git clone https://github.com/veenacareer26/MAIN_PROJECT-Financial-Sector-Stock-Market-Analytics.git
cd MAIN_PROJECT-Financial-Sector-Stock-Market-Analytics
pip install yfinance pandas numpy matplotlib seaborn
```

For Colab, the notebook already installs packages in the first cells (`yfinance`, `PyGithub` when needed).

### Executing program

1. Open `VEENA_MAIN PROJECT.ipynb` in Google Colab or Jupyter.
2. Run all cells top to bottom.
3. Stage 1 fetches live 5-year daily data for the 15 tickers, saves `financial_sector_stock_data.csv`, and can reload the GitHub snapshot if the fetch is empty.
4. Stage 2 cleans the table and writes `financial_sector_stock_data_cleaned.csv`.
5. Stage 3 builds `statistical_summary.csv` (risk-adjusted ranking + SMA trend) and the eight Seaborn/Matplotlib charts, each followed by an interpretation cell.
6. Stage 4 documents the findings and shows the three Power BI dashboard screenshots (Price & Volume, Returns & Risk, Scorecard).

```
# Typical Colab / notebook flow (already in the cells)
# 1. Fetch
df = fetch_live_data(TICKERS)   # period="5y", interval="1d"
# 2. Clean + features
df["Daily_Return_Pct"] = df.groupby("Ticker")["Close"].pct_change() * 100
# 3. Rank
Risk_Adjusted_Score = Avg_Daily_Return_Pct / Volatility
# 4. Trend
# Uptrend if Close > SMA_50 and Close > SMA_200
```

## Help

- If `yfinance` returns empty data, check the network or retry — Yahoo Finance sometimes rate-limits repeated calls. The notebook then loads the last snapshot / GitHub CSV.
- NSE tickers must use the `.NS` suffix (for example `HDFCBANK.NS`). Without it, `yfinance` looks up the US market.
- Matplotlib cannot label a timezone-aware Date axis cleanly; the closing-price chart converts `Date` to a naive calendar date before plotting.
- The first row per ticker has no prior close, so `Daily_Return_Pct` is NaN there; the first few rows also have NaN `Rolling_Volatility_30d` until `min_periods=5`. Stage 2 drops those residual NaNs.
- Do not commit a GitHub personal access token inside the notebook. Use a Colab Secret named `GITHUB_TOKEN`.

## Authors

Veena S

GitHub: [veenacareer26/MAIN_PROJECT-Financial-Sector-Stock-Market-Analytics](https://github.com/veenacareer26/MAIN_PROJECT-Financial-Sector-Stock-Market-Analytics)

## License

This project is submitted for the Data Analytics Final Project. Dataset values come from Yahoo Finance and remain subject to Yahoo’s terms of use.

## Acknowledgments

- [yfinance](https://github.com/ranaroussi/yfinance)
- [Yahoo Finance](https://finance.yahoo.com/)
- Data Analytics Final Project Guidelines
