# 📉 Churn Analysis & Customer Intelligence

> An end-to-end subscriber churn and retention analysis workflow using a relational SQLite database (`customer_churn.db`) and Python (`pandas`, `numpy`, `sqlite3`, `matplotlib`, `seaborn`) to uncover churn rates, revenue leakage, support escalation impact, and customer risk segments.

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.x-blue?logo=python" alt="Python"/>
  <img src="https://img.shields.io/badge/SQLite-Database-003B57?logo=sqlite" alt="SQLite"/>
  <img src="https://img.shields.io/badge/Pandas-Data%20Analysis-150458?logo=pandas" alt="Pandas"/>
  <img src="https://img.shields.io/badge/NumPy-Numerical-013243?logo=numpy" alt="NumPy"/>
  <img src="https://img.shields.io/badge/Matplotlib-Visualization-11557c" alt="Matplotlib"/>
  <img src="https://img.shields.io/badge/Seaborn-Statistical%20Plots-4C72B0" alt="Seaborn"/>
</p>

---

# 📌 Overview

Churn Analysis & Customer Intelligence is a data analytics project that connects to a multi-table SQLite database (`customer_churn.db`), cleans and transforms subscriber and support data in Python, computes retention and revenue metrics, and visualizes churn patterns across subscription tiers, contract types, regions, and support escalations.

The workflow covers three core questions:

- **Who:** Identifying which subscribers churned or fall into higher churn-risk tiers[cite: 1].
- **Why:** Examining cancellation reasons, plan/contract patterns, and support escalations[cite: 1].
- **When:** Tracking monthly cancellation trends and calculating customer tenure.

---

# 📊 Database & Dataset Summary

The SQLite database (`customer_churn.db`) contains **21 customers** across three relational tables joined on `customerid`:

- **`db_customer` (21 rows, 8 columns):** `customerid`, `name`, `country`, `state`, `gender`, `dob`, `interests`, `pincode`.
- **`db_subscription` (21 rows, 11 columns):** `customerid`, `subscription_start_date`, `subscription_type`, `renewal_date`, `plan_type`, `contract_type`, `cancellation_date`, `cancellation_reason`, `monthly_charges`, `cltv`, `churn_score`.
- **`db_support` (9 rows, 6 columns):** `customerid`, `complaint_date`, `escalations`, `csat_score`, `col_1`, `comment`.

After cleaning, feature engineering, and merging in `Churn_analysis.ipynb`, the resulting dataset is exported to `exported_churn_data.csv`.

---

# 🚀 Key Features & Analysis Steps

### 🔌 SQLite Database Integration

- Connects to `customer_churn.db` using `sqlite3` and `pandas.read_sql`.
- Inspects table names from `sqlite_master` and column metadata via `PRAGMA table_info`.

### 🧹 Data Cleaning

- Renames `name` to `customer_name` and drops sparse/unused columns (`interests`, `pincode`, `col_1`, `comment`)[cite: 2].
- Converts date columns (`dob`, `subscription_start_date`, `renewal_date`, `cancellation_date`, `complaint_date`) to `datetime`[cite: 2].
- Standardizes `gender` values (`Men` → `Male`, `Women` → `Female`) and imputes missing `country` values using a `state`-to-`country` mapping[cite: 2].
- Handles duplicate support records per `customerid` by counting complaints and keeping the latest complaint record before merging[cite: 2].

### ⚙️ Feature Engineering & KPIs

- **`churn_flag`:** Binary indicator (`1` if `cancellation_date` is present, else `0`)[cite: 2].
- **`tenure_days`:** Days between `subscription_start_date` and `cancellation_date` (or current date for active subscribers)[cite: 2].
- **`churn_risk`:** Risk tiers derived from `churn_score` (`low` for `< 50`, `med` for `50–69`, `high` for `70+`)[cite: 2].
- **Calculated Metrics:** Churn rate, retention rate, churn by plan type, churn by state, ARPU, average tenure, revenue lost from churned users, escalation rate, average complaints per user, correlation between escalations and churn, and pivot tables[cite: 1, 2].

### 📈 Data Visualization

- Generates charts and plots using `matplotlib` and `seaborn`, including churn vs. retention, monthly churn trend, churn by plan type, churn by subscription type, churn by risk tier, escalations vs. churn, cancellation reasons, CLTV distribution, churn by state, correlation heatmaps, pairplots, and categorical plots[cite: 2].

---

# 🏗️ Project Workflow

```text
1. Connect to customer_churn.db (sqlite3)
          │
          ▼
2. Load Tables (db_customer, db_subscription, db_support)
          │
          ▼
3. Clean Data & Standardize Fields (pandas, numpy)
          │
          ▼
4. Engineer Features & Merge Tables on customerid
          │
          ▼
5. Export Merged Dataset (exported_churn_data.csv)
          │
          ▼
6. Compute KPIs, Pivot Tables & Correlations
          │
          ▼
7. Generate Visualizations (matplotlib, seaborn)
```

---

# 🛠️ Tech Stack

| **Component** | **Technology** |
| :--- | :--- |
| Language | Python |
| Database | SQLite (`sqlite3`) |
| Data Manipulation | Pandas, NumPy |
| Visualization | Matplotlib, Seaborn |
| Notebook Environment | Jupyter Notebook (`Churn_analysis.ipynb`) |
| Reference & Documentation | `Data Analytics Project -Churn Analysis Report.pdf` |

---

# 💡 Key Findings Summary

- **Churn & Retention:** The cohort shows a **28.57%** churn rate and **71.43%** retention rate across 21 subscribers.
- **Contract & Plan Patterns:** Monthly subscribers churn at a much higher rate (**55.56%**) than annual subscribers (**8.33%**), and the **Basic** plan has the highest churn rate (**60.00%**) among plan tiers.
- **Revenue & CLTV Impact:** Churned users account for **$73.94** in lost monthly charges and **$2,047** in lost CLTV.
- **Support Escalations:** Escalated support tickets correlate strongly with churn, and cancellations cluster in **September 2024** and **Karnataka**.
