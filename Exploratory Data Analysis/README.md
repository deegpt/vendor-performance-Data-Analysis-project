# 🔍 Exploratory Data Analysis (EDA)

## Overview

This module performs a **comprehensive exploration of the inventory database** to understand data structure, record counts, relationships between tables, and business-relevant patterns. The EDA directly informs what columns and aggregations are needed for the downstream vendor performance analysis.

The central question driving EDA: *What does the data tell us about vendor behaviour, product pricing, and inventory flow — before any formal analysis begins?*

---

## 📁 Files

| File | Description |
|---|---|
| `exploratory_data_analysis.ipynb` | Full EDA notebook with SQL queries, table profiling, and observations |

---

## 🗃️ Database Tables Explored

All 7 tables in `inventory.db` were profiled:

| Table | Records | Key Columns Explored |
|---|---|---|
| `begin_inventory` | 206,529 | Store, City, Brand, Description, Size, onHand, Price, startDate |
| `end_inventory` | 224,489 | Store, City, Brand, Description, Size, onHand, Price, endDate |
| `purchases` | 2,372,474 | VendorNumber, VendorName, PurchasePrice, Quantity, Dollars, PODate, ReceivingDate |
| `purchase_prices` | 12,261 | Brand, Price, PurchasePrice, Volume, VendorNumber, VendorName |
| `sales` | 12,825,363 | SalesQuantity, SalesDollars, SalesPrice, SalesDate, ExciseTax, VendorNo |
| `vendor_invoice` | 5,543 | VendorNumber, InvoiceDate, PODate, PayDate, Quantity, Dollars, Freight |
| `vendor_sales_summary` | 10,692 | TotalPurchaseDollars, TotalSalesDollars, FreightCost, PurchasePrice, ActualPrice |

---

## 🔬 Analytical Steps Performed

### 1. Schema Discovery
Connected to `inventory.db` using `sqlite3` and queried `sqlite_master` to enumerate all available tables:
```python
conn = sqlite3.connect('inventory.db')
tables = pd.read_sql_query("SELECT name FROM sqlite_master WHERE type = 'table'", conn)
```

### 2. Table Profiling
For every table:
- Counted total records
- Printed the first 5 rows to understand column structure and data types
- Identified key foreign keys linking tables (e.g., `VendorNumber`, `Brand`, `InventoryId`)

### 3. Single-Vendor Deep Dive
Randomly selected **AMERICAN VINTAGE BEVERAGE (VendorNumber: 4466)** to trace the full data journey for one vendor:
- Found 2,192 purchase records across multiple stores
- Traced 55 vendor invoices across the full year (Jan 2024 – Jan 2025)
- Matched 9,453 sales transactions for this vendor's 3 products
- Verified price consistency: purchase price $9.35–$9.41 vs. retail price $12.99

### 4. Business Relevance Mapping
Mapped each column to the three core business objectives:
- **Vendor Profitability** → `TotalPurchaseDollars`, `TotalSalesDollars`, `FreightCost`
- **Pricing Strategy** → `PurchasePrice`, `ActualPrice`, `SalesPrice`
- **Inventory Management** → `onHand`, `startDate`, `endDate`, `TotalSalesQuantity`

---

## 💡 Key Observations from EDA

1. **The `vendor_sales_summary` table is the analytical backbone** — it pre-aggregates purchase quantities, sales dollars, freight costs, and excise taxes per vendor-brand pair, making it the primary table for performance analysis.

2. **Margin exists at all price levels**: Even low-cost items like TGI Fridays ($9.35 purchase → $12.99 retail) carry a ~39% gross margin, indicating consistent markup strategy.

3. **The `sales` table is the largest** at 12.8M rows — any full-table scans should be avoided; filtered queries by vendor or date range are essential.

4. **Freight cost tracking**: The `vendor_invoice` table captures freight costs per PO, which is critical for accurate net profitability calculations — often overlooked in basic analyses.

5. **`InventoryId` is a composite key** combining Store + Brand (e.g., `1_HARDERSFIELD_58`), enabling easy filtering by store or brand independently.

6. **Data spans Jan 2024 to Dec 2024** (full calendar year) with some invoices extending payment dates into early 2025.

---

## 🧠 Decisions Made During EDA

| Decision | Reasoning |
|---|---|
| Use `vendor_sales_summary` as primary table | Pre-aggregated; avoids joining 15M+ rows repeatedly |
| Exclude GP ≤ 0 rows | Removes returns, write-offs, and data errors from profitability analysis |
| Exclude zero-sales inventory | Items never sold skew turnover calculations |
| Create derived `GrossProfit` column | `TotalSalesDollars - TotalPurchaseDollars` |
| Create derived `ProfitMargin` column | `(GrossProfit / TotalSalesDollars) × 100` |

---

## 📊 Sample Data Profile

**`purchase_prices` table (vendor pricing catalogue):**
```
Brand | Description              | Price | PurchasePrice | Margin
62    | Herradura Silver Tequila | 36.99 | 28.67         | 22.5%
63    | Herradura Reposado       | 38.99 | 30.46         | 21.9%
72    | No. 3 London Dry Gin     | 34.99 | 26.11         | 25.4%
58    | Gekkeikan Black & Gold   | 12.99 | 9.28          | 28.6%
```

**`vendor_sales_summary` top performers (by TotalSalesDollars):**
```
Vendor               | Brand Product              | TotalSalesDollars
BROWN-FORMAN CORP    | Jack Daniels No 7 Black   | $5,101,919
MARTIGNETTI          | Tito's Handmade Vodka     | $4,819,073
PERNOD RICARD USA    | Absolut 80 Proof          | $4,538,120
DIAGEO NORTH AMERICA | Capt Morgan Spiced Rum    | $4,475,972
```

---

> ⬅️ [Back to root](../README.md) | ➡️ [Vendor Performance Analysis](../Vendor%20Performance%20Analysis/README.md)
