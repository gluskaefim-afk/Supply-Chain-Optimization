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
Revenue breakdown by customer demographic group. A Kruskal-Wallis test found no statistically significant revenue difference across age groups.

### 4. Operational Efficiency
- Monthly lead time trend (2023–2024)
- Normality checked with Q-Q plots for shipping times and shipping costs (2023 vs. 2024); both showed right-skew, so the **Mann-Whitney U test** (not a t-test) was used — no significant year-over-year change in either metric
- Manufacturing lead time and shipping times across 10 supplier locations: Q-Q plots showed outliers/non-normal distributions, so the **Kruskal-Wallis test** (not ANOVA) was used — no significant difference between locations on either metric
- Lead time and cost by route, transport mode, and carrier
- Pivot tables: carrier × transport mode × route

### 5. Hypothesis — Air vs. Sea Shipping
Descriptive stats showed Air and Sea have nearly identical lead times and shipping times despite a large cost gap. Q-Q plots showed non-normal distributions (outliers in Sea), so the **Mann-Whitney U test** was used: no significant difference in shipping time, but shipping cost differed significantly between modes. This became the basis for the cost optimization scenario.

> **Note on the data:** in a real-world dataset, Air and Sea having statistically indistinguishable delivery times would be very unlikely — aircraft don't normally match sea-freight speed. Since this dataset is AI-generated, the anomaly is treated here as a synthetic artifact, and the scenario below is a practice exercise in the analytical workflow rather than a claim about real air/sea physics.

### 6. Scenario Simulation
Bootstrap simulation (10,000 iterations): shift **100%** of Air shipments to Sea. Each iteration resamples the Sea cost distribution (with replacement) up to the combined Air+Sea observation count, and compares that resampled total to the actual combined cost.

Result: **median savings ~334,154 USD**, 95% CI: 264,186 – 381,422 USD.

> **Method note:** only the Sea-side cost distribution is resampled; the actual (Air+Sea) baseline is treated as fixed rather than also resampled. This captures uncertainty in "what if all volume drew from the Sea cost distribution" but does not reflect uncertainty in the observed baseline itself, so the interval is narrower than a fully two-sided bootstrap would give.

---

## Key findings

| Area | Finding |
|------|---------|
| 🚢 Transportation | Air and Sea shipping times are not statistically distinguishable (Mann-Whitney U), but Air costs ~3.5× more (36.6 vs 10.6 USD avg) |
| 💰 Cost simulation | Shifting 100% of Air to Sea → ~334,154 USD median savings (bootstrapped 95% CI: 264,186 – 381,422 USD) |
| ⚠️ Product quality | Electronics-related suppliers show the highest defect rates |
| 📈 Correlations | Manufacturing costs positively correlated with defect rate — complex products show higher quality variance |
| 📅 Stability | Lead times and shipping costs are stable across 2023–2024 (Mann-Whitney U); manufacturing lead time and shipping times don't differ significantly by supplier location (Kruskal-Wallis) |

---

## Business Impact

| Initiative | Action | Expected Outcome |
|------------|--------|-----------------|
| 🚢 Modal shift | Redirect Air shipments to Sea | ~334,154 USD savings (95% CI: 264,186–381,422 USD), with shipping time statistically indistinguishable between modes — though see the synthetic-data caveat above before treating this as a real-world recommendation |
| ⚠️ Supplier quality | Prioritize audits and improvement plans for high-defect electronics suppliers | Reduce defect-driven rework and return costs |
| 📦 Carrier & route optimization | Reallocate volume toward lower-cost carriers and routes identified in pivot analysis | Further reduction in per-unit shipping cost beyond the modal shift gains |
| 🔄 Lead time monitoring | Establish periodic checks (Kruskal-Wallis / Mann-Whitney) across supplier locations and time periods | Early detection of emerging regional or seasonal delays before they affect SLAs |

The **Air → Sea modal shift** is the largest single number in the analysis, but it rests on a hypothesis test result the author already flags as an artifact of synthetic data rather than a real-world pattern — treat it as a workflow exercise, not a production recommendation, without validating against real operational data. Quality improvements at high-defect electronics suppliers are a more directly actionable lever, since manufacturing cost and defect rate are positively correlated.

## Limitations
- Normality was assessed visually via Q-Q plots throughout, not with a formal test (e.g. Shapiro-Wilk).
- Multiple hypothesis tests were run across sections without correction for multiple comparisons.
- Kruskal-Wallis and Mann-Whitney results indicate an overall/pairwise difference (or lack of one) but don't by themselves establish which specific groups differ — no post-hoc test was run.
- The bootstrap simulation resamples only the Sea side of the comparison; it does not propagate uncertainty from the observed Air+Sea baseline.
