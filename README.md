# 🚀 FundedNext — Growth Analytics & Strategy Assessment

> **End-to-End Business Intelligence, Unit Economics, Econometric Demand Modeling & Strategic Growth Deliverables**

[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg?logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg?logo=jupyter&logoColor=white)](https://jupyter.org/)
[![Pandas](https://img.shields.io/badge/Pandas-2.0%2B-150458.svg?logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Statsmodels](https://img.shields.io/badge/Statsmodels-OLS%20Regression-informational.svg)](https://www.statsmodels.org/)

---

## 📌 Executive Summary

This repository contains a full-stack growth analytics case study and decision framework for **FundedNext**, evaluating **50,000 trading accounts**, **113,693 transactions**, and **$9.40M in trader disbursements** across an 18.5-month observation window.

The project delivers evidence-backed answers to two primary executive briefs:
1. **Part 1 — $200,000 Acquisition Campaign:** Optimal target segment allocation, customer lifetime value (LTV) models, expected ROI/ROAS return frontiers, geographic unit margins, and an RCT geo-testing experimental measurement design.
2. **Part 2 — Pricing & Promotions Strategy:** Empirical price elasticity of demand estimation via log-log OLS regressions across all 10 product tiers, promotional volume lift calculations, and a complete financial waterfall quantifying the net profit impact of deeper discounts accounting for baseline cannibalization, challenge revenue expansion, downstream resets, and trader payout COGS.

---

## 🎯 Key Strategic Insights

| Strategy Pillar | Key Finding | Strategic Recommendation |
| :--- | :--- | :--- |
| **Growth Engine** | **50K Challenges (Stellar 2-Step & 1-Step)** deliver **$456 – $562 Net Contribution per customer** (72.5%–83.0% margin) powered by heavy reset attachment (1.43–2.08 resets/account). | **Allocate 100% of the $200K Acquisition Budget** (60% to Stellar 2-Step 50K / 40% to Stellar 1-Step 50K), targeting **$1.25M net contribution (+523% net ROI)** at an $80 CAC base case. |
| **High-Velocity Promo Engine** | **Stellar Lite (25K & 50K)** exhibits hyper-elastic price sensitivity ($\beta \approx -5.0$), driving a **+380% to +413% volume surge** with minimal payout obligations ($11–$15/account) and 92%–94% net margins. | **Lean aggressively into promotional discounts (25%–30%)**. A +10% deeper discount yields **+$190.2K quarterly / +$760.7K annualized incremental net profit**. |
| **Protected Inelastic Tier** | **Stellar 1-Step Plans (10K, 25K, 50K)** are price-inelastic ($\beta = -0.39$ to $-0.79$). | **Maintain strict full catalog price (Zero Discounts)** to preserve margin and prevent top-line cannibalization. |
| **Toxic Risk Bleed** | **Stellar Instant (Plan 9)** bypasses challenge screening, resulting in an empirical net loss of **-$1,837 per account** (-$5.69M cumulative loss). | **Eliminate all discounts immediately** and phase out / restructure the instant funding product model. |

---

## 📂 Repository Structure

```text
├── Data/                          # Reconciled Production Data Exports
│   ├── plans.csv                  # Product catalog & pricing dimensions (10 plans)
│   ├── accounts.csv               # 50,000 trading account records & status lifecycle
│   ├── transactions.parquet       # 113,693 revenue events (purchases, resets, refunds)
│   ├── payouts.jsonl              # 8,480 trader disbursement records & performance payout tags
│   └── lifecycle.csv              # 42,280 CFD journey rollup records
├── Solutions/                     # Generated Executive Deliverables (Auto-exported)
│   ├── memo.md                    # Complete 3-page Executive Strategy Memorandum
│   ├── appendix.md                # Technical Appendix: Data cleaning, audit & methodology notes
│   ├── chart_acquisition.png      # Headline Visual: Unit Contribution & $200K Return Frontier
│   └── chart_pricing.png          # Headline Visual: Demand Elasticity Curves & Promo Waterfall
├── notebook.ipynb                 # Master End-to-End Analytics & Modeling Notebook
├── requirements.txt               # Python package dependencies
├── README.pdf                     # Original Case Study Brief
└── README.md                      # Repository Documentation
```

---

## 📊 Headline Visualizations

### 1. Acquisition Strategy & Unit Contribution
> **`Solutions/chart_acquisition.png`**
* **Panel A:** Unit Net Contribution per Account ($/account) comparing challenge purchases + resets against trader payout disbursements.
* **Panel B:** $200,000 Campaign Return Frontier across variable CAC scenarios ($40 to $120), benchmarking expected returns against the break-even spend threshold.

### 2. Pricing Elasticity & Financial Waterfall
> **`Solutions/chart_pricing.png`**
* **Panel A:** Empirical Price Elasticity of Demand ($\beta$) estimated via Log-Log OLS regressions across all 10 products.
* **Panel B:** Stellar Lite +10% Deeper Discount Financial Waterfall decomposing Baseline Revenue, Cannibalization Loss, Incremental Purchases, Incremental Resets, and Trader Payout COGS.

---

## 🛠️ Data Pipeline & Methodological Framework

### 1. Data Cleaning & Master Financial Reconciliation
- **Mixed Timestamp Normalization:** Multi-pattern parser reconciling ISO 8601, standard space timestamps, and European slash dates (`%d/%m/%Y %H:%M`) to prevent 2026 month-swapping errors.
- **Categorical Harmonization:** Normalized server types across casing variants (`'MT5'`, `'mt5'`, `'Mt5 '`, `'TRADOVATE'`).
- **Revenue & COGS Imputation:** Reconstructed missing transaction amounts via algebraic identities ($\text{Total} = \text{Amount} \times (1 - \text{Discount Pct})$) and linked real payout disbursements from JSON Lines.

### 2. Econometric Demand Estimation
$$\ln(\text{Units}_{i,t}) = \alpha_i + \beta_i \ln(\text{Effective Price}_{i,t}) + \epsilon_{i,t}$$
- **Log-Log OLS Demand Regressions:** Formulated over daily aggregated sales to extract unbiased elasticity coefficients ($\beta$) and measure statistically significant promotional volume shifts ($p < 0.0001$).

### 3. Financial Waterfall Decomposition
$$\Delta \text{Net Profit} = \underbrace{-Q_0 \cdot \Delta P}_{\text{Cannibalization}} + \underbrace{\Delta Q \cdot P_{\text{new}}}_{\text{Incremental Purchases}} + \underbrace{\Delta Q \cdot \bar{R}_{\text{acc}}}_{\text{Incremental Resets}} - \underbrace{\Delta Q \cdot \bar{C}_{\text{acc}}}_{\text{Incremental COGS}}$$

---

## ⚡ Quickstart & Reproducibility

### Prerequisites
- Python 3.10+
- Jupyter Notebook / JupyterLab or VS Code

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/fundednext-growth-analytics.git
   cd fundednext-growth-analytics
   ```

2. **Create and activate a virtual environment:**
   ```bash
   python -m venv .venv
   # Windows (PowerShell):
   .venv\Scripts\Activate.ps1
   # Linux/macOS:
   source .venv/bin/activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Run the master notebook:**
   ```bash
   jupyter notebook notebook.ipynb
   ```
   *Running all cells in `notebook.ipynb` will execute the end-to-end audit, re-run econometric models, generate headline charts, and export updated deliverables directly into the [`Solutions/`](Base Directory/Solutions) folder.*

---

## 📄 Deliverables

- [📄 Executive Memorandum (`Solutions/memo.md`)] (Base Directory/Solutions/memo.md)
- [📑 Technical Appendix (`Solutions/appendix.md`)] (Base Directory/Solutions/appendix.md)
- [📈 Master Analysis Notebook (`notebook.ipynb`)] (Base Directory/notebook.ipynb)

---

## 👤 Author
**Md. Golam Mahmud (Nafiz)**  
*Case Study & Executive Decision System for FundedNext*  
**Business Intelligence & Growth Analytics Department**
