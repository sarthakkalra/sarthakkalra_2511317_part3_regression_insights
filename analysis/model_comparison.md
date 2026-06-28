# Model Comparison

Comparison of all three regression models built on `business_regression_data.xlsx`
(320 store-month records). Dependent variable: **monthly_sales**. All statistics
are computed directly from the dataset.

---

## Comparison Table

| Model | Dependent | Independent variables | R² | Adj. R² | Significant variables | Business interpretation |
|-------|-----------|------------------------|----:|-------:|------------------------|--------------------------|
| **Simple Reg 1** | monthly_sales | footfall | 0.736 | 0.736 | footfall (p<0.001) | More visitors → more sales. Strongest single predictor, but blind to everything else. |
| **Simple Reg 2** | monthly_sales | marketing_spend | 0.167 | 0.165 | marketing_spend (p<0.001) | Marketing helps, but explains little on its own. |
| **Multiple Reg** | monthly_sales | footfall, marketing_spend, avg_discount_pct, inventory_availability_pct, store_type (3 dummies) | 0.821 | 0.817 | footfall, marketing_spend, inventory_availability_pct, Airport, Residential | Footfall + marketing + stock availability + store format jointly drive sales. Best, most balanced view. |

---

## Detailed assessment

### Simple Regression 1 — Sales vs Footfall
- **Strengths:** explains ~74% of variation with one variable; intuitive; footfall
  is measurable and actionable.
- **Weaknesses:** ignores marketing, stock, store format; over-credits footfall
  for effects that really belong to other drivers.
- **Limitations:** single-variable models exaggerate one factor's apparent importance.

### Simple Regression 2 — Sales vs Marketing Spend
- **Strengths:** confirms marketing has a real, positive, significant association.
- **Weaknesses:** only ~17% of variation explained; weak standalone predictor.
- **Limitations:** spend levels vary for many reasons (a few very high-spend outliers),
  so the line is noisy.

### Multiple Regression — Sales vs 4 numeric drivers + Store Type
- **Strengths:** highest Adjusted R² (0.817); isolates the independent effect of each
  driver; no multicollinearity (all VIF ≈ 1); low error (MAPE ≈ 5.1%).
- **Weaknesses:** two terms are weak/borderline — avg_discount_pct (p=0.11, not
  significant) and st_Mall (p=0.08); these should not be over-interpreted.
- **Limitations:** observational data — shows association, not proof of causation;
  built on 4 months of data, so seasonality and longer-term trends are not captured.

---

## Recommended Model

**Multiple Regression** is recommended.

It offers the best fit (Adjusted R² 0.817), captures multiple genuine and significant
drivers simultaneously, has no multicollinearity, and predicts within ~5% on average.
The two simple models are useful for explaining single relationships but materially
under-fit the data and mislead by attributing the whole effect to one variable.

| Criterion | Simple 1 | Simple 2 | **Multiple** |
|-----------|:--------:|:--------:|:------------:|
| Adjusted R² | 0.736 | 0.165 | **0.817** |
| # significant drivers | 1 | 1 | **5** |
| Multicollinearity | n/a | n/a | **None (VIF≈1)** |
| Typical error (MAPE) | higher | highest | **~5.1%** |
| Recommended | No | No | **✓ Yes** |
