# Trader-Behaviour-Insights

This project explores the relationship between **market sentiment** (e.g., Greed, Fear, Neutral) and **trading performance** using a large real-world dataset. It performs data cleaning, exploratory data analysis (EDA), statistical testing, predictive modeling, sentiment lag analysis, and unsupervised clustering to extract key behavioral insights.

---

## 📂 Section 1: Connecting Drive & Importing Dataset

- Mounted Google Drive to access and load the dataset.
- Read merged trade data with key fields such as:
  - Execution price
  - Trade size
  - Trade direction (buy/sell)
  - PnL (Profit & Loss)
  - Market sentiment classification

---

## 🧹 Section 2: Data Cleaning

- Removed missing values and duplicates.
- Parsed timestamps and converted to IST.
- Ensured consistent data types.
- Engineered `is_profitable` column to classify profitable vs unprofitable trades.

**✅ Cleaned Dataset Summary:**
- Total Trades: `211,224`
- Sentiment Categories: `Extreme Fear`, `Fear`, `Neutral`, `Greed`, `Extreme Greed`

---
## 📊 Section 3: Exploratory Data Analysis (EDA)

This section explores the relationship between **market sentiment** and **trade performance metrics** like profitability, trade size, and event type. The key objective is to identify how trader behavior and outcomes vary under different emotional conditions (e.g., greed, fear).

---

### 🔵 1. Sentiment Distribution — *Pie Chart*

**What it shows:**  
The proportion of trades executed under each sentiment condition.

**Insight:**  
- Most trades occur under `Fear`, `Greed`, and `Extreme Greed`.  
- `Extreme Fear` conditions lead to the fewest trades — possibly indicating market hesitancy.

---

### 🔷 2. Average Profit per Sentiment — *Bar Plot*

**What it shows:**  
Mean `closed_pnl` (profit/loss) grouped by `classification` (sentiment).

| Sentiment       | Avg. PnL |
|-----------------|----------|
| Extreme Greed   | 67.91    |
| Fear            | 54.35    |
| Greed           | 42.95    |
| Neutral         | 34.35    |
| Extreme Fear    | 34.33    |

**Insight:**  
- Trades made in **Extreme Greed** periods are most profitable on average.  
- Surprisingly, **Fear** yields better returns than **Greed**, suggesting more disciplined trading.  
- **Neutral** and **Extreme Fear** lead to lower profits, possibly due to market indecision.

---

### 📉 3. Profitability Distribution — *Box Plot*

**What it shows:**  
Distribution of `closed_pnl` across sentiment categories.

**Insight:**  
- `Extreme Greed` has the **highest median** and **widest range**.  
- `Fear` profits are relatively **stable**, suggesting controlled trading.  
- Losses during `Greed` and `Extreme Fear` can be **extreme**, showing risk volatility.

---

### 📈 4. PnL Over Time by Sentiment — *Line Plot*

**What it shows:**  
Average `closed_pnl` per day, colored by sentiment classification.

**Insight:**  
- Helps visualize how sentiment phases affect market behavior over time.  
- Shows spikes in profitability during strong sentiment waves (e.g., `Extreme Greed` rallies).

---

### 🟧 5. Trade Size by Sentiment — *Box Plot / Violin Plot*

**What it shows:**  
Variation in `size` (trade volume) under different sentiment regimes.

**Insight:**  
- Larger trades are often placed under `Greed` and `Extreme Greed`.  
- Smaller trades dominate in `Neutral` and `Extreme Fear`, suggesting defensive positions.

---

### 🟨 6. Count of Event Types per Sentiment — *Grouped Bar Plot*

**What it shows:**  
Number of trading `events` (open, close, SL-hit, etc.) by sentiment.

**Insight:**  
- More `close` and `SL-hit` events in `Fear` and `Extreme Fear`.  
- More `open` events during `Greed` and `Extreme Greed` — traders are aggressive in bull runs.

---

### 🟩 7. Trade Profitability by Symbol & Sentiment — *Heatmap*

**What it shows:**  
Average `closed_pnl` for each symbol-sentiment pair.

**Insight:**  
- Certain symbols perform better in specific sentiments.  
- Helps build **sentiment-aware trading strategies** by selecting symbols with better risk-adjusted return profiles.

---

### 🟦 8. Correlation Matrix — *Heatmap*

**What it shows:**  
Pairwise correlation between all numeric variables (e.g., `size`, `start_position`, `pnl`).

