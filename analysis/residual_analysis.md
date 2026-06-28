# Residual Analysis

Residuals are taken from the **Multiple Regression** model (the recommended model),
computed on all 320 store-month records.

- **Predicted Sales** = model estimate of monthly_sales
- **Residual** = Actual − Predicted (positive ⇒ store beat the model; negative ⇒ store fell short)
- **Absolute Residual** = |Residual|
- **Residual %** = Residual ÷ Actual

**Overall accuracy:** Mean Absolute Error ≈ **₹34,009**; MAPE ≈ **5.1%** of average
sales (₹680,629). The model is accurate on average, with a handful of stores it
explains poorly — those are the interesting cases below.

---

## Top 5 Positive Residuals — model UNDER-predicts (stores out-performing the model)

| Store | Month | Region | Store Type | Actual | Predicted | Residual | % |
|-------|-------|--------|------------|-------:|----------:|---------:|---:|
| STR-1030 | 2025-02 | West | Residential | 820,519 | 714,620 | **+105,899** | +12.9% |
| STR-1073 | 2025-03 | East | Residential | 813,317 | 713,693 | **+99,623** | +12.3% |
| STR-1032 | 2025-01 | South | High Street | 914,544 | 815,773 | **+98,771** | +10.8% |
| STR-1050 | 2025-04 | North | Residential | 735,787 | 638,540 | **+97,246** | +13.2% |
| STR-1028 | 2025-04 | East | Mall | 713,611 | 618,466 | **+95,145** | +13.3% |

**Possible business reasons:** these stores sold ~11–13% more than their drivers
predict. Likely explanations not captured by the model: strong local catchment or
loyalty, a successful local promotion or event, an unusually effective manager, or
a temporary competitor closure nearby. Three of the five are Residential — a format
the model penalises on average, yet these locations beat it, suggesting pockets of
strong residential demand worth studying.

## Top 5 Negative Residuals — model OVER-predicts (stores under-performing the model)

| Store | Month | Region | Store Type | Actual | Predicted | Residual | % |
|-------|-------|--------|------------|-------:|----------:|---------:|---:|
| STR-1017 | 2025-03 | West | High Street | 685,379 | 844,620 | **−159,241** | −23.2% |
| STR-1023 | 2025-02 | South | Mall | 627,172 | 755,437 | **−128,265** | −20.5% |
| STR-1012 | 2025-01 | West | Residential | 595,468 | 716,546 | **−121,079** | −20.3% |
| STR-1007 | 2025-02 | West | Mall | 800,452 | 900,272 | **−99,821** | −12.5% |
| STR-1009 | 2025-01 | North | Residential | 641,865 | 740,315 | **−98,450** | −15.3% |

**Possible business reasons:** these stores sold 12–23% *less* than their inputs
imply. Common drivers of over-prediction: high marketing/footfall that didn't
convert (operational issues, long queues, poor staffing-to-traffic match), strong
local competition, supply or display problems, or one-off disruptions
(renovation, weather, local event). STR-1017 and STR-1012 both carry **very high
marketing_spend outliers** (₹172k and ₹139k) — the model credits that spend with
sales that never materialised, a classic sign of inefficient marketing.

---

## Underprediction vs Overprediction

- **Underprediction** (positive residuals): the model is conservative for a cluster
  of Residential and South/West stores — real demand exceeds what footfall, marketing,
  inventory and format explain. These are *upside* stories: replicable local strengths.
- **Overprediction** (negative residuals): concentrated where marketing spend is very
  high but conversion is weak, and in some West/Mall locations — *efficiency* problems:
  inputs are present but not translating into sales.

---

## Patterns by Region

Average residual by region (Actual − Predicted):

| Region | Mean residual | Reading |
|--------|--------------:|---------|
| South | **+7,948** | Model slightly **under-predicts** — South stores beat their drivers. |
| West | **+7,883** | Also under-predicted on average, but West holds the biggest individual misses (both directions). |
| North | −2,012 | Roughly on target. |
| East | **−10,321** | Model slightly **over-predicts** — East stores fall a little short of their drivers. |

A region effect is visible (South/West run hot, East runs cool). Adding `region`
as a second set of dummies could lift the model further — a clear next step.

## Patterns by Store Type

Average residual by store type is essentially **0 for every type** (≈₹0). This is
expected and reassuring: because store_type is *already in the model* as dummies,
the model fits each format's average exactly. Store-type-specific bias has been
absorbed; the remaining error is within-format (individual locations), not
between-format.

---

## Takeaways

1. The model is accurate on average (~5% MAPE) but misses a small set of stores by 12–23%.
2. The biggest over-predictions coincide with very high marketing spend that didn't
   convert — flag these for a marketing-efficiency review.
3. A regional signal remains (South/West under-predicted, East over-predicted) —
   adding region dummies is the most promising model improvement.
4. Store-type bias is fully captured; no format is systematically mis-modelled.
