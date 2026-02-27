```markdown
# Trader Performance vs Market Sentiment Analysis

**Author:** Tamanna Bhrigunath  
**Role:** Data Science Intern Assignment (Primetrade.ai)  

## Objective
This project analyzes the relationship between Bitcoin market sentiment (Fear vs. Greed) and trader behavior/performance on the Hyperliquid exchange. The goal of this analysis is to uncover data-backed patterns regarding profitability, risk exposure, and trading frequency to inform smarter, actionable algorithmic trading strategies.

*Note: The assignment prompt suggested analyzing "leverage distribution." Because leverage data was not available in the provided historical dataset, `Size USD` was used as the primary proxy to measure trader risk exposure and position sizing.*

## Project Structure
```text
Trader_Sentiment_Analysis/
│
├── data/
│   ├── fear_greed_index.csv      # Dataset 1: Bitcoin Fear & Greed Index
│   └── historical_data.csv       # Dataset 2: Hyperliquid Historical Trades
│
├── outputs/
│   ├── behavior_by_sentiment.png
│   └── trader_archetypes_clustering.png
│
├── notebook_1.ipynb              # Main Jupyter Notebook containing the full analysis
├── ds_report.pdf                 # 1-page executive summary report
└── README.md                     # Project documentation and summary

```

## Methodology

1. **Data Preparation & Cleaning:** Both datasets were loaded and cleaned. The Hyperliquid execution timestamps (Unix milliseconds) were converted and aligned with the daily Fear/Greed index.
2. **Feature Engineering:** Created key metrics including absolute PnL, risk exposure (USD), daily long/short ratios, and individual trade win rates (`is_win`).
3. **Trader Segmentation:** Traders were grouped by `Account` into specific archetypes to see how different cohorts react to market shifts:
* **Trade Frequency:** "Frequent Traders" (top 25% by trade volume) vs. "Infrequent Traders".
* **Profitability:** "Consistent Winners" (overall win rate > 50%) vs. "Inconsistent Traders".
* **Bonus (Machine Learning):** Applied K-Means clustering to organically group traders based on their frequency and win-rate profiles.


4. **Statistical Analysis:** Conducted Welch's t-tests to mathematically validate that observed differences in risk exposure and PnL between Fear and Greed days were statistically significant.

## Key Insights

1. **Risk Appetite Surges in Greed:** Average risk exposure (measured by trade size in USD) is significantly higher during Greed phases compared to Fear phases, indicating a stark shift in market risk appetite.
2. **PnL Doesn't Scale with Risk:** Despite the increased trade sizes during Greed days, median PnL remains largely stagnant. Frequent traders actually see a slight dip in their overall win rates during Extreme Greed.
3. **Smart Money Adapts:** "Consistent Winners" adapt to market sentiment much better than the average trader, dynamically altering their Long/Short ratios and leaning into short biases when the market transitions into Fear.

## Actionable Recommendations (Strategy Rules)

Based on the behavioral analysis, here are two actionable rules of thumb for trading algorithms:

* **Rule 1 (Cap Position Sizes During Greed):** The data shows that while traders significantly increase their risk exposure during Greed phases, their median PnL does not improve to match that risk.
* **Strategy:** If running a high-frequency algorithmic strategy, implement a hard ceiling on order sizes when the index hits "Greed" to prevent the bot from over-leveraging into hype. Smaller, more frequent positions yield better risk-adjusted returns here.


* **Rule 2 (Follow Consistent Winners into Shorts During Fear):** "Consistent Winners" (accounts with >50% historical win rates) behave drastically differently during Fear days—they significantly increase their ratio of short positions compared to the broader market.
* **Strategy:** To mimic this smart-money behavior, a portfolio algorithm should automatically lower its long exposure limit and heavily weight signal generation toward shorts whenever the daily sentiment classification drops into "Fear".



## Instructions to Run

1. Ensure Python 3.8+ is installed along with the following libraries: `pandas`, `numpy`, `scipy`, `matplotlib`, `seaborn`, and `scikit-learn`.
2. Ensure `fear_greed_index.csv` and `historical_data.csv` are placed inside the `data/` folder.
3. Ensure there is an empty `outputs/` folder in the root directory to receive the generated plots.
4. Run `notebook_1.ipynb` from top to bottom to reproduce the data cleaning, statistical tests, K-Means clustering, and visual outputs.
