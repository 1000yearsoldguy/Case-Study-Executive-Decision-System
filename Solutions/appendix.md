# TECHNICAL APPENDIX: DATA RECONCILIATION & METHODOLOGY NOTES

**Document:** Technical Appendix to Executive Memorandum  
**Author:** Executive, Business Intelligence Department  
**Scope:** Data Quality, System Reconciliation, Statistical Caveats & Robustness Checks  

---

### 1. Data Quality Anomalies & Cleaning Protocols
During cross-table extraction and reconciliation across the 5 source systems, four primary data quality anomalies were identified and resolved:
1. **Mixed Date Formatting in Transactions (`transactions.parquet`):**
   * *Anomaly:* The `created_at_utc` column contained three distinct format patterns: ISO 8601 timestamps (`2025-01-13T15:29:21Z`, 92.0% of records), standard space-separated timestamps (`2025-09-05 16:49:53`, 5.0%), and European slash format (`13/07/2025 07:27`, 3.0%). Naive datetime parsing erroneously parsed `06/12/2024` as June 12 instead of Dec 6, creating false dates extending to December 2026.
   * *Resolution:* A multi-pattern parser was deployed enforcing `%d/%m/%Y %H:%M` for slash dates and ISO 8601 for standard formats. This fully aligned transaction dates with the accounts creation window (`2024-10-31` to `2026-05-15`).
2. **Server Type Categorical Inconsistencies (`accounts.csv`):**
   * *Anomaly:* Casing variants and trailing whitespaces (`'MT5'`, `'mt5'`, `'Mt5 '`, `'TRADOVATE'`, `'tradovate'`).
   * *Resolution:* Normalized via `.str.strip().str.upper()` into standard platform categories (`MT5`, `TRADOVATE`, `MT4`, `CTRADER`).
3. **Missing Amount and Total Fields (`transactions.parquet`):**
   * *Anomaly:* 3,412 rows had null `amount`; 5,605 rows had null `total`.
   * *Resolution:* Reconstructed missing values via exact mathematical identity: $\text{Total} = \text{Amount} \times (1 - \text{Discount Pct} / 100)$, backed by plan catalog base prices for full-price transactions.
4. **Lifecycle vs. Account Scope (`lifecycle.csv`):**
   * *Validation:* `lifecycle.csv` contains 42,280 rows exclusively representing CFD journeys (Plans 1–9), matching 100% of CFD accounts in `accounts.csv`. Futures Real accounts (Plan 10, 7,720 rows) are processed separately via Tradovate platform exports.

---

### 2. Financial Metrics & Unit Economics Definitions
* **Gross Purchase Revenue:** Upfront challenge fees received from new and repurchase accounts, net of payment processor refunds: $\sum \text{Purchase Total} - \sum \text{Refunded Total}$.
* **Reset Revenue:** Fees collected from breached traders restarting the same challenge account at discounted reset rates: $\sum \text{Reset Total}$.
* **Net Revenue:** $\text{Gross Purchase Revenue} + \text{Reset Revenue}$.
* **Cost of Goods Sold (COGS):** Total trader payout disbursements extracted from `payouts.jsonl` ($\sum \text{real\_disbursement\_amount}$). Commissions are logged as internal network disbursements ($518.4K total).
* **Net Contribution:** $\text{Net Revenue} - \text{COGS}$.
* **Net Contribution Margin:** $\text{Net Contribution} / \text{Net Revenue}$.
* **Net ROI:** $(\text{Net Contribution} - \text{Ad Spend}) / \text{Ad Spend}$.

---

### 3. Econometric Demand Model & Robustness Checks
* **Price Elasticity Specification:**
  $$\ln(Q_{i,t}) = \alpha_i + \beta_i \ln(P_{i,t}) + \epsilon_{i,t}$$
  where $Q_{i,t}$ is daily unit sales for plan $i$ at date $t$, and $P_{i,t}$ is the effective daily transaction price ($\text{Catalog Price} \times (1 - \text{Discount Pct})$).
* **Robustness & Significance:**
  * Stellar Lite 25K and 50K models yield $F$-statistics $> 800$ and $p$-values $< 10^{-100}$ with $R^2$ of $0.65 - 0.67$, confirming structural price elasticity.
  * Stellar 1-Step plans demonstrate robust inelasticity ($|\beta| < 1.0$) across both OLS log-log models and non-parametric promotional window comparisons (+10% to +24% volume lift under 23% average discounts).

---

### 4. Methodological Caveats & Forward Looking Risks
1. **Adverse Selection in Instant Funding:** Stellar Instant (Plan 9) has an empirical 100% funded conversion and zero reset revenue, generating -$1,837 net contribution per account. This structural loss reflects adverse selection where experienced predatory traders exploit instantaneous capital without proving risk discipline.
2. **Platform Provider Fees:** While platform hosting fees (e.g., MT5/Tradovate server costs of ~$3–$5/account) were not itemized in the database, incorporating a $5/account platform fee reduces portfolio net contribution from $3.87M to $3.62M (27.3% margin) without altering product rank-order or campaign recommendations.
3. **Capacity & Execution Limits:** Deeper discounting on Stellar Lite will expand daily trade executions by ~50%. Operational infrastructure must ensure server capacity and risk monitoring scale ahead of promotional rollouts.
