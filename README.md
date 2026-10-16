# Extreme Events and Stock Market Behavior: Why Crashes Differ, 2000–2026

DATA 400 individual mini-project · J (julesdaovuanh) · Dickinson College · Fall 2026

## The project

**Research question.** How do different types of extreme events move the U.S. stock market, why do some crashes recover quickly while others take years, and what does this mean for protecting a portfolio?

The project identifies 17 extreme episodes in the S&P 500 from 2000 to 2026 with rule-based volatility filters. Each crisis is classified by the economic channel through which it hit prices: growth and credit, rates and inflation, valuation unwind, or sudden event. The project then studies how stocks, Treasuries, gold, oil, sectors and five portfolios behaved, and stress-tests those portfolios against historical and hypothetical scenarios.

**Main findings**

- **Recovery time depends on the type of shock.** Sudden events recovered in a median of 10 trading days. Growth and credit crises took 77, rate and inflation shocks 115, and valuation unwinds about 900.
- **Treasuries hedged growth shocks but not inflation shocks.** They protected portfolios when growth collapsed and fell alongside stocks when inflation and rising rates caused the crash, as in 2022.
- **Balanced portfolios were protected mostly by holding fewer stocks.** Very little of the protection came from hedging.
- **Knowing the shock type did not predict losses better than a generic crash scenario.**

The full argument is in the written report.

## Repository contents

| Path | What it is |
|---|---|
| `report/Extreme_Events_Final_Report.pdf` | Final written report (research question, disciplinary motivation, data, analysis, findings, stakeholders, limitations, conclusion) |
| `report/Extreme_Events_Final_Report.docx` | Same report in Word format |
| `report/figures/` | The three figures used in the report, exported from the notebook |
| `extreme_events_full.ipynb` | All code: data download and preparation, exploration, analysis and stress tests. It is saved with the outputs of the final run, so every result can be read without re-running. |
| `presentation/` | Slides from the in-class mini-project presentation (see the note in that folder) |
| `requirements.txt` | Python packages |
| `.gitignore` | Keeps raw market data out of the repository (see *Data*) |

When it runs, the notebook creates three folders next to itself:

- `data/raw/`: the downloaded inputs (`market_prices.csv`, `macro_fred.csv`, `data_metadata.json`), frozen after the first run.
- `outputs/`: about 30 result tables (CSV) plus `config.json` with every parameter.
- `figures/`: nine figures (PNG).

## Data

All data are public, and the notebook downloads them automatically. Raw price files are **not** included in this repository because Yahoo Finance's terms do not permit redistribution, and some FRED series (e.g., Moody's yields) are copyrighted by their providers.

