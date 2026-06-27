# Residual Analysis

This document presents the residual analysis of our selected final model (**Multiple Regression Model 2 - Final Comprehensive**). 

The residual for each store-month observation is calculated as:
$$\text{Residual} = \text{Actual Monthly Sales} - \text{Predicted Monthly Sales}$$

This analysis helps identify stores that are either over-performing or under-performing relative to what their marketing, footfall, location, and operational metrics would predict. It also helps diagnose any systematic model bias across store formats or regions.

---

## Extremes in Residuals

By sorting the residuals, we identify the top 5 over-performing stores (largest positive residuals) and the top 5 under-performing stores (largest negative residuals).

### Top 5 Positive Residuals (Over-Performing Stores)
These stores generated significantly higher sales than our comprehensive regression model predicted:

| Observation | Store ID | Region | Store Type | Month | Actual Sales | Predicted Sales | Residual | Error % |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 230 | STR-1058 | East | High Street | 2025-04-01 | \$870,937.40 | \$759,919.94 | **+\$111,017.46** | 12.75% |
| 112 | STR-1028 | East | Mall | 2025-04-01 | \$713,611.16 | \$610,531.95 | **+\$103,079.21** | 14.44% |
| 291 | STR-1073 | East | Residential | 2025-03-01 | \$813,316.71 | \$721,415.92 | **+\$91,900.79** | 11.30% |
| 202 | STR-1051 | East | Airport | 2025-02-01 | \$787,715.51 | \$701,381.34 | **+\$86,334.17** | 10.96% |
| 104 | STR-1026 | East | Mall | 2025-04-01 | \$625,514.04 | \$540,234.62 | **+\$85,279.42** | 13.63% |

*Note: Observations are 1-indexed to match Excel workbook row numbers (Observation # = Row index - 4).*

### Top 5 Negative Residuals (Under-Performing Stores)
These stores generated significantly lower sales than predicted, suggesting operational challenges or negative local factors:

| Observation | Store ID | Region | Store Type | Month | Actual Sales | Predicted Sales | Residual | Error % |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 90 | STR-1023 | South | Mall | 2025-02-01 | \$627,171.90 | \$764,038.71 | **-\$136,866.81** | 21.82% |
| 67 | STR-1017 | West | High Street | 2025-03-01 | \$685,379.08 | \$805,279.85 | **-\$119,900.77** | 17.50% |
| 45 | STR-1012 | West | Residential | 2025-01-01 | \$595,467.60 | \$709,913.78 | **-\$114,446.18** | 19.22% |
| 26 | STR-1007 | West | Mall | 2025-02-01 | \$800,451.94 | \$912,344.87 | **-\$111,892.93** | 13.98% |
| 237 | STR-1060 | West | Mall | 2025-04-01 | \$721,079.35 | \$808,021.71 | **-\$86,942.36** | 12.06% |

---

## Business Interpretation of Residuals

In regression analysis, residuals represent the variation in the dependent variable (`monthly_sales`) that cannot be explained by the independent variables in the model. In business terms:

### 1. What Positive Residuals Mean:
A large positive residual indicates that a store is **over-performing** relative to expectations based on its marketing, footfall, ratings, and location. Leadership should investigate these stores as "best practice" cases. Potential uncaptured success factors include:
* **Exceptional Store Leadership:** A highly motivated store manager or sales team with excellent customer service practices.
* **Superior Product Mix:** Better localized merchandising that aligns perfectly with neighborhood demographics, leading to higher transaction values.
* **Effective Local Marketing:** Hyper-local community sponsorships or word-of-mouth reputation that drives higher sales without showing up as corporate "marketing spend."
* **Weak Competitors:** Although competitor distance is controlled for, the *quality* or *reputation* of the competitor isn't. A competitor nearby might be poorly run, pushing customers to our store.

### 2. What Negative Residuals Mean:
A large negative residual indicates that a store is **under-performing** relative to what its footprint and footfall should produce. These stores represent urgent areas for operational intervention. Potential uncaptured problems include:
* **Operational Friction:** Long checkout lines, poor store layout, or bad customer service that deters purchases.
* **High Staff Attrition or Low Morale:** Even if the count of staff is normal, new or unmotivated staff are less effective at converting footfall into sales.
* **Localized Stock Issues:** Stockouts of highly popular regional products, even though overall inventory availability looks acceptable.
* **Local Disturbances:** Roadwork, utility issues, or mall renovations that disrupt access to the store, creating an artificial drop in sales.

---

## Systematic Bias Analysis: Under/Over-Prediction

We analyzed the mean residuals across different store formats and regions to determine if the model systematically under-predicts or over-predicts certain categories.

### 1. Analysis by Region and Store Type:
Mathematically, the mean residual for each individual region and store type is extremely close to zero (on the order of $10^{-8}$). This indicates that the OLS model is **unbiased on average** across all categories because we included dummy variables for regions and store types. The model does not systematically over-predict or under-predict one entire region or store type.

### 2. Residual Volatility Analysis:
While the mean residual is zero, the **standard deviation (spread) of the residuals** varies by store type:
* **High Street:** \$37,510.94
* **Residential:** \$38,303.04
* **Airport:** \$41,452.80
* **Mall:** \$42,867.11

**Business Insight:**
* **Mall and Airport Stores** have higher residual standard deviations. This means their sales are more volatile and harder for the model to predict perfectly. This makes sense in business terms, as Mall and Airport sales are highly dependent on transient tourist flows, flight schedule changes, holiday travel patterns, or mall-wide events, which are not captured in monthly store-level averages.
* **High Street and Residential Stores** have lower residual standard deviations, indicating more stable and predictable sales. These stores serve local, repeating neighborhoods, making them less prone to external, transient spikes or drops.

---

## Workbook Visual Evidence

A visual preview of the calculated predictions, residuals, and the top residual tables can be found in:
[residuals_preview.png](file:///c:/Users/HP/OneDrive/Desktop/Assignment/part3_regression_insights/screenshots/residuals_preview.png)

This screenshot confirms the setup of the prediction and residual sheet in the workbook, facilitating audits and quick reviews of store performance anomalies.
