# 📥 Data Ingestion Pipeline

## Overview

This module handles the **automated extraction and loading (EL)** of raw CSV data files into a local SQLite database (`inventory.db`). It forms the foundation of the entire analysis pipeline — all downstream notebooks query this database directly.

The ingestion is designed to be **idempotent** (safe to re-run), **logged**, and structured for scheduled execution, making it production-ready for scenarios like nightly batch refreshes.

---

## 📁 Files

| File | Description |
|---|---|
| `ingestion_script.ipynb` | Main ingestion notebook with the ETL pipeline |

---

## ⚙️ How It Works

The ingestion script follows a clean **functional programming structure** — all logic is encapsulated in reusable functions:

```python
def ingest_db(df, table_name, engine):
    """Loads a DataFrame into the SQLite database, replacing the table if it already exists."""
    df.to_sql(table_name, con=engine, if_exists='replace', index=False)

def load_raw_data():
    """Scans the /data directory, reads all CSVs, and ingests them into the database."""
    for file in os.listdir('data'):
        if '.csv' in file:
            df = pd.read_csv('data/' + file)
            ingest_db(df, file[:-4], engine)
```

The `if __name__ == '__main__'` guard ensures the script only runs when directly executed — not when imported as a module — following Python best practices.

---

## 📋 Tables Ingested

After running the ingestion script, the following 7 tables are created/refreshed in `inventory.db`:

| Table Name | Source CSV | Records |
|---|---|---|
| `begin_inventory` | begin_inventory.csv | 206,529 |
| `end_inventory` | end_inventory.csv | 224,489 |
| `purchases` | purchases.csv | 2,372,474 |
| `purchase_prices` | purchase_prices.csv | 12,261 |
| `sales` | sales.csv | 12,825,363 |
| `vendor_invoice` | vendor_invoice.csv | 5,543 |
| `vendor_sales_summary` | vendor_sales_summary.csv | 10,692 |

**Total records ingested: ~15.6 million rows**

---

## 📝 Logging

The script uses Python's built-in `logging` module to write detailed execution logs to `logs/ingestion_db.log`:

```python
logging.basicConfig(
    filename='logs/ingestion_db.log',
    level=logging.DEBUG,
    format='%(asctime)s - %(levelname)s - %(message)s',
    filemode='a'  # Appends on each run, preserving history
)
```

Logged events include:
- Each file being ingested (table name, timestamp)
- Completion confirmation
- Total execution time in minutes

This logging setup ensures **observability** — any failure during ingestion is traceable to the exact file and timestamp.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| `pandas` | Reading CSV files into DataFrames |
| `sqlalchemy` | Creating DB engine and managing connections |
| `sqlite3` / SQLite | Lightweight local database |
| `os` | Directory scanning for CSV files |
| `logging` | Execution monitoring and error tracking |
| `time` | Measuring total ingestion duration |

---

## ▶️ Running the Script

```bash
# Ensure your raw CSVs are inside a /data directory at the project root
# Then open and run the notebook:
jupyter notebook ingestion_script.ipynb
```

Or convert to a standalone Python script for scheduling:

```bash
jupyter nbconvert --to script ingestion_script.ipynb
python ingestion_script.py
```

For scheduled execution (Linux/Mac), add to cron:
```bash
# Runs every day at 2 AM
0 2 * * * /usr/bin/python /path/to/ingestion_script.py
```

---

## 📌 Design Decisions

- **`if_exists='replace'`** — Chosen over `append` to ensure a clean, full refresh on every run, preventing duplicate records.
- **SQLAlchemy engine** — Used instead of raw `sqlite3` connection for better compatibility and future extensibility (easy to swap SQLite for PostgreSQL).
- **Functional structure** — Separate functions for `ingest_db` and `load_raw_data` keep concerns separated and make unit testing straightforward.
- **Append-mode logging** — Logs accumulate over time, enabling historical run tracking and audit trails.

---

> ⬆️ [Back to project root](../README.md)
