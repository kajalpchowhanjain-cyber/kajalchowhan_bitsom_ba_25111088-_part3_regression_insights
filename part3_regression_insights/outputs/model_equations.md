# Regression Model Equations & Interpretation

This document outlines the mathematical equations for the regression models developed to analyze monthly sales performance. It explains each coefficient in plain business language and describes the dummy variable coding scheme.

---

## 1. Simple Regression Equations

We ran two simple linear regression models to analyze the individual impact of key drivers on monthly sales.

### Model 1: Marketing Spend
This model evaluates the direct association between monthly marketing spend and monthly sales.

$$\text{Monthly Sales} = 560,777.35 + 2.13 \times \text{Marketing Spend}$$

* **Intercept (\$560,777.35):** The baseline monthly sales a store is expected to generate if marketing spend is zero. (Note: While marketing is rarely zero, this intercept serves as the mathematical starting point).
* **Coefficient of Marketing Spend (2.13):** For every **\$1.00 increase** in monthly marketing spend, monthly sales are associated with an increase of **\$2.13**. This indicates a positive return on marketing investment (a gross return of 213%).
* **Significance:** Highly statistically significant ($p = 2.48 \times 10^{-14}$), indicating that this relationship is very unlikely to be due to random chance.

### Model 2: Store Footfall
This model evaluates the direct association between visitor counts (footfall) and monthly sales.

$$\text{Monthly Sales} = 446,410.58 + 35.68 \times \text{Footfall}$$

* **Intercept (\$446,410.58):** The baseline monthly sales expected if footfall is zero. (In reality, a store with zero footfall would have zero sales; this high intercept reflects the fact that we are extrapolating outside our data range, as all stores have substantial baseline traffic).
* **Coefficient of Footfall (35.68):** For every **additional visitor** (footfall) entering the store, monthly sales are associated with an increase of **\$35.68**. This represents the average sales yield per visitor.
* **Significance:** Extremely significant ($p = 4.75 \times 10^{-94}$), explaining 73.63% of the variation in monthly sales on its own.

---

## 2. Multiple Regression Equation (Selected Final Model)

Our selected final model is **Multiple Regression Model 2 (Comprehensive)**. It includes 8 numerical predictors and 2 sets of dummy variables to control for store type and region, explaining **85.70%** of the variance in monthly sales.

### Mathematical Equation:

$$\begin{aligned}
\text{Monthly Sales} = & \; 86,319.14 \\
& + 1.21 \times \text{Marketing Spend} \\
& + 27.34 \times \text{Footfall} \\
& - 41,243.15 \times \text{Avg Discount Pct} \\
& + 3,508.40 \times \text{Staff Count} \\
& + 3,062.21 \times \text{Inventory Availability Pct} \\
& - 3,438.11 \times \text{Competitor Distance KM} \\
& + 15,157.29 \times \text{Holiday Flag} \\
& + 12,509.46 \times \text{Customer Rating} \\
& + 9,948.71 \times \text{Region\_North} \\
& + 21,089.57 \times \text{Region\_South} \\
& + 25,291.42 \times \text{Region\_West} \\
& - 24,475.84 \times \text{StoreType\_High Street} \\
& - 11,862.24 \times \text{StoreType\_Mall} \\
& - 44,695.09 \times \text{StoreType\_Residential}
\end{aligned}$$

---

## 3. Explanation of Dummy Variables & Reference Categories

Categorical variables (such as region and store type) cannot be entered directly into a regression model. They must be converted into binary dummy variables (0 or 1). 

To avoid the **dummy variable trap** (perfect multicollinearity, where one category can be perfectly predicted by the others), we must omit one category from each group. This omitted category serves as the **reference category** (or baseline), and the coefficients of the other categories are interpreted as relative differences compared to this baseline.

