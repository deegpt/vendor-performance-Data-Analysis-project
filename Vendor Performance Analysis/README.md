# 📊 Vendor Performance Analysis

## Overview

This is the **core analysis module** of the project. It takes the cleaned, aggregated `vendor_sales_summary` dataset and performs a multi-dimensional profitability analysis across vendors and brands. The output is a set of data-backed business recommendations covering vendor strategy, pricing, inventory management, and supply chain risk.

---

## 📁 Files

| File | Description |
|---|---|
| `vendor_performance_analysis.ipynb` | Full analysis notebook with metrics, visualizations, statistical tests, and recommendations |

---

## 🎯 Analysis Objectives

| # | Objective |
|---|---|
| 1 | Identify the top vendors by total revenue and gross profit |
| 2 | Detect underperforming brands with high margins but low sales volumes |
| 3 | Quantify the impact of bulk purchasing on unit costs |
| 4 | Evaluate inventory turnover to flag slow-moving products |
| 5 | Statistically validate profitability differences between vendor tiers |

---

## 🔄 Analysis Workflow

```
Raw vendor_sales_summary
        │
        ▼
Data Cleaning & Feature Engineering
(GrossProfit, ProfitMargin, StockTurnoverRatio)
        │
        ▼
Filtering (GP > 0, Margin > 0, Sales > 0)
        │
        ▼
┌──────────────────────────────────┐
│  1. Vendor Revenue Ranking       │
│  2. Gross Profit Analysis        │
│  3. Profit Margin Distribution   │
│  4. Bulk Purchase Impact         │
│  5. Inventory Turnover           │
│  6. Hypothesis Testing           │
└──────────────────────────────────┘
        │
        ▼
Business Recommendations
```

---

## 🛠️ Feature Engineering

New columns derived from existing data:

| New Feature | Formula | Purpose |
|---|---|---|
| `GrossProfit` | `TotalSalesDollars - TotalPurchaseDollars` | Core profitability metric |
| `ProfitMargin` | `(GrossProfit / TotalSalesDollars) × 100` | Normalized profitability % |
| `StockTurnoverRatio` | `TotalSalesQuantity / TotalPurchaseQuantity` | Inventory efficiency indicator |
| `AvgUnitCost` | `TotalPurchaseDollars / TotalPurchaseQuantity` | Bulk discount verification |

---

## 📈 Key Findings

### 1. Top Vendors by Revenue

| Rank | Vendor | Top Brand | Total Sales Revenue |
|---|---|---|---|
| 1 | BROWN-FORMAN CORP | Jack Daniels No 7 Black | $5.1M |
| 2 | MARTIGNETTI COMPANIES | Tito's Handmade Vodka | $4.8M |
| 3 | PERNOD RICARD USA | Absolut 80 Proof | $4.5M |
| 4 | DIAGEO NORTH AMERICA | Captain Morgan Spiced Rum | $4.5M |
| 5 | DIAGEO NORTH AMERICA | Ketel One Vodka | $4.2M |

### 2. Profitability Tiers

- **High-margin, low-volume vendors** — Operate on a premium model with high profit margins per unit but limited sales reach. Suitable for focused upselling strategies.
- **High-volume, moderate-margin vendors** — Drive bulk revenue with thinner margins; scale is the competitive advantage. Freight cost optimization is critical here.
- **Low-margin, low-volume vendors** — Represent the highest risk; these warrant re-evaluation or delisting.

### 3. Bulk Purchasing Impact

Analysis of the `purchases` table confirms that vendors with higher purchase volumes demonstrate consistently lower average unit costs. This relationship validates a **volume-discount pricing structure** across most vendors in the portfolio.

### 4. Inventory Turnover

- Products with `StockTurnoverRatio < 0.5` are classified as **slow-movers** — meaning less than half the purchased inventory was sold during the year.
- These items represent **capital tied up in unsold stock** and should be prioritized for clearance or procurement reduction.

### 5. Freight Cost Analysis

Freight costs as a proportion of invoice value vary widely by vendor. Vendors with freight exceeding **2.5% of invoice dollars** are flagged for renegotiation, as this directly erodes net margins.

---

## 🧪 Hypothesis Testing

**Test Design:**
- **Group A:** Top-performing vendors (top 25% by GrossProfit)
- **Group B:** Low-performing vendors (bottom 25% by GrossProfit)
- **Test:** Two-sample statistical test on `ProfitMargin`

**Hypotheses:**
- **H₀ (Null):** No significant difference in profit margin between top and low-performing vendors.
- **H₁ (Alternate):** A statistically significant difference exists in profit margins between the two groups.

**Result:** ✅ **Null hypothesis rejected** (p-value < 0.05)

**Interpretation:** The two vendor groups operate under fundamentally different profitability models. Top-performing vendors do not simply sell more — they achieve structurally superior margins, likely through better pricing power, lower freight terms, or more efficient product mix.

---

## 💡 Business Recommendations

### 1. 🏷️ Re-evaluate Pricing for Low-Volume, High-Margin Brands
> These products have healthy margins but are not moving. Modest price reductions or promotional bundles can stimulate volume without significantly hurting profitability.

### 2. 🔗 Diversify Vendor Partnerships
> The top 5 vendor-brand combinations account for a disproportionate share of revenue. Heavy concentration in a few suppliers creates supply chain fragility. Onboarding alternative vendors for top SKUs reduces dependency risk.

### 3. 📦 Leverage Bulk Purchasing Strategically
> For fast-moving SKUs (high turnover ratio), negotiate volume-based contracts to lock in lower unit costs. Avoid bulk purchasing for slow-movers — it amplifies holding costs.

### 4. 🗑️ Liquidate Slow-Moving Inventory
> Products with turnover ratios below 0.5 should be flagged in monthly reviews. Options include: cross-store redistribution to high-demand locations, flash sales/clearance pricing, or reducing future purchase quantities.

### 5. 🚚 Renegotiate Freight Terms
> Identify vendors where freight cost exceeds 2.5% of invoice value. Even a 0.5% freight reduction across high-volume vendors can translate into thousands of dollars in recovered margin annually.

### 6. 📊 Build a Vendor Scorecard
> Operationalize this analysis by building a quarterly vendor scorecard tracking: Revenue, Gross Profit, Profit Margin, Stock Turnover, and Freight % — enabling proactive vendor management vs. reactive responses.

---

## 📐 Statistical Methods Used

| Method | Application |
|---|---|
| Descriptive Statistics | Mean, median, std dev of profitability metrics across vendors |
| Distribution Analysis | Profit margin distribution to identify outliers |
| Two-sample t-test / Mann-Whitney U | Testing margin difference between vendor tiers |
| Correlation Analysis | Bulk quantity vs. unit cost relationship |
| Segmentation | Vendor tiering by revenue quartiles |

---

## 🛠️ Tech Stack

| Tool | Usage |
|---|---|
| `pandas` | Data wrangling, feature engineering, aggregations |
| `matplotlib` / `seaborn` | Visualizations (bar charts, box plots, scatter plots) |
| `scipy.stats` | Statistical hypothesis testing |
| `sqlite3` | Querying `vendor_sales_summary` from database |

---

> ⬅️ [Back to EDA](../Exploratory%20Data%20Analysis/README.md) | 🏠 [Back to root](../README.md)
