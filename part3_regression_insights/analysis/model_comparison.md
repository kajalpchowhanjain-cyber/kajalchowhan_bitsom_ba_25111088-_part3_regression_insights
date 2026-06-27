# Model Comparison Analysis

This document provides a detailed comparison between the simple linear regression models and multiple linear regression models developed to analyze monthly sales performance across the retail store network.

## Executive Summary

Regression analysis was conducted to isolate the key drivers of store sales. We evaluated two simple regression models and two multiple regression models to understand how specific store features, marketing spend, and operational metrics impact performance. The models demonstrate that while simple models (particularly footfall) provide high explanatory power, a comprehensive multiple regression model controls for confounding factors and provides actionable levers for store operations and leadership decisions.

---

## Comparative Model Matrix

The table below summarizes the key statistical and business attributes of each regression model:

| Attribute / Metric | Simple Model 1 (Marketing Spend) | Simple Model 2 (Footfall) | Multiple Model 1 (Intermediate) | Multiple Model 2 (Final Comprehensive) |
| :--- | :--- | :--- | :--- | :--- |
| **Model Type** | Simple Linear Regression | Simple Linear Regression | Multiple Linear Regression | Multiple Linear Regression |
| **Dependent Variable** | `monthly_sales` | `monthly_sales` | `monthly_sales` | `monthly_sales` |
| **Independent Variables** | `marketing_spend` | `footfall` | `marketing_spend`, `footfall`, `avg_discount_pct`, `store_type` (3 dummies) | `marketing_spend`, `footfall`, `avg_discount_pct`, `staff_count`, `inventory_availability_pct`, `competitor_distance_km`, `holiday_flag`, `customer_rating`, `region` (3 dummies), `store_type` (3 dummies) |
| **Predictors ($k$)** | 1 | 1 | 6 | 14 |
| **Observations ($n$)** | 320 | 320 | 320 | 320 |
| **Multiple R** | 0.4090 | 0.8581 | 0.8932 | 0.9257 |
| **R-squared ($R^2$)** | 16.72% | 73.63% | 79.77% | 85.70% |
| **Adjusted $R^2$** | 16.46% | 73.55% | 79.38% | 85.04% |
| **Residual Std Error ($S$)** | \$94,846.03 | \$53,373.74 | \$47,118.41 | \$40,133.89 |
| **F-Statistic** | 63.86 | 887.85 | 205.86 | 130.41 |
| **Significance F** | $2.48 \times 10^{-14}$ | $4.75 \times 10^{-94}$ | $3.57 \times 10^{-96}$ | $5.13 \times 10^{-120}$ |
| **Significant Variables ($p < 0.05$)** | `marketing_spend` | `footfall` | `marketing_spend`, `footfall`, `avg_discount_pct`, `store_type_Residential` | `marketing_spend`, `footfall`, `staff_count`, `inventory_availability_pct`, `competitor_distance_km`, `holiday_flag`, `customer_rating`, `region_South`, `region_West`, `store_type_High Street`, `store_type_Residential` |
| **Weak Variables ($p \ge 0.05$)** | None | None | `store_type_High Street`, `store_type_Mall` | `avg_discount_pct`, `region_North`, `store_type_Mall` |

---

## Detailed Model Evaluations

### 1. Simple Model 1: Monthly Sales vs. Marketing Spend
* **Independent Variable:** `marketing_spend`
* **Explanatory Power ($R^2$):** 16.72%
* **Statistical Significance:** The predictor is highly significant ($p = 2.48 \times 10^{-14}$), suggesting that marketing spend has a reliable positive impact on sales.
* **Business Usefulness:** Low to Moderate. While it proves that marketing investments yield sales increases (specifically, \$2.13 in sales for every \$1.00 spent on marketing), it leaves 83.28% of the sales variance unexplained.
* **Limitations:** The model suffers from omitted variable bias. It assumes marketing is the sole driver of sales, ignoring footfall, store location, size, competitor presence, and staff availability.

