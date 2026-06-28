# Part 3 — Regression Insights: Drivers of Monthly Store Sales

Regression analysis of an 80-store retail network to identify and quantify what
drives **monthly_sales**, and to translate the statistics into executive guidance.
Every number in this repository is computed directly from the dataset — nothing is
fabricated.

---

## Business Problem Summary
Leadership wants to know **which factors actually move monthly sales**, how strong
each is, and where to focus marketing, inventory, discounting and staffing. The goal
is a defensible, evidence-based set of priorities — separating real drivers from
assumptions that don't hold up.

## Dataset Description
- **File:** `data/business_regression_data.xlsx` (sheet `store_performance`)
- **Size:** 320 rows × 14 columns
- **Grain:** one row per **store × month** (80 stores × 4 months, Jan–Apr 2025)

### Dependent Variable
- **monthly_sales** — total monthly sales per store (currency units, shown as ₹).

### Independent Variables (candidates)
| Type | Variables |
|------|-----------|
| Numeric | marketing_spend, footfall, avg_discount_pct, staff_count, inventory_availability_pct, competitor_distance_km, customer_rating |
| Categorical | region (East/North/South/West), store_type (High Street/Residential/Mall/Airport) |
| Binary | holiday_flag |
| Identifiers / not predictors | store_id, month |
| Outcome-derived (excluded) | monthly_profit (a *result* of sales — would leak the target) |

**Variables unsuitable for regression and why:** `store_id` (identifier, no signal),
`month` (date label, only 4 values), and `monthly_profit` (derived from sales —
including it leaks the outcome). `staff_count` is valid but highly correlated with
footfall, so it was left out of the final model to avoid redundancy.

## Cleaning Performed
- **Missing values:** `competitor_distance_km` (6) and `customer_rating` (8) imputed
  with **group medians** (by region and store_type respectively). 0 missing after.
- **Duplicates:** none found.
- **Outliers (1.5×IQR):** flagged — marketing_spend (6), competitor_distance_km (15),
  inventory_availability_pct (7), staff_count (1), monthly_profit (1). **Retained**
  as plausible real values, not deleted.
- **Data types:** validated — `month` as date, ids/region/store_type as text, the rest numeric.
- The **original sheet is preserved unchanged**; all work is on a copy.

## Dummy Variable Approach
- Categorical encoded: **store_type**.
- **Reference = High Street** (most frequent level, 116/320 rows → most stable baseline).
- Three dummies created (Airport, Mall, Residential); High Street omitted to avoid
  the **dummy-variable trap**. Each dummy coefficient = sales difference vs an
  identical High-Street store.

## Regression Methodology
Ordinary Least Squares (OLS) via `statsmodels`. Two simple regressions and one
multiple regression on `monthly_sales`. For each model: equation, coefficients,
intercept, R²/Adjusted R², p-values, F-statistic, 95% confidence intervals, and
multicollinearity (VIF) for the multiple model. Predictions and residuals computed
for all 320 records.

## Simple Regression Summary
| Model | Predictor | Equation | R² | Significant? |
|-------|-----------|----------|----:|--------------|
| Simple 1 | footfall | Sales = 446,411 + 35.68·Footfall | 0.736 | Yes (p<0.001) |
| Simple 2 | marketing_spend | Sales = 560,777 + 2.13·Marketing | 0.167 | Yes (p<0.001) |

Footfall is the strongest single predictor; marketing alone is significant but weak.

## Multiple Regression Summary
```
Sales = 136,914 + 33.66·Footfall + 1.17·Marketing − 58,921·Discount%
        + 3,001·Inventory% + 21,906·Airport + 11,433·Mall − 16,576·Residential
```
- **Adjusted R² = 0.817**, **F ≈ 205 (p<0.001)**, **MAPE ≈ 5.1%**, all **VIF ≈ 1**.
- **Significant & positive:** footfall, marketing_spend, inventory_availability_pct,
  Airport, (Residential significant & negative).
- **Not significant:** avg_discount_pct (p=0.11); Mall borderline (p=0.08).

## Residual Analysis Summary
- Accurate on average (MAE ≈ ₹34,009); a few stores miss by 12–23%.
- **Under-predicted** (out-performers): cluster of Residential and South/West stores.
- **Over-predicted** (under-performers): stores with very high marketing spend that
  didn't convert (e.g. STR-1017, STR-1012).
- **By region:** South/West run hot (+), East runs cool (−) → adding region dummies
  is the top improvement. **By store_type:** ≈0 bias (already modelled).

## Model Comparison Summary
| Model | Adj. R² | # significant drivers | Recommended |
|-------|--------:|----------------------:|:-----------:|
| Simple 1 (footfall) | 0.736 | 1 | No |
| Simple 2 (marketing) | 0.165 | 1 | No |
| **Multiple** | **0.817** | **5** | **✓ Yes** |

## Final Selected Model
**Multiple Regression** — best fit, multiple genuine drivers, no multicollinearity,
~5% typical error.

## Business Recommendation
Grow **footfall**, protect **inventory availability**, and spend **marketing where it
converts**. **Stop using discount depth as a growth lever** (no significant effect).
Protect Airport-format economics; study South/West over-performers; diagnose East
shortfall. Full detail in `outputs/final_recommendation.md`.

## Assumptions
- OLS assumptions (linearity, independent errors, roughly constant variance) hold
  well enough for guidance-level use.
- Median imputation is reasonable for the two partially-missing columns.
- 4 months is treated as representative for cross-sectional drivers (not for seasonality).

## Limitations
- **Association ≠ causation** — these are evidence-backed hypotheses, not proof.
- Short time window (no seasonality/trend); a few extreme spend outliers; unmodelled
  factors (competition, pricing, weather, management) appear as residuals; region not
  yet modelled.

## Repository Structure
```
part3_regression_insights/
├── data/
│   └── business_regression_data.xlsx      (original, preserved)
├── analysis/
│   ├── regression_workbook.xlsx           (8 sheets, live formulas)
│   ├── model_comparison.md
│   ├── residual_analysis.md
├── outputs/
│   ├── model_equations.md
│   ├── final_recommendation.md
│   └── regression_summary.xlsx             (clean comparison table)
├── screenshots/
│   ├── simple_regression_output.png
│   ├── multiple_regression_output.png
│   ├── residuals_preview.png
│   └── model_comparison_preview.png
└── README.md
```

## Screenshots Included
- `simple_regression_output.png` — both simple-regression results side by side.
- `multiple_regression_output.png` — full multiple-regression coefficient table.
- `residuals_preview.png` — top positive/negative residuals + region pattern.
- `model_comparison_preview.png` — model comparison summary.

