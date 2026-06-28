# Final Recommendation — Drivers of Monthly Sales

Prepared for executive leadership. Every statement is grounded in the regression
analysis of `business_regression_data.xlsx` (320 store-month records, 80 stores,
Jan–Apr 2025). Dependent variable: **monthly_sales**.

---

## Executive Summary

Sales across the network are driven primarily by **how many customers walk in
(footfall)**, supported by **marketing investment** and **keeping products in stock
(inventory availability)**. Together with store format, these factors explain about
**82% of the variation in monthly sales** (Adjusted R² = 0.817), with typical
prediction error around **5%**.

Two widely-assumed levers do **not** hold up: **deeper discounting shows no reliable
sales benefit**, and a chunk of **marketing spend is not converting** in specific
stores. Leadership should double down on footfall, marketing efficiency and stock
availability, and stop treating discount depth as a growth lever.

---

## Most Influential Drivers (ranked by evidence)

1. **Footfall** — strongest driver. +1 visit ≈ **₹33.7 in sales** (p<0.001).
   Alone it explains ~74% of sales variation.
2. **Inventory Availability %** — +1 point of availability ≈ **₹3,001 in sales**
   (p<0.001). A controllable, high-leverage operational lever.
3. **Marketing Spend** — +₹1 ≈ **₹1.17 in sales** (p<0.001), *on top of* its effect
   on footfall. Pays back, but efficiency varies sharply by store.
4. **Store Format** — vs an identical High-Street store: Airport **+₹21,906**
   (p=0.02), Residential **−₹16,576** (p=0.007). Format sets the baseline.

## Variables Leadership Should Prioritise
- **Footfall-generating activity** (location, catchment, local awareness, in-store experience).
- **Inventory availability** — protect and raise it; it is reliably and positively tied to sales.
- **Marketing — but on a return basis**, not raw spend (see risks below).

## Variables That Should NOT Be Over-interpreted
- **Avg Discount % (p = 0.11, not significant):** no statistical evidence that deeper
  discounts lift sales. The negative coefficient is **not** proof discounts hurt —
  it is simply **not reliable**. Do not build strategy on it.
- **Store Type = Mall (p = 0.08, borderline):** the Mall premium is suggestive but
  not firmly established; treat as a hypothesis, not a fact.
- **The intercept** is a mathematical anchor, not a "store with zero everything."

---

## Recommended Business Actions

**Marketing**
- Shift from "spend more" to "spend efficiently." The model values each ₹1 at ₹1.17
  *on average*, but the worst over-predicting stores (e.g. STR-1017, STR-1012) carry
  **very high spend that produced no matching sales**. Audit high-spend, low-conversion
  stores and reallocate budget to stores where marketing converts.

**Inventory**
- Treat availability as a revenue lever, not just an ops metric. Each point of
  availability ≈ ₹3,001 in sales. Prioritise replenishment and demand forecasting
  for high-footfall stores where stockouts cost the most.

**Discount strategy**
- **Stop using discount depth as a growth tactic.** It shows no significant sales
  lift while compressing margin. Test targeted/conditional promotions instead and
  measure them explicitly rather than discounting across the board.

**Staff allocation**
- Footfall is the master driver, so align staffing to traffic. Several
  over-predicting stores look like conversion failures (traffic present, sales
  short) — a sign of under-staffing or poor service at peak. Match staff to footfall
  patterns in those locations. *(Note: staff_count correlates with footfall and was
  kept out of the final model to avoid redundancy; manage it via footfall.)*

**Store prioritisation**
- Protect and expand **Airport** format economics (structural premium of ~₹22k/store).
- Investigate **South and West** stores that beat the model (under-predicted) — they
  hold replicable local strengths.
- Diagnose the **East** region's mild shortfall and the specific large over-predictors.

---

## Business Risks & Model Limitations

- **Association, not causation.** Regression shows variables *move together*; it does
  **not prove** that increasing one *causes* sales to rise. Footfall and sales are
  strongly associated — but a confounder (e.g. store quality) could drive both. Treat
  recommendations as well-evidenced hypotheses to test, not guarantees.
- **Short window.** Only 4 months (Jan–Apr 2025): seasonality and longer trends are not captured.
- **Outliers.** A few extreme marketing-spend values (up to ₹172k) distort the
  marketing relationship; they were retained but flagged.
- **Unmodelled factors.** Local competition, pricing, weather, events and management
  quality are not in the data and surface as residuals.
- **Region not yet modelled.** A clear regional residual pattern remains (South/West
  hot, East cool) — adding region dummies is the recommended next improvement.
- **Imputation.** 6 competitor-distance and 8 customer-rating values were median-imputed;
  neither is in the final model, so impact is minimal.

---

## Bottom Line
Grow footfall, keep shelves stocked, and spend marketing where it converts —
those three are the evidence-backed levers. Retire across-the-board discounting as a
sales tactic. Validate the biggest causal claims with a controlled test before
committing large budget.