### A. Region Dummies (Reference Category: East)
* **Reference Category:** **East**
* **Dummies Created:** `Region_North`, `Region_South`, `Region_West`
* **Interpretation:**
  * **`Region_North` (+1.51 t-stat, $p = 0.131$):** North stores generate \$9,948.71 *more* in monthly sales than East stores, but this difference is **not statistically significant** ($p > 0.05$). The difference could be due to random variation.
  * **`Region_South` (+3.18 t-stat, $p = 0.0016$):** South stores generate **\$21,089.57 more** in monthly sales than East stores, which is highly statistically significant.
  * **`Region_West` (+4.29 t-stat, $p = 2.43 \times 10^{-5}$):** West stores generate **\$25,291.42 more** in monthly sales than East stores, which is extremely statistically significant.

### B. Store Type Dummies (Reference Category: Airport)
* **Reference Category:** **Airport**
* **Dummies Created:** `StoreType_High Street`, `StoreType_Mall`, `StoreType_Residential`
* **Interpretation:**
  * **`StoreType_High Street` (-2.84 t-stat, $p = 0.0048$):** High Street stores generate **\$24,475.84 less** in monthly sales than Airport stores, which is highly statistically significant.
  * **`StoreType_Mall` (-1.32 t-stat, $p = 0.187$):** Mall stores generate \$11,862.24 *less* in monthly sales than Airport stores, but this difference is **not statistically significant** ($p > 0.05$).
  * **`StoreType_Residential` (-5.11 t-stat, $p = 5.73 \times 10^{-7}$):** Residential stores generate **\$44,695.09 less** in monthly sales than Airport stores, which is extremely statistically significant.

---

## 4. Explanation of Numerical Coefficients (Business Language)

* **Intercept (\$86,319.14, $p = 0.0556$):** The baseline sales of a store that is located in the East region, is of Airport type, and has zero marketing, zero footfall, zero staff, zero inventory, and zero rating. This baseline is marginally significant, serving as the constant adjustment.
* **Marketing Spend (\$1.21, $p < 0.001$):** Every **\$1.00 increase** in marketing spend is associated with a **\$1.21 increase** in monthly sales, holding all other variables constant. This indicates a positive return on advertising investment, even after controlling for footfall.
* **Footfall (\$27.34, $p < 0.001$):** Every **additional visitor** yields **\$27.34** in monthly sales. This highlights footfall conversion as a primary business engine.
* **Average Discount Percentage (-\$41,243.15, $p = 0.224$):** A 1% increase in the average discount rate is associated with a **\$412.43 decrease** in sales. However, this variable is **statistically weak** ($p > 0.05$). Once other factors (like inventory, staff, and ratings) are accounted for, discount strategy does not have a reliable direct impact on total sales revenue.
* **Staff Count (\$3,508.40, $p = 0.0027$):** Each **additional staff member** is associated with a **\$3,508.40 increase** in monthly sales. This indicates that staffing levels drive customer assistance and conversions.
* **Inventory Availability (\$3,062.21, $p < 0.001$):** Every **1% increase** in inventory availability is associated with a **\$3,062.21 increase** in monthly sales. Keeping stock on shelves is highly profitable.
* **Competitor Distance KM (-\$3,438.11, $p < 0.001$):** For every **1 km increase** in distance from the nearest competitor, monthly sales **decrease by \$3,438.11**. In retail, being closer to competitors (clustering/agglomeration) is associated with higher sales because competitors cluster in high-traffic shopping hubs.
* **Holiday Flag (\$15,157.29, $p = 0.0139$):** Months with major holiday periods see an average sales bump of **\$15,157.29**, representing seasonal shopping lifts.
* **Customer Rating (\$12,509.46, $p = 0.0052$):** A **1-point increase** in customer rating is associated with a **\$12,509.46 increase** in monthly sales, showing a strong link between customer satisfaction and revenue.

---

## 5. Final Model Selection & Rationale

We selected **Multiple Model 2 (Final Comprehensive Model)** for the following reasons:
1. **Highest Explanatory Power ($R^2 = 85.70\%$):** It captures the bulk of the drivers of monthly sales, leaving only 14.30% unexplained.
2. **Actionable Operational Levers:** It includes actionable variables (staff count, inventory availability, customer rating) that store managers can control, unlike simple models that look only at footfall or marketing.
3. **Statistical Validity:** The low Residual Standard Error (\$40,133.89) and high overall significance ($F\text{-sig} = 5.13 \times 10^{-120}$) confirm that the model's coefficients are highly reliable.
