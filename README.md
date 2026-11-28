# NIFTY 50 Quantitative Risk Engine — Monte Carlo & Historical Bootstrap VaR

A quantitative market risk backtesting project evaluating **Value at Risk (VaR)** across **15 years (3,730 trading days)** of NIFTY 50 daily returns. The model simulates daily loss distributions using a **756-day (3-year) rolling empirical Monte Carlo bootstrap** and validates tail performance under the **Kupiec & Christoffersen statistical backtesting framework**.

---

## 🎯 Project Objective
Most basic risk models stop at calculating a single VaR number or checking average breach counts. This project tests whether a standard 3-year rolling Monte Carlo bootstrap actually protects a portfolio during real market panics—or whether its risk limits adapt too slowly when volatility spikes.

---

## ⚙️ Simulation & Validation Architecture

1. **Rolling Window Setup (`756` Trading Days):**
   * Uses a 3-year rolling lookback window across **3,730 NIFTY 50 trading days** to capture changing market cycles without look-ahead bias.
2. **Monte Carlo Empirical Bootstrap:**
   * At each trading step $t$, draws resampled return paths from the trailing 756-day empirical return distribution to estimate next-day **95% VaR** and **99% VaR** thresholds.
3. **Out-of-Sample Breach Tracking:**
   * Compares realized daily NIFTY 50 returns against forecasted VaR bounds to generate binary breach sequences (`True` / `False`) across the entire 15-year test period.
4. **Statistical Backtesting (Kupiec & Christoffersen Framework):**
   * **Unconditional Coverage:** Tests whether realized breach rates match theoretical expectations (`5.00%` and `1.00%`).
   * **Christoffersen’s Markov Independence Test ($LR_{ind}$):** Computes transition counts ($n_{00}, n_{01}, n_{10}, n_{11}$) and conditional breach probabilities ($\pi_0$ vs. $\pi_1$) using log-likelihood ratios to test whether breaches occur independently or cluster into multi-day streaks against the $\chi^2_1$ critical threshold (**`3.841`** at 95% confidence).

---

## 📊 Backtest Results (3,730 Trading Days)

| Metric | 95% VaR | 99% VaR | Benchmark / Threshold |
| :--- | :---: | :---: | :--- |
| **Expected Breach Rate** | `5.00%` | `1.00%` | Theoretical tail expectation |
| **Realized Breach Rate** | **`3.86%`** | **`0.97%`** | Unconditional coverage |
| **Christoffersen $LR_{ind}$** | **`3.15`** | **`18+`** | $\chi^2_1$ critical value = **`3.841`** (95% conf.) |
| **Independence Verdict** | **Pass** *(borderline)* | **Fail** *(severe clustering)* | $LR_{ind} < 3.841$ required |

---

## 🔍 Key Empirical Findings

### 1. Accurate on Long-Term Averages, Sluggish on Sudden Regime Shifts
Over a 15-year horizon, the model’s unconditional breach frequencies appear well-calibrated (`0.97%` realized vs. `1.00%` expected at 99% VaR). However, Markov transition analysis exposes a critical structural limitation of equal-weighted resampling (`1/756 = 0.13%` weight per historical day):

* **Crisis-Time Lag & Breach Clustering ($LR_{ind} > 18$ on 99% VaR):**
  In Christoffersen’s test, $\pi_1$ (probability of breaching today given yesterday breached) is drastically higher than $\pi_0$ (probability of breaching after a safe day). Because a new crash day initially receives only `0.13%` sampling weight in a 756-day window, the 99% VaR boundary adjusts too slowly when a panic hits—requiring roughly a week of heavy losses to shift upward and causing consecutive breach streaks.
* **Post-Crash "Ghost Effect" (`3.86%` breaches on 95% VaR):**
  Once a market sell-off ends and daily volatility subsides, extreme historical losses remain inside the 756-day bootstrap pool for three full years—keeping the 95% VaR limit overly conservative during calm recovery periods.
* **Overnight Gap Exposure:**
  Backward-looking daily return resampling cannot anticipate sudden overnight macro or global shocks before market open.

---

## 📁 Repository Contents
* `*.ipynb` — End-to-end Jupyter Notebook containing data preprocessing, 756-day rolling Monte Carlo bootstrap VaR simulation, breach tracking, and Christoffersen log-likelihood independence testing.
* `README.md` — Project architecture, statistical backtest metrics, and empirical risk diagnosis.

---

## 💡 Core Takeaway
> While a 3-year rolling Monte Carlo bootstrap accurately captures **long-run unconditional tail risk**, equal-weighted resampling treats yesterday's shock identically to a quiet day from years ago—making it conservative after crises (**Ghost Effect**) and slow to react during acute market panics ($\pi_1 \gg \pi_0$).