### 2. Simple Model 2: Monthly Sales vs. Footfall
* **Independent Variable:** `footfall`
* **Explanatory Power ($R^2$):** 73.63%
* **Statistical Significance:** Highly significant ($p = 4.75 \times 10^{-94}$). Footfall alone is a massive driver of monthly sales volume.
* **Business Usefulness:** Moderate. It tells leadership that bringing customers into stores is strongly associated with sales revenue (roughly \$35.68 per visitor). However, "footfall" is a lagging or intermediate metric; leadership cannot directly "increase footfall" without understanding *what* drives footfall (such as marketing, location, or store type).
* **Limitations:** Does not control for operational levers (e.g., inventory availability, staff count, discounts). It also doesn't isolate the effect of marketing from footfall.

### 3. Multiple Model 1 (Intermediate Model)
* **Independent Variables:** `marketing_spend`, `footfall`, `avg_discount_pct`, and `store_type` (Reference: Airport)
* **Explanatory Power ($R^2$):** 79.77%
* **Statistical Significance:** The overall model is highly significant ($F$-sig $= 3.57 \times 10^{-96}$). `marketing_spend`, `footfall`, and `avg_discount_pct` are statistically significant. For `store_type`, only `Residential` is statistically significant ($p = 0.0005$), while `High Street` is borderline ($p = 0.056$) and `Mall` is weak ($p = 0.348$) compared to Airport stores.
* **Business Usefulness:** High. It reveals that once we control for footfall, the coefficient of marketing spend drops to \$1.13, showing that part of marketing's effect is mediated through footfall. It also shows a significant negative effect of average discount percentage (-\$78,421.28 for a 100% discount, or -\$784.21 for every 1% increase in discount rate), indicating that discounting might erode sales margins if it doesn't sufficiently drive footfall.
* **Limitations:** It still ignores key operational variables like inventory levels, staff presence, competitor distance, and region-specific differences, which are critical for management action.

### 4. Multiple Model 2 (Final Comprehensive Model)
* **Independent Variables:** 8 numerical variables (including 1 binary flag) and 2 sets of dummy variables (Region and Store Type).
* **Explanatory Power ($R^2$):** 85.70% (Adjusted $R^2 = 85.04\%$)
* **Statistical Significance:** Extremely high overall significance ($F$-sig $= 5.13 \times 10^{-120}$). Almost all included operational metrics (`staff_count`, `inventory_availability_pct`, `competitor_distance_km`, `holiday_flag`, `customer_rating`) and region/store type dummies are highly significant ($p < 0.05$).
* **Business Usefulness:** Outstanding. This model provides leadership with a comprehensive view of operational, marketing, and locational drivers. It isolates the impact of each variable while controlling for others:
  - **Marketing Spend** contributes \$1.21 in sales per \$1.00 spent.
  - **Inventory Availability** is critical: every 1% increase in availability yields \$3,062.21 in sales.
  - **Customer Rating** shows a \$12,509.46 sales boost for each 1-point increase.
  - **Staff Count** yields \$3,508.40 in sales per additional employee.
  - **Agglomeration Effect:** Stores closer to competitors have *higher* sales (sales decrease by \$3,438.11 for every 1 km increase in competitor distance).
  - **Discounts** are statistically weak ($p = 0.224$) in this model, suggesting that discounting does not have a reliable direct effect on sales revenue once inventory, service, and footfall are accounted for.
* **Limitations:** Although the model explains 85.70% of the variance, it does not capture qualitative factors such as manager capability, local economic shocks, marketing campaign creativity, or store aesthetics.

---

## Selection of the Final Model

We select **Multiple Model 2 (Final Comprehensive Model)** as our primary model for business insights and recommendations.

### Rationale:
1. **Highest Explanatory Power:** It explains **85.70%** of the variance in monthly sales, compared to 79.77% for Multiple Model 1, and 73.63% or 16.72% for the simple models.
2. **Lowest Prediction Error:** The Residual Standard Error ($S$) is the lowest at **\$40,133.89**, indicating that its predictions are on average closer to actual sales than any other model.
3. **Controls for Operational Variables:** It incorporates actionable operational levers (staffing, stock availability, customer rating) which are under the direct control of store management, enabling specific, tactical recommendations.
4. **Isolates Confounding Factors:** It successfully separates regional and store-format differences from operational performance, ensuring that recommendations are not biased by store type or region.
