[1. GET 10 YEARS OF CLOSING PRICES]
       │  Load past daily closing prices into Pandas
       ▼
[2. CONVERT TO DAILY PERCENTAGE CHANGES]
       │  Use .pct_change().dropna() so you have a clean list
       │  of ~2,500 daily percentage moves (your "hat of slips")
       ▼
[3. SPIN 10,000 FUTURES × 252 DAYS AT ONCE]
       │  Use np.random.choice(past_returns, size=(10000, 252), replace=True)
       │  No loops needed—creates a 10,000 × 252 grid of daily moves
       ▼
[4. COMPOUND ACROSS THE 252 DAYS]
       │  Use np.cumprod(1 + random_returns, axis=1)
       │  Starts at 1.0 and multiplies day-by-day across each row
       ▼
[5. CONVERT TO FINAL PERCENTAGE RETURNS]
       │  Take the last column (Day 252) and subtract 1:
       │  final_returns = growth_paths[:, -1] - 1
       ▼
[6. MEASURE THE ODDS & RISK]
       │  From those 10,000 final 1-year return percentages, check:
       │   • Median return (50th percentile)
       │   • Worst 5% return (5th percentile) & Worst 1% return (1st percentile)
       │   • % of simulated years that end negative (< 0%)
       ▼
[7. PLOT 2 CHARTS IN MATPLOTLIB]
          • Chart 1: Line plot of the first 100 simulated paths over 252 days
          • Chart 2: Histogram of all 10,000 final 1-year percentage returns



Summary in own words

So basically, I took 10 year daily returns of NIFTY 50 index fund, cleaned up the closing prices, and used numpy to get 10,000 possibilities of a single year with 252 trading days, using historical bootstrapping method, next would use this data and compound it to get annual returns for all those 10,000 possibilities and get median, 5th percentile and 95th percentile changes and would basically infer from that whether its a good index fund to invest in






General Info to prevent misunderstandings:
Skewness: Measure of which side the data is concentrated more comparing it to median
Positive: Mean > Median, means crazy bull years pulled the number up; More Bullish: So investing can be considered a good choice
Negative: Median > Mean, means crazy bear years were there; So investing should be considered and talked about thoughtfully.


Median: Tells about the average year looks like, cuz its at the middle of those 10,000 values, above it all bullish and below it all bearish
Compare the top 5th percentile(95 percentile) and bottom 5 percentile to understand about whether the risk is worth taking or not
Probability of loss, basically tells us what are the chances of losing the money (Out of those 10,000: how many ended below 0%)
We dont use mean to find about average year returns as extreme outliers tend to shift it
VaR is calculated post sorting and looking at bottom values to understand what is the worst that could happen

