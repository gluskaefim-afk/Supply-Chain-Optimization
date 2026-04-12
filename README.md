# Supply-Chain-Optimization (Performance Analysis)

End-to-end analysis of supply chain bottlenecks with the goal of cutting logistics costs and improving delivery reliability. The work covers data cleaning, exploratory analysis, statistical hypothesis testing, and a bootstrap simulation of potential savings.

**Stack:** Python · pandas · numpy · scipy · matplotlib · seaborn

---

## What's inside

### 1. Data Preparation
Loading data from `supply_chain_v2.xlsx`, fixing date types, recalculating lead time as `Shipping times + Manufacturing lead time` (the source column was incorrect), and checking for nulls and duplicates.

### 2. Exploratory Analysis
Distribution plots and boxplots for price, revenue, shipping cost, manufacturing cost, defect rate, and lead time. Supplier-level revenue, profit breakdown and defect rate comparison. Carrier-level defect rate comparison. Correlation heatmap across all key metrics.

### 3. Sales & Customer Segment Analysis
Revenue breakdown by customer demographic group to support prioritization decisions.

### 4. Operational Efficiency
- Monthly lead time trend (2023–2024)
- T-test: shipping times and costs year-over-year (normality verified via Q-Q plots)
- ANOVA: manufacturing lead time and shipping times across 10 supplier locations
- Lead time and cost by route, transport mode, and carrier
- Pivot tables: carrier × transport mode × route

### 5. Hypothesis — Air vs. Sea Shipping
Descriptive stats showed that Air and Sea have nearly identical lead times and shipping times despite a large cost gap. Normality was verified via Q-Q plots; a T-test confirmed the difference is not statistically significant (p = 0.69). This became the basis for the cost optimization scenario.

### 6. Scenario Simulation
Bootstrap simulation (10,000 iterations): shift 75% of Air shipments to Sea. Both the baseline and the post-shift scenario are resampled on each iteration to correctly capture uncertainty in the savings estimate.

Result: **median savings ~4,281 USD** per period, 95% CI: 3,877 – 4,684 USD.

---

## Key findings

| Area | Finding |
|------|---------|
| 🚢 Transportation | Air and Sea shipping times are statistically identical, but Air costs ~3.5× more (36.6 vs 10.6 USD avg) |
| 💰 Cost simulation | Shifting 75% of Air to Sea → ~4,281 USD median savings (bootstrapped 95% CI: 3,877 – 4,684 USD) |
| ⚠️ Product quality | Electronics-related suppliers (ElectoMart, DigiParts Ltd, TechSource Asia) show the highest defect rates |
| 📈 Correlations | Manufacturing costs positively correlated with defect rate — complex products show higher quality variance |
| 📅 Stability | Lead times and shipping costs are stable across 2023–2024; no significant year-over-year shift detected |
