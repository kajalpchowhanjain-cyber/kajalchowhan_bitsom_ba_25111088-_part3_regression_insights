# Retail Store Performance Analysis (Regression-Based Business Insights)

This repository contains a comprehensive, data-driven regression analysis of store performance across a retail chain. The objective is to identify the primary drivers of monthly sales across stores, helping leadership make informed business decisions regarding marketing spend, inventory availability, discounting strategy, staffing levels, and real estate prioritization.

---

## 1. Business Problem Summary

The leadership team of our retail chain wants to understand what factors are driving monthly sales performance across different store locations. Specifically, they need to evaluate the returns on marketing spend, the impact of store traffic (footfall), the effectiveness of promotional discounts, and the importance of operational metrics like staffing and inventory levels.

To address this, we have developed multiple linear regression models that control for locational and regional differences. The goal is to provide leadership with actionable recommendations backed by statistical evidence, outlining where to focus capital and resources and where to exercise caution.

---

## 2. Dataset Description

The dataset used in this analysis is stored in [business_regression_data.xlsx](file:///c:/Users/HP/OneDrive/Desktop/Assignment/part3_regression_insights/data/business_regression_data.xlsx). It contains **320 records** across various store locations, covering monthly observations. 

### Variables Analyzed:
* **Dependent Variable ($Y$):** 
  * `monthly_sales`: The primary business metric representing total monthly sales revenue for a store (numerical, currency).
* **Independent Variables ($X$):**
  * **Numerical Predictors:**
    * `marketing_spend`: Monthly advertising and marketing expenditure for the store (currency).
    * `footfall`: Monthly visitor count (integer).
    * `avg_discount_pct`: Average promotional discount rate applied during the month (percentage).
    * `staff_count`: Number of employees scheduled in the store (integer).
    * `inventory_availability_pct`: Average product availability on store shelves (percentage).
    * `competitor_distance_km`: Distance to the nearest competitor store (kilometers).
    * `holiday_flag`: Binary indicator (1 if the month had a major holiday period, 0 otherwise).
    * `customer_rating`: Average customer feedback score (1.0 to 5.0).
  * **Categorical Predictors:**
    * `region`: Business region where the store is located (East, North, South, West).
    * `store_type`: Store format/location type (Airport, High Street, Mall, Residential).
  * **Variables Dropped or Omitted:**
    * `store_id`: Excluded as it is a unique identifier, not a predictive factor.
    * `month`: Excluded as a predictor since we have a short time window (4 months) and time-series trends are represented by seasonal operational metrics and holiday flags.
    * `monthly_profit`: Omitted from the independent variables because profit is collinear with and derived directly from sales revenue.

---

## 3. Data Cleaning & Prep

During exploration, we identified **14 rows with missing values**:
* `competitor_distance_km`: 6 missing values.
* `customer_rating`: 8 missing values.

### Imputation Strategy:
We applied **Median Imputation** to fill these missing values (Median competitor distance: **3.65 km**; Median customer rating: **3.80**). This preserves all 320 observations for regression, maintaining statistical power without introducing bias from skewed outliers.

---

## 4. Dummy Variable Approach

Categorical variables (`region` and `store_type`) were converted into binary dummy variables (0 or 1). To avoid the **dummy variable trap** (perfect multicollinearity), we omitted one reference category from each group. The coefficients of the remaining dummy variables represent the difference in sales baselines compared to the reference category.

* **Region Reference Category:** **East**
  * Dummies: `region_North`, `region_South`, `region_West`
* **Store Type Reference Category:** **Airport**
  * Dummies: `store_type_High Street`, `store_type_Mall`, `store_type_Residential`

---

## 5. Model Comparison Summary

We developed and compared four linear regression models:

| Metric | Simple Model 1 (Marketing) | Simple Model 2 (Footfall) | Multiple Model 1 (Intermediate) | Multiple Model 2 (Final Comprehensive) |
| :--- | :---: | :---: | :---: | :---: |
| **Predictors ($k$)** | 1 | 1 | 6 | **14** |
| **R-squared ($R^2$)** | 16.72% | 73.63% | 79.77% | **85.70%** |
| **Adjusted $R^2$** | 16.46% | 73.55% | 79.38% | **85.04%** |
| **Std Error ($S$)** | \$94,846.03 | \$53,373.74 | \$47,118.41 | **\$40,133.89** |
| **Significance F** | $2.48 \times 10^{-14}$ | $4.75 \times 10^{-94}$ | $3.57 \times 10^{-96}$ | **$5.13 \times 10^{-120}$** |

A complete, auditable model comparison is documented in [model_comparison.md](file:///c:/Users/HP/OneDrive/Desktop/Assignment/part3_regression_insights/analysis/model_comparison.md) and formatted in the Excel output [regression_summary.xlsx](file:///c:/Users/HP/OneDrive/Desktop/Assignment/part3_regression_insights/outputs/regression_summary.xlsx).

---

## 6. Selected Final Model & Rationale

We selected **Multiple Model 2 (Final Comprehensive)** as the final model because:
1. **Highest Explanatory Power:** It explains **85.70%** of the variance in monthly sales, leaving only 14.30% to random noise.
2. **Lowest Prediction Error:** It minimizes the standard error of regression to **\$40,133.89**.
3. **Actionable Variables:** It includes operational variables under manager control (staffing, stock availability, and ratings), enabling direct intervention.

### Mathematical Equation:

$$\begin{aligned}
\text{Monthly Sales} = & \; 86,319.14 + 1.21 \times \text{MarketingSpend} + 27.34 \times \text{Footfall} - 41,243.15 \times \text{AvgDiscountPct} \\
& + 3,508.40 \times \text{StaffCount} + 3,062.21 \times \text{InventoryAvailabilityPct} - 3,438.11 \times \text{CompetitorDistanceKM} \\
& + 15,157.29 \times \text{HolidayFlag} + 12,509.46 \times \text{CustomerRating} + 9,948.71 \times \text{Region\_North} \\
& + 21,089.57 \times \text{Region\_South} + 25,291.42 \times \text{Region\_West} - 24,475.84 \times \text{StoreType\_HighStreet} \\
& - 11,862.24 \times \text{StoreType\_Mall} - 44,695.09 \times \text{StoreType\_Residential}
\end{aligned}$$

For coefficient-level explanations and details on equations, see [model_equations.md](file:///c:/Users/HP/OneDrive/Desktop/Assignment/part3_regression_insights/outputs/model_equations.md).

---

## 7. Business Recommendations

Our strategic recommendations are supported by regression evidence and detailed in [final_recommendation.md](file:///c:/Users/HP/OneDrive/Desktop/Assignment/part3_regression_insights/outputs/final_recommendation.md):

* **Inventory Availability:** Increase replenishment frequency to keep stock availability above 95%, as every 1% increase generates **\$3,062.21** in monthly sales.
* **Customer Service:** Improve store ratings, as a 1-point increase is associated with **\$12,509.46** in additional sales.
* **Staffing Levels:** Dynamically allocate labor, as each additional employee is associated with a **\$3,508.40** sales increase.
* **Capital Allocation (Expansion):** Prioritize expansion in the **West** and **South** regions, which show significantly higher sales baselines compared to the East region. Prioritize Airport and Mall formats, avoiding Residential areas where baseline demand is significantly lower.
* **Discounting Caution:** Do not over-interpret discounting as a direct sales driver ($p = 0.224$, statistically weak). Use discounts strictly as a local tactical mechanism to drive footfall, rather than a broad revenue-generation tool.

---

## 8. Assumptions and Limitations

* **Association vs. Causation:** The regression shows statistical associations. While operational improvements likely *cause* sales increases, some factors (like staffing) may suffer from reverse causality (successful stores are given higher labor budgets). Leadership should conduct controlled tests before wide execution.
* **Omitted Variables:** The model explains 85.70% of variance, but does not capture qualitative factors such as manager capability, local economic shocks, competitor pricing actions, or store aesthetics.

---

## 9. Repository Structure

```
part3_regression_insights/
├── data/
│   └── business_regression_data.xlsx       # Cleaned and original dataset
├── analysis/
│   ├── regression_workbook.xlsx            # Workbook with dummy sheets & OLS outputs
│   ├── model_comparison.md                 # Detailed markdown model comparison
│   └── residual_analysis.md                # Residual calculation & outlier analysis
├── outputs/
│   ├── regression_summary.xlsx             # Clean model comparison sheet
│   ├── final_recommendation.md             # Business strategy recommendation report
│   └── model_equations.md                  # Regression equations & coefficient guides
├── screenshots/
│   ├── simple_regression_output.png        # Simple regression output screenshot
│   ├── multiple_regression_output.png      # Multiple regression output screenshot
│   ├── residuals_preview.png               # Predictions & residuals preview
│   └── model_comparison_preview.png        # Comparison table preview
└── README.md                               # Project documentation (this file)
```

---

## 10. Screenshots

Below are previews of our regression outputs and workbook layouts:

### Simple Regression Output (Footfall Model)
![Simple Regression Output](file:///c:/Users/HP/OneDrive/Desktop/Assignment/part3_regression_insights/screenshots/simple_regression_output.png)

### Multiple Regression Output (Final Comprehensive Model)
![Multiple Regression Output](file:///c:/Users/HP/OneDrive/Desktop/Assignment/part3_regression_insights/screenshots/multiple_regression_output.png)

### Predictions and Residuals Preview (Top 5 Positive & Negative Residuals)
![Residuals Preview](file:///c:/Users/HP/OneDrive/Desktop/Assignment/part3_regression_insights/screenshots/residuals_preview.png)

### Model Comparison Table Preview
![Model Comparison Preview](file:///c:/Users/HP/OneDrive/Desktop/Assignment/part3_regression_insights/screenshots/model_comparison_preview.png)
