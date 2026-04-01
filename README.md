# Supply-Chain-Optimization (Performance Analysis)

End-to-end analysis of supply chain bottlenecks with the goal of cutting logistics costs and improving delivery reliability. The work covers data cleaning, exploratory analysis, statistical hypothesis testing, and a bootstrap simulation of potential savings.

**Stack:** Python · pandas · numpy · scipy · matplotlib · seaborn

---

## What's inside

**1. Data Preparation**  
Loading data from `supply_chain_v2.xlsx`, fixing date types, recalculating lead time as `Shipping times + Manufacturing lead time` (the source column was incorrect), and checking for nulls and duplicates.

**2. Exploratory Analysis**  
Distribution plots and boxplots for price, revenue, shipping cost, manufacturing cost, defect rate, and lead time. Supplier-level revenue and profit breakdown. Carrier-level defect rate comparison. Correlation heatmap across all key metrics.

**3. Sales & Customer Segment Analysis**  
Revenue breakdown by customer demographic group to support prioritization decisions.

**4. Operational Efficiency**  
- Monthly lead time trend (2023–2024)  
- Mann-Whitney U test: shipping times and costs year-over-year  
- Lead time and cost by route, transport mode, and carrier  
- Pivot tables: carrier × transport mode × route

**5. Hypothesis — Air vs. Sea Shipping**  
The descriptive stats showed that Air and Sea have nearly identical lead times despite a large cost gap. A Mann-Whitney U test confirmed the difference is not statistically significant (p = 0.68). This became the basis for the cost optimization scenario.

**6. Scenario Simulation**  
Bootstrap simulation (10,000 iterations): shift 75% of Air shipments to Sea.  
Result: **median savings ~4,279 USD** per period, 95% CI: 4,044 – 4,501 USD.

---

## Key findings

| Area | Finding |
|------|---------|
| Transportation | Air and Sea shipping times are statistically identical, but Air costs ~3.5× more (36.6 vs 10.6 USD avg) |
| Cost simulation | Shifting 75% of Air to Sea → ~4,279 USD median savings (bootstrapped 95% CI) |
| Product quality | Electronics and technics categories show the highest defect rates across all suppliers |
| Correlations | Manufacturing costs positively correlated with defect rate — complex products show higher quality variance |
| Seasonality | Lead times are stable across 2023–2024; no significant year-over-year shift detected |
