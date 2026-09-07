# SQL Data Warehouse & Analytics Project

**Built by Sudhanshu Dharmendra Yadav**  
B.Tech CSE (Business Systems) | Data Analyst | [LinkedIn](https://www.linkedin.com/in/sudhanshu-yadav-dataanalyst/) · [GitHub](https://github.com/Sudhanshu-yadav01) · [Portfolio](https://sudhanshuyadav.netlify.app)

---

## Why I Built This

Most portfolios stop at dashboards. I wanted to go deeper — to show I understand *where* the data comes from before it reaches a Power BI report. This project is my hands-on walkthrough of building a production-style data warehouse from scratch, using SQL Server and Medallion Architecture (Bronze → Silver → Gold layers).

If you're a recruiter or hiring manager reading this: this project demonstrates my ability to design ETL pipelines, model data for analytics, and write business-insight SQL — not just drag visuals onto a canvas.

---

## What This Project Does

I consolidated sales data from two simulated source systems — **ERP** and **CRM** — into a unified, analysis-ready data model. The end goal: clean, structured data that business stakeholders can query for insights on customer behavior, product performance, and sales trends.

### My Architecture: Medallion Layers

```
[ERP CSV] ──┐
            ├──► BRONZE (raw ingestion) ──► SILVER (cleanse & standardize) ──► GOLD (star schema, BI-ready)
[CRM CSV] ──┘
```

| Layer  | What I Did |
|--------|-----------|
| 🟤 Bronze | Loaded raw CSV files as-is into SQL Server — no transformations, just ingestion |
| ⚪ Silver | Cleaned nulls, resolved duplicates, standardized formats, normalized fields |
| 🟡 Gold | Modeled fact + dimension tables in a star schema optimized for analytical queries |

---

## My Walkthrough — Step by Step

### Step 1: Understanding the Source Data

I started by analyzing two CSV datasets:
- **ERP data** — transactional sales records
- **CRM data** — customer and product metadata

Before writing a single SQL line, I mapped out the relationships using DrawIO and identified key data quality issues (nulls, inconsistent date formats, duplicate customer IDs across systems).

### Step 2: Building the Bronze Layer

The Bronze layer is intentionally "dumb" — I bulk-loaded both CSVs into SQL Server tables exactly as they came in. No transformations. This preserves the raw source state and gives me a rollback point.

```sql
-- Example: Bronze ingestion pattern
BULK INSERT bronze.erp_sales
FROM 'datasets/erp_sales.csv'
WITH (FIRSTROW = 2, FIELDTERMINATOR = ',', ROWTERMINATOR = '\n');
```

### Step 3: Silver Layer — Where the Real Work Happens

This is where I spent the most time. Key transformations I applied:
- **Null handling** — replaced missing customer segments with 'Unknown', flagged incomplete orders
- **Date standardization** — normalized all dates to `YYYY-MM-DD`
- **Deduplication** — used `ROW_NUMBER()` with `PARTITION BY` to keep the latest record per customer
- **Cross-source join** — matched CRM customer IDs to ERP records using fuzzy logic where IDs didn't align perfectly

### Step 4: Gold Layer — Star Schema Design

I modeled the Gold layer as a classic star schema:

```
                    ┌──────────────┐
                    │  fact_sales  │
                    └──────┬───────┘
          ┌─────────┬──────┴──────┬─────────┐
   dim_customer  dim_product  dim_date   dim_location
```

Every dimension table has a surrogate key. The fact table stores only foreign keys + measures (quantity, revenue, discount). This keeps queries fast and the model clean.

### Step 5: Analytics & Reporting SQL

With Gold layer ready, I wrote analytical queries for:

**Customer Behavior**
- RFM segmentation (Recency, Frequency, Monetary)
- Customer lifetime value by segment

**Product Performance**
- Revenue contribution by category
- Return rate analysis

**Sales Trends**
- Month-over-month and Year-over-Year growth
- Seasonal demand patterns

---

## Tools & Tech I Used

| Tool | Purpose |
|------|---------|
| SQL Server Express | Database engine |
| SSMS | Query writing & DB management |
| DrawIO | Architecture diagrams, data flow, star schema design |
| Git + GitHub | Version control |
| Notion | Project planning & task tracking |

---

## What I Learned

- **ETL design thinking** — understanding *why* each layer exists, not just following a template
- **Data quality is the hardest part** — 60% of my time was in Silver layer, not Gold
- **Star schema tradeoffs** — when to denormalize a dimension vs keep it normalized
- **Writing analytical SQL at scale** — window functions, CTEs, and query optimization for large fact tables

---

## Repository Structure

```
sql-data-warehouse-project/
│
├── datasets/               # Raw ERP and CRM CSV files
├── docs/
│   ├── data_architecture.drawio
│   ├── data_flow.drawio
│   ├── data_models.drawio  # Star schema diagram
│   ├── data_catalog.md     # Field descriptions & metadata
│   └── naming-conventions.md
│
├── scripts/
│   ├── bronze/             # Raw ingestion scripts
│   ├── silver/             # Cleansing & transformation
│   └── gold/               # Star schema creation & analytical views
│
├── tests/                  # Data quality checks
├── README.md
└── requirements.txt
```

---

## Connect With Me


If this project resonates with you or you'd like to discuss my work, let's connect:

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/sudhanshu-yadav-dataanalyst/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Sudhanshu-yadav01)
[![Portfolio](https://img.shields.io/badge/Portfolio-000000?style=for-the-badge&logo=google-chrome&logoColor=white)](https://sudhanshuyadav.netlify.app)
