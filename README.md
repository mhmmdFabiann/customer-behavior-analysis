# Customer Shopping Behavior & Revenue Optimization Analysis

An end-to-end data analytics project exploring customer demographics, purchasing behavior, discount sensitivity, and subscription status to deliver actionable insights and optimize retail revenue streams.

---

## 📌 Executive Summary

This project establishes a full data analytics lifecycle: from exploratory data analysis and feature engineering in **Python**, relational data modeling and querying in **PostgreSQL**, to an interactive executive dashboard in **Power BI**. 

The goal is to understand what drives customer spending, assess whether promotional discounts erode or enhance margins, and identify strategies for customer retention.

---

## 🛠️ Tech Stack & Architecture

| Layer | Tools / Technologies | Key Activities |
| :--- | :--- | :--- |
| **Data Cleaning & Prep** | Python (Pandas, NumPy, SQLAlchemy) | Missing value imputation, feature engineering, pipeline automation |
| **Data Warehousing & Querying** | PostgreSQL 16+ / pgAdmin | Advanced SQL, CTEs, Window Functions, conditional aggregations |
| **Business Intelligence** | Microsoft Power BI Desktop | Interactive DAX measures, modern card visuals, multi-button slicers |
| **Documentation & Delivery** | Markdown, Git & GitHub | Modular code structure, reproducible workflow |

```
                       DATA PIPELINE ARCHITECTURE
                       
   [Raw CSV Data] 
         │
         ▼
[Python / Pandas] ────► • Category-based Median Imputation
         │              • Column standardisation (snake_case)
         │              • Feature Engineering (age_group, frequency_days)
         ▼
[PostgreSQL Database] ─► • Analytical Queries (CTEs, Window Functions)
         │              • Customer Segmentation & Promo Sensitivity
         ▼
[Power BI Dashboard] ──► • Interactive KPI Cards & Donut Chart
                        • Category Revenue vs. Order Volume Breakdown
                        • Multi-dimensional Slicers (Gender, Subscriptions)
```

---

## 📂 Project Repository Structure

```text
customer-behavior-analysis/
│
├── data/
│   ├── raw/
│   │   └── customer_shopping_behavior.csv
│   └── processed/
│       └── cleaned_customer_data.csv
│
├── notebooks/
│   ├── eda_and_cleaning.ipynb          # EDA, imputation, & feature engineering
│   └── 02_load_to_db.ipynb             # Data ingestion pipeline to PostgreSQL
│
├── sql/
│   └── customer_behavior_sql_queries.sql # Business queries (Q1 to Q10)
│
├── dashboard/
│   ├── customer_behavior_dashboard.pbix # Power BI report file
│   └── screenshots/
│       └── dashboard_overview.png       # Preview image for README
│
├── reports/
│   └── laporan_proyek.md               # Detailed analytical documentation
│
├── .gitignore
├── LICENSE
└── README.md
```

---

## 🔍 Data Preparation & Feature Engineering (Python)

1. **Contextual Imputation:**
   Rather than applying a global median across all records, missing values in `review_rating` were imputed using **category-specific medians** (`df.groupby('category')['review_rating'].transform(...)`), preserving distinct product category rating benchmarks.
2. **Naming Conventions:**
   All column headers were programmatically sanitized into `snake_case` using regular expressions to ensure zero syntax clashes in PostgreSQL.
3. **Feature Derivation:**
   - `age_group`: Segmented customer age into quantile-based cohorts (`young adult`, `adult`, `middle-aged`, `senior`).
   - `purchase_frequency_days`: Mapped qualitative frequency strings (`Weekly`, `Fortnightly`, `Monthly`, `Annually`) into standardized numeric day intervals.
4. **Redundancy Pruning:**
   Removed `promo_code_used` after correlation testing proved a 100% mutual match with `discount_applied`.

---

## 📊 Key Analytical Queries & Business Insights (SQL)

### 1. Subscription Spend Paradox
* **Finding:** Non-subscribers average **$59.87** per transaction, while subscribers average **$59.49**. Subscribers do *not* yield a higher Average Order Value (AOV).
* **Business Takeaway:** Subscription programs currently serve as access programs rather than spending accelerators. Growth efforts should focus on cross-selling rather than purely increasing subscription sign-ups.

### 2. Discount Sensitivity by Product
* **Finding:** Items such as Hats and Footwear showed high discount utilization (>45%).
* **SQL Implementation (Conditional Aggregation):**
```sql
SELECT 
    item_purchased AS product, 
    ROUND((SUM(CASE WHEN discount_applied = 'Yes' THEN 1.0 ELSE 0 END) / COUNT(*)) * 100, 2) AS discount_percentage
FROM customer
GROUP BY item_purchased
ORDER BY discount_percentage DESC
LIMIT 5;
```

### 3. Top 3 Best-Selling Products per Category
* **Finding:** Utilized PostgreSQL Window Functions with `PARTITION BY` to rank top volume drivers across Accessories, Clothing, Footwear, and Outerwear.
* **SQL Implementation:**
```sql
WITH ranked_items AS (
    SELECT 
        category,
        item_purchased AS item,
        COUNT(*) AS total_orders,
        ROW_NUMBER() OVER (
            PARTITION BY category 
            ORDER BY COUNT(*) DESC
        ) AS item_rank
    FROM customer
    GROUP BY category, item_purchased
)
SELECT category, item, total_orders, item_rank
FROM ranked_items
WHERE item_rank <= 3
ORDER BY category, item_rank;
```

---

## 📈 Power BI Interactive Dashboard

![Dashboard Overview](dashboard/screenshots/dashboard_overview.png)

### Core Dashboard Metrics:
* **Total Revenue:** ~$233K aggregate sales.
* **Customer Base:** 3,900 unique customer transactions.
* **Average Order Value (AOV):** $59.76 per order.
* **Customer Satisfaction:** 3.75 / 5.00 average review rating.
* **Subscription Distribution:** 27% Subscribed vs. 73% Non-subscribed.

---

## 🚀 How to Replicate This Project Locally

### 1. Prerequisites
- Python 3.10+
- PostgreSQL 14+ and pgAdmin 4
- Microsoft Power BI Desktop (Windows)

### 2. Clone the Repository
```bash
git clone https://github.com/mhmmdFabiann/customer-behavior-analysis.git
cd customer-behavior-analysis
```

### 3. Setup Python Environment & Run Pipeline
```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
pip install pandas numpy sqlalchemy psycopg2-binary
```
Execute `notebooks/eda_and_cleaning.ipynb` followed by `notebooks/02_load_to_db.ipynb` to clean the dataset and ingest it into your PostgreSQL instance.

### 4. Database Setup & SQL Execution
Open `sql/customer_behavior_sql_queries.sql` in VS Code or pgAdmin to run the analytical queries against the `customer_behavior` database.

### 5. Launch Power BI
Open `dashboard/customer_behavior_dashboard.pbix` in Power BI Desktop and update the PostgreSQL data source credentials to match your local database settings.

---

## 📜 License

This project is licensed under the [MIT License](LICENSE).