| Data | Source | How it is obtained |
|---|---|---|
| S&P 500 index, VIX, 10-year and 3-month Treasury yields (`^GSPC`, `^VIX`, `^TNX`, `^IRX`) | Yahoo Finance | `yfinance` |
| Total-return funds: S&P 500 (VFINX), long Treasury (VUSTX), intermediate Treasury (VFITX) | Yahoo Finance | `yfinance` |
| Gold and crude oil futures (`GC=F`, `CL=F`), U.S. dollar index (`DX-Y.NYB`) | Yahoo Finance | `yfinance` |
| Nine Select Sector SPDR ETFs (XLK, XLF, XLE, XLV, XLY, XLP, XLI, XLB, XLU) | Yahoo Finance | `yfinance` |
| Baa–10-year credit spread (BAA10Y), CPI (CPIAUCSL), 10-year and 3-month Treasury yields (DGS10, DTB3) | Federal Reserve Bank of St. Louis, [FRED](https://fred.stlouisfed.org/) | Public CSV endpoint, no API key needed |

The sample is fixed in the configuration cell. The download runs from 1999-01-01, with the extra year used as a warm-up for the volatility estimates, to an exclusive end date of 2026-10-16. The last trading day is therefore 2026-10-15, and the analysis covers 6,737 trading days from January 2000. The report's results come from data downloaded on October 16, 2026, using `yfinance` 0.2.59.

Yahoo Finance sometimes revises historical prices. A fresh download can therefore differ slightly in the last decimal place, but the conclusions do not depend on this. The yield data are checked against the official FRED series inside the notebook (median gap 0.01 percentage points).

No passwords, API keys or private data are used.

## Software

- Python 3.10 or later (the final run used Python 3.12.4)
- pandas 2.2, numpy 1.26, matplotlib 3.8, yfinance 0.2.59 (exact versions are in `requirements.txt`)
- Jupyter Notebook, JupyterLab, VS Code or Google Colab

The notebook needs an internet connection only for the first run, to download the data.

## How to run the analysis

**Option A: Google Colab**

1. Open [colab.research.google.com](https://colab.research.google.com), then choose *File → Upload notebook* and select `extreme_events_full.ipynb`.
2. Choose *Runtime → Run all*. The first cell installs `yfinance`; if Colab asks to restart, restart and run all cells again from the second cell.
3. Results appear inside the notebook. The `data/`, `outputs/` and `figures/` folders appear in the Colab file panel on the left.

**Option B: on your own computer**

```bash
git clone https://github.com/julesdaovuanh/data400-mini-project.git
cd data400-mini-project
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook extreme_events_full.ipynb
```

Then choose *Kernel → Restart & Run All*. A full run takes several minutes, mostly for the permutation and leave-one-out tests.

**Notes**

- The first run downloads and freezes the data in `data/raw/`. Later runs read the frozen files, so results do not change between runs. To download again, set `"refresh_data": True` in the configuration cell.
- All settings are in the configuration cell (Section 1) and are saved to `outputs/config.json`: thresholds, windows, portfolio weights and scenario definitions.
- Section 22 of the notebook prints the headline findings directly from the data. They match the numbers in the report.

## How the notebook is organised

| Part | Sections | What it does |
|---|---|---|
| Framework | 0 | Why stock prices fall and recover: cash flows, the risk-free rate and the risk premium |
| Setup | 1–2 | Configuration, data download and freeze, data checks |
| Part 1: Finding the events | 3–10b | Volatility-adjusted detection rules, episodes, labelling, 17 crises, robustness, depth and recovery, depth-controlled regression |
| Part 2: Market behavior | 11–15 | Cross-asset moves, hedges, sector rotation, starting conditions and policy |
| Part 2: Stress testing | 16–20c | Historical replay, family scenarios with walk-forward and leave-one-out validation, VaR, reverse stress test, loss attribution, what-if scenarios |
| Robustness and conclusions | 21–23 | Sensitivity of each conclusion, generated findings, limitations |

## Citations

**Academic literature**

- Baker, S. R., Bloom, N., Davis, S. J., & Sammon, M. C. (2021). *What triggers stock market jumps?* NBER Working Paper No. 28687.
- Bloom, N. (2009). The impact of uncertainty shocks. *Econometrica*, 77(3), 623–685.
- Campbell, J. Y., & Vuolteenaho, T. (2004). Bad beta, good beta. *American Economic Review*, 94(5), 1249–1275.
- Freedman, D., & Lane, D. (1983). A nonstochastic interpretation of reported significance levels. *Journal of Business & Economic Statistics*, 1(4), 292–298. (Residual permutation test used in Section 10b.)
- J.P. Morgan/Reuters. (1996). *RiskMetrics—Technical document* (4th ed.). (EWMA volatility, λ = 0.94.)
- Lee, S. S., & Mykland, P. A. (2008). Jumps in financial markets: A new nonparametric test and jump dynamics. *Review of Financial Studies*, 21(6), 2535–2563.
- Longin, F. M. (1996). The asymptotic distribution of extreme stock market returns. *Journal of Business*, 69(3), 383–408.
- Longin, F., & Solnik, B. (2001). Extreme correlation of international equity markets. *Journal of Finance*, 56(2), 649–676.

**Data**

- Yahoo Finance, daily prices accessed through the `yfinance` Python library (Aroussi, R., [github.com/ranaroussi/yfinance](https://github.com/ranaroussi/yfinance)), version 0.2.59.
- Federal Reserve Bank of St. Louis, FRED: [BAA10Y](https://fred.stlouisfed.org/series/BAA10Y), [CPIAUCSL](https://fred.stlouisfed.org/series/CPIAUCSL), [DGS10](https://fred.stlouisfed.org/series/DGS10), [DTB3](https://fred.stlouisfed.org/series/DTB3).

**News sources used to label events**

- Dunya News. *Iran war timeline and key moments, explained.* [Link](https://dunyanews.tv/en/World/953391-iran-war-timeline-and-key-moments-explained)
- BusinessDay (2026, March 31). *Dashed rate cut hopes sets gold up for worst month in 17 years.* [Link](https://www.businessday.co.za/markets/2026-03-31-dashed-rate-cut-hopes-sets-gold-up-for-worst-month-in-17-years)

**Software:** pandas, NumPy, Matplotlib, yfinance, Jupyter.

## Acknowledgements

An AI assistant (Claude, by Anthropic) was used to help write and debug code, structure the analysis and edit the report. The research question, the choice and interpretation of results, and the final submission are my own.
