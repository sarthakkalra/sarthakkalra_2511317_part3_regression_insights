# Model Equations & Coefficient Interpretation

All figures below are computed directly from `business_regression_data.xlsx`
(320 store-month records, 80 stores × 4 months, Jan–Apr 2025).
Dependent variable throughout: **monthly_sales** (currency units, shown as ₹).

---

## 1. Dummy-Variable Design — Store Type

`store_type` was chosen as the categorical feature to encode because it shows a
clear, business-meaningful relationship with sales and has a balanced spread of
records across its four levels.

| Store Type   | Records | Role in model        |
|--------------|--------:|----------------------|
| High Street  | 116     | **Reference (omitted)** |
| Residential  | 100     | Dummy `st_Residential` |
| Mall         | 76      | Dummy `st_Mall`        |
| Airport      | 28      | Dummy `st_Airport`     |

**Reference category = High Street.**

- **Why High Street was selected:** it is the most frequent level (116 of 320
  rows, ~36%), so it provides the largest, most stable baseline group. Every
  other store type's effect is then read as a clean comparison against a typical
  High Street store.
- **Why one category was omitted:** including a dummy for *all four* levels plus
  the intercept creates perfect collinearity (the four dummies always sum to 1,
  duplicating the intercept). This is the **dummy-variable trap**. Dropping one
  level — High Street — removes the redundancy. Three dummies fully describe four
  categories.

---

## 2. Simple Regression Equations

### Model 1 — Sales vs Footfall
```
Monthly Sales = 446,411 + 35.68 × Footfall
```
- **Intercept (446,411):** the model's baseline sales for a (hypothetical)
  store with zero footfall. Not literally meaningful — it anchors the line.
- **Footfall (+35.68):** each additional customer visit per month is associated
  with about **₹35.7 more in monthly sales**.
- **Fit:** R² = 0.736 — footfall alone explains ~74% of sales variation.
  Highly significant (p < 0.001, F ≈ 888).

### Model 2 — Sales vs Marketing Spend
```
Monthly Sales = 560,777 + 2.13 × Marketing Spend
```
- **Intercept (560,777):** baseline sales with zero marketing spend.
- **Marketing Spend (+2.13):** each ₹1 of marketing spend is associated with
  about **₹2.13 of additional sales** — a positive but much weaker signal on its own.
- **Fit:** R² = 0.167 — marketing spend alone explains only ~17% of variation.
  Significant (p < 0.001) but a weak standalone predictor.

---

## 3. Multiple Regression Equation (Final / Recommended Model)

```
Monthly Sales = 136,914
              + 33.66   × Footfall
              + 1.17    × Marketing Spend
              - 58,921  × Avg Discount %        (not significant)
              + 3,001   × Inventory Availability %
              + 21,906  × (Store Type = Airport)
              + 11,433  × (Store Type = Mall)    (borderline)
              - 16,576  × (Store Type = Residential)
```
(Store Type = High Street is the omitted reference; its effect is folded into the intercept.)

### Coefficient-by-coefficient interpretation

| Term | Coefficient | p-value | Plain-English meaning |
|------|------------:|--------:|------------------------|
| Intercept | 136,914 | 0.002 | Baseline for a High-Street store with all numeric drivers at zero — an anchor, not a real store. |
| Footfall | +33.66 | <0.001 | One extra visit ≈ **₹33.7 more sales**, holding everything else constant. The strongest controllable driver. |
| Marketing Spend | +1.17 | <0.001 | Each ₹1 of marketing ≈ **₹1.17 of sales**, after accounting for footfall — marketing still pays back independently. |
| Avg Discount % | −58,921 | **0.110** | **Not statistically significant.** No reliable evidence that deeper discounting raises sales here. Do not over-interpret. |
| Inventory Availability % | +3,001 | <0.001 | Each 1-percentage-point gain in stock availability ≈ **₹3,001 more sales** — keeping shelves full matters. |
| Store Type = Airport | +21,906 | 0.020 | An Airport store sells **₹21,906 more** than an otherwise-identical High-Street store. |
| Store Type = Mall | +11,433 | 0.083 | A Mall store sells ~₹11,433 more than High Street, but **borderline** — treat with caution. |
| Store Type = Residential | −16,576 | 0.007 | A Residential store sells **₹16,576 less** than an otherwise-identical High-Street store. |

### How to read the dummy variables
Each store-type coefficient is a **difference versus the High-Street reference**,
with all numeric drivers held equal. Positive = sells more than High Street,
negative = sells less. High Street itself has no separate term because it is the
baseline already captured by the intercept.

### Model quality
- **Adjusted R² = 0.817** — explains ~82% of sales variation, well above either
  simple model.
- **F-statistic ≈ 205, p < 0.001** — the model as a whole is highly significant.
- **All VIFs ≈ 1.0–1.3** — no multicollinearity, so each coefficient can be read independently.
- **Typical error:** MAE ≈ ₹34,009; MAPE ≈ 5.1% of average sales.

---

## 4. Final Selected Model

**The Multiple Regression model is selected.**

Why:
1. Highest explanatory power (Adjusted R² 0.817 vs 0.736 and 0.165 for the simple models).
2. Captures several independent, significant drivers at once instead of one.
3. No multicollinearity, so coefficients are stable and interpretable.
4. Low practical error (~5% MAPE).
5. Separates genuine drivers (footfall, marketing, inventory) from a non-effect
   (discount %), which is exactly the kind of guidance leadership needs.
