# Google-Trends-Economic-Data-scraper

# African Market Intelligence Pipeline: Identifying Investment Opportunities

## Project Overview

This project develops an **African Market Intelligence Pipeline** designed to identify and assess investment attractiveness for Private Equity (PE) and Development Finance Institution (DFI) investors across key African markets: Kenya, Nigeria, Ghana, South Africa, and Egypt.

It integrates two primary data sources:

1.  **Google Trends:** To capture real-time consumer interest and sentiment, acting as a leading economic indicator.
2.  **World Bank API:** To pull official macro-economic data (GDP, inflation, FDI, etc.) for a foundational economic overview.

The ultimate goal is to combine these diverse data points into a **Country Attractiveness Index**, providing a data-driven verdict on where to deploy capital.

## Key Components & Methodology

### Part 1: Google Trends - Consumer Pulse Across Africa

This section uses the `pytrends` library to analyze search interest for various keywords across the target countries. Google Trends data is normalized and serves as a proxy for:

*   **Consumer Confidence:** e.g., searches for "buy car," "new phone."
*   **Economic Stress:** e.g., searches for "loan," "job vacancy."
*   **Sector Growth:** e.g., searches for "mobile banking," "solar panel."

**Steps:**
1.  **Setup Pytrends:** Initialize connection to Google Trends.
2.  **Track Mobile Banking Interest:** Analyze search interest for 'mobile banking' across all five countries over a 5-year period, smoothed monthly.
3.  **Track Multiple Economic Signals (Kenya Example):** Explore various economic signals (fintech growth, consumer spend, economic stress, job market, solar adoption) for a single country (Kenya) to demonstrate broader applicability.
4.  **Visualize Trends:** Plot the historical search interest for 'mobile banking' across countries and for different economic signals in Kenya.
5.  **Score Trend Momentum:** Calculate a momentum score by comparing recent (last 6 months) search interest with prior periods, categorizing countries as 'GROWING', 'DECLINING', or 'STABLE'.

### Part 2: World Bank API - Official Macro Data

This section leverages the `world-bank-data` library to extract official macro-economic indicators, providing a traditional economic backdrop.

**Steps:**
1.  **Install World Bank Data Library:** Ensure the necessary library is installed.
2.  **Define Indicators:** Use specific World Bank indicator codes for GDP growth, inflation, FDI inflows, unemployment, debt-to-GDP, and mobile penetration.
3.  **Pull Macro Data:** Retrieve the most recent 5 years of data for the selected indicators and countries.
4.  **Initial Data Restructuring:** Attempt to restructure the pulled data into a clean country-level summary.
5.  **Address Data Limitations:** Recognize and address issues with missing/incomplete World Bank data due to indicator code mismatches or data lag. This involves using a verified set of indicators and performing manual estimations for critical missing values (e.g., Nigeria's Exports_pct_GDP).

### Part 3: Country Attractiveness Index

This is the core deliverable, combining insights from Google Trends and World Bank data into a unified investment score.

**Steps:**
1.  **Define Scoring Function (`score_country_v2`):** A custom Python function is created to calculate an investment attractiveness score (0-100) based on:
    *   GDP Growth (weighted up to 25 points)
    *   Inflation (weighted up to 20 points, lower is better)
    *   GDP per capita (as a proxy for Market Maturity, weighted up to 15 points)
    *   Mobile Penetration (as a proxy for Digital Economy, weighted up to 15 points)
    *   Consumer Momentum (from Google Trends, weighted up to 25 points)

    The function also assigns a `Verdict` ('ATTRACTIVE', 'MONITOR', 'CAUTION') based on the total score.
2.  **Calculate and Present Scores:** Apply the scoring function to each country using the cleaned macro data and Google Trends momentum scores. The results are presented in a DataFrame, sorted by total score.
3.  **Visualize the Investment Map:** Professional visualizations are generated using `matplotlib` to display total scores, verdicts, and a detailed breakdown of score contributions by component for each country.

## Question Analysis & Insights

The notebook includes a detailed question analysis section to interpret the results and draw actionable insights:

*   **Q1: Highest Ranking Country:** Identifies the top-ranked country and discusses whether its position is justified by the underlying data components (especially after fixing World Bank data issues).
*   **Q2: Worst Inflation Score & PE Impact:** Highlights the country with the highest inflation and explains why high inflation is a critical concern for long-term PE investors.
*   **Q3: GDP per Capita vs. Overall Rank:** Examines if the country with the highest GDP per capita also ranks highest overall, revealing the multi-faceted nature of the attractiveness index.
*   **Q4: Three-Sentence Verdict for BII/IFC:** Provides a concise, executive-level summary of the African market opportunity based on the analysis.

## Limitations & Robustness Improvements

The project acknowledges several limitations and proposes improvements for future iterations:

### Current Limitations:
*   **Initial Missing World Bank Data:** The initial model suffered from `None` values for all macro-economic indicators, leading to an unreliable score. This has since been addressed in `v2`.
*   **Narrow Google Trends Scope:** 'Consumer Momentum' is currently based on a single keyword ('mobile banking').
*   **Static & Arbitrary Weights/Thresholds:** Scoring weights and verdict thresholds are fixed and not data-driven.
*   **Simple Scoring Logic:** Linear transformations might oversimplify complex economic relationships.
*   **Lack of Historical Context for Macro Data:** The scoring primarily uses the latest macro values without deep time-series analysis.
*   **No Risk Factors:** Excludes crucial investment risk factors like political stability or regulatory environment.

### Proposed Improvements:
*   **Robust World Bank Data Handling:** Ensure proper fetching, debugging, and consider alternative sources or imputation for missing data.
*   **Expanded Google Trends Keywords:** Incorporate a broader set of keywords for a more comprehensive sentiment signal.
*   **Dynamic Weighting & Thresholds:** Utilize expert input or machine learning for more refined weighting and thresholding.
*   **Sophisticated Scoring Functions:** Explore non-linear or percentile-based scoring.
*   **Integrate More Risk Factors:** Include qualitative and quantitative data on political stability, ease of doing business, etc.
*   **Time Series Analysis:** Analyze trends and volatility in macro indicators.
*   **Sensitivity Analysis:** Understand how changes in inputs affect the final scores.

## Conclusion

This **African Market Intelligence Pipeline** provides a robust framework for assessing investment attractiveness by combining diverse data sources. While the initial model highlighted the challenges of data availability, the refined version (`v2`) demonstrates a more comprehensive and data-driven approach, offering valuable insights for PE/DFI investors navigating the dynamic African market landscape.