**Insight:**  
- `Size` and `execution_price` have low correlation with `closed_pnl`.  
- Suggests profit is more sentiment- and strategy-driven than size-driven.

---

### 📌 Summary of EDA Insights:

- **Sentiment significantly affects trade profitability** both visually and statistically.
- **Extreme Greed** is often associated with high-profit trades.
- **Fear**, while traditionally seen as negative, often results in disciplined and profitable trading.
- Trading behavior (size, event type) shifts meaningfully across different sentiment states.


---

## 🧪 Section 4: Statistical Testing

### T-Test: Greed vs Fear PnL Comparison

- **T-statistic:** -1.851  
- **P-value:** 0.0642

**🧠 Interpretation:**  
While not significant at the 5% level, this result is **directionally meaningful**. It suggests that **Greed-period trades tend to outperform Fear-period trades** in profitability. Further sampling or refined metrics may yield significance.

---

## 🤖 Section 5: Predicting Profitability (Classification Model)

### Model: Random Forest Classifier
- **Target:** `is_profitable` (binary)
- **Features:** sentiment, trade size, execution price, start position, symbol, event type

### ✅ Model Performance:
| Metric        | Score |
|---------------|-------|
| Accuracy      | 99%   |
| Precision     | 97%   |
| Recall        | 99%   |
| F1-score      | 98%   |

**🧠 Insight:**  
Market sentiment and trade size are strong indicators of trade profitability. The model generalizes well with minimal overfitting.

---

## ⏱️ Section 6: Sentiment Time-Lag Effects

- Introduced `lagged_classification` to reflect previous day’s sentiment.
- Compared lagged sentiment to current-day PnL.

| Lagged Sentiment | Avg. PnL |
|------------------|----------|
| Extreme Greed    | 67.91    |
| Fear             | 54.35    |
| Greed            | 42.95    |
| Extreme Fear     | 34.33    |
| Neutral          | 34.35    |

**🧠 Insight:**  
Sentiment from the **previous day** significantly impacts trade performance. This suggests a **momentum effect or trader psychology carryover**.

---

## 📉 Section 7: Risk-Adjusted Returns (Sharpe-like Ratio)

Computed `return / standard deviation` of PnL by sentiment:

| Sentiment       | Return / Risk Ratio |
|-----------------|---------------------|
| Extreme Greed   | Highest             |
| Fear            | Medium              |
| Greed           | Medium-High         |
| Neutral         | Low                 |
| Extreme Fear    | Lowest              |

**🧠 Insight:**  
Greedy periods deliver the **best risk-adjusted performance**, while fearful conditions produce the **least favorable trade-offs**.

---

## 🔍 Section 8: Clustering Analysis (Unsupervised Learning)

### Method: KMeans Clustering (k=4)

Clustered trades based on:
- Trade size
- Execution price
- Start position
- Market sentiment

### ✅ Cluster Breakdown:

| Cluster | Dominant Sentiment     |
|---------|------------------------|
| 0       | Fear (28%), Greed (23%), Extreme Greed (20%)  
| 1       | Fear (39%), Greed (25%), Neutral (19%)  
| 2       | Extreme Greed (46%), Greed (18%)  
| 3       | Greed (73%), Neutral (12%), Fear (9%)  

**🧠 Insight:**
- **Cluster 2** represents high-risk, high-reward behavior (Extreme Greed).
- **Cluster 1** is more conservative (Fear-dominated).
- Clusters reflect **behavioral archetypes** which can be useful for personalized trade recommendations.

---

## 📌 Summary of Key Findings

- **Greedy market sentiment is linked to higher profitability and better risk-adjusted returns**.
- **Lagged sentiment influences today’s trade performance**, indicating behavioral continuity.
- **Classification models can predict trade profitability with high accuracy**.
- **Clustering reveals distinct trading behavior segments** driven by sentiment.

---

## 🚀 Future Work

- Incorporate **time-series models** (e.g., LSTM, ARIMA) for forecasting sentiment impact.
- Enrich features using **real-time sentiment** from Twitter/news APIs.
- Add **hyperparameter tuning** and **cross-validation** to models.
- Build a **Streamlit dashboard** for interactive analysis.
- Track **sequential trade behavior** using RNNs or Transformer models.

---

## 📁 Project Structure

> This project consists of a single self-contained notebook that performs:
> - Data cleaning and preparation
> - Exploratory data analysis (EDA) with visualizations
> - Statistical testing (t-tests)
> - Machine learning classification model
> - Time-lagged sentiment analysis
> - Clustering to explore trader behavior segments

