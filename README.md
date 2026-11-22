# NIFTY 50 Non-Parametric Monte Carlo Simulator

A quantitative risk-modeling engine built in Python that simulates **10,000 possible 1-year return trajectories** for the **NIFTY 50 (`^NSEI`)** using non-parametric historical bootstrapping[cite: 1, 2].

Instead of assuming stock market returns follow a bell curve (normal distribution), this simulator samples directly from nearly two decades of actual NIFTY 50 daily moves—preserving real-world fat tails, market crashes, and compounding skewness[cite: 1, 2].

---

## Key Simulation Results (10,000 Paths × 252 Trading Days)

| Metric | Value | Interpretation |
| :--- | :--- | :--- |
| **Median 1-Year Return (50th pct)** | **+10.68%** | Typical, middle-of-the-road 1-year return without extreme bull or bear outliers. |
| **Mean 1-Year Return (Average)** | **+13.09%** | **+2.41% higher than the Median**, confirming **positive skewness** driven by compounded bull-market runs. |
| **95% Value at Risk (5th pct)** | **-21.54%** | Worst-case loss threshold in a 1-in-20 bad year (95% of simulated futures ended better than `-21.54%`). |
| **99% Value at Risk (1st pct)** | **-32.29%** | Extreme tail-risk drawdown in a 1-in-100 crash scenario. |
| **95th Percentile (Upside Tail)** | **+55.85%** | Top-5% bull-market outcome—more than **2.5×** the magnitude of the 5th-percentile downside risk (`-21.54%`). |
| **Probability of Loss (`< 0%`)** | **31.48%** | Roughly **1 in 3 simulated 1-year periods** ended in the red, highlighting short-horizon equity volatility. |

---

## How It Works

1. **Historical Data Ingestion (`yfinance`):** Downloads daily closing prices for `^NSEI` from September 2007 through December 2025 (`4,487` raw trading days)[cite: 1, 2].
2. **Return Pool Construction:** Computes daily percentage changes (`pct_change()`) and drops missing values (`dropna()`) to build an empirical pool of `4,486` historical daily returns[cite: 1, 2].
3. **Vectorized Historical Bootstrapping:** Uses `np.random.choice(..., size=(252, 10000), replace=True)` to generate 10,000 independent 1-year trading sequences without Python loops[cite: 1, 2].
4. **Geometric Compounding:** Multiplies daily growth factors across all 252 trading days via `np.cumprod(1 + r, axis=0)` and subtracts `1` at day 252 to extract final 1-year percentage returns[cite: 1, 2].
5. **Risk & Distribution Analytics:** Evaluates Median vs. Mean skewness, empirical 95% and 99% Value at Risk (VaR), 95th-percentile upside, and overall loss probability.

---

## Tech Stack
* **Python 3**[cite: 1, 2]
* **NumPy** — Vectorized bootstrap sampling, cumulative product compounding, and statistical metrics[cite: 1, 2]
* **Pandas** — Time-series cleaning, percentage return calculation, and DataFrame manipulation[cite: 1, 2]
* **yfinance** — Historical market data retrieval for `^NSEI`[cite: 1, 2]

---

## Model Assumptions & Limitations
* **i.i.d. Sampling (No Volatility Clustering):** Drawing individual days independently breaks multi-day autocorrelation—in real markets, high-volatility panic days tend to cluster together rather than appearing randomly.
* **Stationarity:** Assumes the statistical behavior of 2007–2025 NIFTY 50 returns remains representative of future market regimes.
* **Price Return Only:** Uses index closing prices (`^NSEI`), excluding dividend yields, taxes, and transaction costs[cite: 1, 2].

---

## Getting Started

```bash
# Clone the repository
git clone [https://github.com/YOUR_USERNAME/nifty50-monte-carlo-simulator.git](https://github.com/YOUR_USERNAME/nifty50-monte-carlo-simulator.git)
cd nifty50-monte-carlo-simulator

# Install dependencies
pip install numpy pandas yfinance jupyter

# Launch the notebook
jupyter notebook nifty50_monte_carlo_simulator.ipynb
