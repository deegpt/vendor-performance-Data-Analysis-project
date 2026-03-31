# 🏪 Vendor Performance Data Analysis

[![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python)](https://www.python.org/)
[![SQLite](https://img.shields.io/badge/Database-SQLite-lightgrey?logo=sqlite)](https://www.sqlite.org/)
[![Pandas](https://img.shields.io/badge/Library-Pandas-darkblue?logo=pandas)](https://pandas.pydata.org/)
[![Jupyter](https://img.shields.io/badge/Notebook-Jupyter-orange?logo=jupyter)](https://jupyter.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> **An end-to-end data analysis project** examining vendor profitability, pricing strategies, inventory turnover, and supply chain efficiency across a multi-store beverage retail operation — built with Python, SQL, and SQLite.

---

## 📌 Project Overview

This project analyzes **~15 million rows** of transactional retail data spanning sales, purchases, inventory, and vendor invoices for a beverage distribution company operating across multiple cities. The goal is to surface actionable insights that enable smarter vendor selection, pricing optimization, and inventory management.

### Business Questions Answered

- Which vendors generate the most revenue and gross profit?
- Are there underperforming brands that need pricing adjustments?
- Does bulk purchasing reduce per-unit costs? By how much?
- Which products have the worst inventory turnover and why?
- Is there a statistically significant difference in profit margins between high-performing and low-performing vendors?

---

## 🗂️ Project Structure

```
vendor-performance-data-analysis-project/
│
├── Data Ingestion/               # ETL pipeline: CSV → SQLite
│   ├── ingestion_script.ipynb    # Automated ingestion with logging
│   └── README.md
│
├── Exploratory Data Analysis/    # EDA: Schema exploration, table profiling
│   ├── exploratory_data_analysis.ipynb
│   └── README.md
│
├── Vendor Performance Analysis/  # Core analysis, insights & recommendations
│   ├── vendor_performance_analysis.ipynb
│   └── README.md
│
├── docs/                         # Supporting documentation & visuals
├── business_problem.md           # Problem statement
├── project_explanation.md        # Methodology walkthrough
├── README.md                     # ← You are here
└── LICENSE
```

---

## 🗃️ Database Schema

The `inventory.db` SQLite database contains 7 tables:

| Table | Records | Description |
|---|---|---|
| `begin_inventory` | 206,529 | Opening stock at each store on Jan 1, 2024 |
| `end_inventory` | 224,489 | Closing stock at each store on Dec 31, 2024 |
| `purchases` | 2,372,474 | All purchase orders raised across the year |
| `purchase_prices` | 12,261 | Catalogue prices per vendor per brand |
| `sales` | 12,825,363 | Daily sales transactions by store & product |
| `vendor_invoice` | 5,543 | Invoice-level vendor billing with freight costs |
| `vendor_sales_summary` | 10,692 | Aggregated profitability view per vendor-brand |

---

## ⚙️ Tech Stack

| Layer | Tools Used |
|---|---|
| Language | Python 3.12 |
| Data Manipulation | Pandas, NumPy |
| Database | SQLite, SQLAlchemy |
| Analysis & Viz | Matplotlib, Seaborn |
| Statistical Testing | SciPy (t-test / Mann-Whitney U) |
| Automation | Python logging, os, time modules |
| Notebook Environment | Jupyter Notebook |

---

## 🔍 Key Insights

1. **Top 3 revenue-generating vendor-brand combos** are Jack Daniels No 7 (BROWN-FORMAN CORP), Tito's Handmade Vodka (MARTIGNETTI), and Absolut 80 Proof (PERNOD RICARD USA), with individual brand revenues exceeding **$4.5M+**.

2. **Bulk purchasing demonstrably reduces unit costs** — vendors with high purchase quantities show significantly lower per-unit purchase prices.

3. **Gross Profit filtering** removed transactions with GP ≤ 0 and zero-sale inventory items to focus on meaningful profitability signals.

4. **Hypothesis testing confirmed** (p-value < 0.05) that top-performing and low-performing vendors operate under statistically distinct profitability models.

5. **Freight cost as a % of invoice varies significantly** across vendors — some vendors impose freight costs >2% of invoice value, impacting net margins.

---

## 💡 Business Recommendations

- **Re-evaluate pricing** for high-margin, low-volume brands to unlock sales growth without sacrificing profitability.
- **Reduce vendor concentration risk** — diversify supply chain partnerships beyond the top 3-5 vendors.
- **Leverage bulk purchasing** for fast-moving SKUs to lock in lower unit costs and protect margins.
- **Liquidate slow-moving inventory** via clearance pricing or redistribution across stores before it becomes a holding-cost liability.
- **Negotiate freight terms** with vendors who charge disproportionate shipping costs relative to order value.

---

## 🚀 How to Run

```bash
# 1. Clone the repository
git clone https://github.com/deegpt/vendor-performance-data-analysis-project.git
cd vendor-performance-data-analysis-project

# 2. Install dependencies
pip install pandas sqlalchemy matplotlib seaborn scipy jupyter

# 3. Run data ingestion (ensure CSV files are in /data folder)
jupyter notebook "Data Ingestion/ingestion_script.ipynb"

# 4. Run EDA
jupyter notebook "Exploratory Data Analysis/exploratory_data_analysis.ipynb"

# 5. Run full analysis
jupyter notebook "Vendor Performance Analysis/vendor _performance_analysis.ipynb"
```

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

> 👤 **Author:** [@deegpt](https://github.com/deegpt)  
> 📅 **Analysis Period:** January 2024 – December 2024  
> 🏷️ **Domain:** Retail Analytics | Supply Chain | Vendor Management
