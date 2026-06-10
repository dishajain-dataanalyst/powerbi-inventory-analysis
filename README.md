# WarmeHands Inventory Analysis — Power BI Case Study

### End-to-End Inventory Dashboard — by Disha Jain | DataCamp Certified

---

## Why I Built This

Inventory analysis is one of the highest-impact use cases for BI — getting it wrong means either overstocking (wasting cash) or stockouts (losing revenue). This case study gave me the chance to build a full inventory analytics workflow from raw transactional data to an executive dashboard, using the same metrics real operations teams use.

The client, WarmeHands Inc., needed actionable insights on which products to renew, reorder, or deprioritize. I delivered a dashboard covering Revenue, COGS, Gross Profit, Inventory Turnover, and ABC classification across all 104 SKUs.

**Completed:** June 2026 | **Certified by:** DataCamp

---

## Business Problem

> WarmeHands Inc. needs to decide which items to reorder for the next season. They want to know which products drive the most revenue, which categories have the highest cost, and which items are genuinely high-priority vs. low-priority inventory.

**Three key questions this dashboard answers:**
1. Which items should be renewed or increased in inventory?
2. How does performance break down by product category?
3. What is the geographic influence on sales (by country)?

---

## Dashboard Pages (8 Stages)

### 1. Preparing Data
Data quality check across 6 source tables: Stock (104 SKUs), Orders (~13,300 rows), Customer, Country, Price, and Costs. Verified relationships and resolved SKU key mismatches before any analysis.

### 2. COGS, Revenue & Profit
Calculated the three core financial metrics using DAX:
- **Revenue** = Quantity × Retail Price
- **COGS** = Sum of raw materials + labor + equipment rent + distribution + advertising
- **Gross Profit** = Revenue − COGS

Key finding: Home Accessories dominated 2020 profit at **$110K**.

### 3. Sales by Year
Year-over-year comparison of sales volumes. Identified products with growing vs. declining demand heading into 2021.

### 4. Inventory Turnover
Calculated how quickly each product moves through stock:
- **Average Inventory Value** = (Starting + Ending stock value) / 2
- **Inventory Turnover** = COGS / Average Inventory Value

Higher turnover = product sells fast relative to stock held. Doughnut Lip Gloss topped the list at **1.97x turnover**.

### 5. ABC Analysis
Classified all 104 SKUs by revenue contribution using cumulative % logic:
- **A [High Value]** — top 70% of revenue (~14 products)
- **B [Medium Value]** — next 20%
- **C [Low Value]** — bottom 10%

Top Class A insight: *"Grow a Flytrap or Sunflower"* is the highest revenue SKU.

### 6. Smart Purchasing
Identified which Class A and B items have low remaining 2021 stock — these are priority reorder candidates. Cross-referenced ABC class with inventory turnover to prioritize highest-ROI restocking decisions.

### 7. Category Inspection
Breakdown of COGS and performance by the 5 product categories. Decoration and Jewelry items have average COGS of ~$5, making them high-margin. Home Accessories dominate on both revenue and volume.

### 8. Management Solutions Dashboard (Final)
The complete executive dashboard consolidating all findings:

![Management Dashboard](screenshots/08_management_dashboard.png)

**Key metrics on the final dashboard:**
- **104** total SKUs
- **$268.92K** Total 2021 COGS
- **0.94** Average Inventory Turnover
- Revenue split: Class A = $0.32M | Class B = $0.10M | Class C = $0.05M

---

## Key DAX Measures

```dax
-- Revenue
Revenue = SUMX(Orders, Orders[Quantity] * RELATED(Price[Retail_Price]))

-- COGS
COGS = SUMX(Orders, Orders[Quantity] * (
    RELATED(Costs[raw_material]) + RELATED(Costs[factory_labor]) +
    RELATED(Costs[factory_equipment_rent]) + RELATED(Costs[distribution]) +
    RELATED(Costs[advertisement])
))

-- Gross Profit
Gross Profit = [Revenue] - [COGS]

-- Inventory Turnover
Inventory_Turnover = DIVIDE([COGS], [Avg_Inventory_Value])

-- ABC Classification
ABC_Class = 
    IF([CP_Revenue] <= 0.70, "A [High Value]",
    IF([CP_Revenue] <= 0.90, "B [Medium Value]", "C [Low Value]"))
```

See [`dax_measures.md`](dax_measures.md) for all measures with full formulas.

---

## Top Findings

| Finding | Detail |
|---|---|
| Top revenue SKU | Grow a Flytrap or Sunflower |
| Fastest-selling item | Doughnut Lip Gloss (1.97x turnover) |
| Rising product | Set of 6 Soldier Skittles (increased YoY) |
| Highest profit category | Home Accessories ($110K in 2020) |
| Lowest-cost categories | Decoration & Jewelry (~$5 COGS per unit) |
| Priority reorder class | Class A — 14 SKUs, $0.32M revenue |

---

## Data Model

Six source tables joined on SKU and InvoiceNo keys:
Categories (dim)     Price (dim)     Costs (dim)
└──────────────┬───────────────┘
│
Stock (dim)
│
Orders (fact) ──── Country (dim)
│
Customer (dim)
See [`data_model.md`](data_model.md) for full schema documentation.

---

## Repository Structure
powerbi-inventory-analysis/
│
├── 3_4_management_solutions_solution.pbix   # Complete final report
│
├── exercise-solutions/                      # All 13 exercise stages
│   ├── 1_1_preparing_data_solution.pbix
│   ├── 1_2_cleaning_and_exploring_data_solution.pbix
│   ├── 1_3_cogs_solution.pbix
│   ├── 1_4_revenue_and_profit_solution.pbix
│   ├── 2_1_sales_by_year_solution.pbix
│   ├── 2_2_inventory_turnover_solution.pbix
│   ├── 2_3_contrasting_revenue_solution.pbix
│   ├── 2_4_abc_analysis_solution.pbix
│   ├── 2_5_preliminary_results_solution.pbix
│   ├── 3_1_smart_purchasing_solution.pbix
│   ├── 3_2_Inspecting_categories_solution.pbix
│   ├── 3_3_control_decisions_solution.pbix
│   └── 3_4_management_solutions_solution.pbix
│
├── datasets/
│   ├── WarmeHands_data.xlsx                 # Source data: Orders, Stock, Price, Costs, Customer, Country
│   └── categories.csv                       # Product category lookup (104 SKUs)
│
├── screenshots/                             # All 8 dashboard page screenshots
├── dax_measures.md                          # Full DAX measure documentation
├── data_model.md                            # Data model and table relationships
└── README.md
---

## How to Open This Report

1. Download and install [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
2. Clone or download this repository
3. Open `3_4_management_solutions_solution.pbix`
4. Data is imported — no live connection needed, opens immediately

To explore the progression, open any file in `exercise-solutions/` in order from `1_1` through `3_4`.

---

## Related Projects

| Project | What it covers |
|---|---|
| [SQL Data Warehouse](https://github.com/dishajain-dataanalyst/sql-data-warehouse-project) | ETL pipeline, Medallion Architecture, star schema modeling |
| [SQL Data Analytics](https://github.com/dishajain-dataanalyst/sql-data-analytics-project) | 14 advanced SQL scripts — EDA, segmentation, time-series |
| [Power BI AdventureWorks](https://github.com/dishajain-dataanalyst/powerbi-adventureworks) | 8-page sales analytics report — drillthrough, What-If, AI visuals |
| [Alteryx Fitness Analysis](https://github.com/dishajain-dataanalyst/alteryx-fitness-data-analysis) | Data blending and workflow automation in Alteryx |
| **This repo** | Inventory analysis — Revenue, COGS, Profit, ABC Analysis, Turnover |

---

## License

MIT License — free to use and adapt with attribution.
Dataset: WarmeHands Inc. (DataCamp case study dataset).

---

## About Me

I'm **Disha Jain**, an Analytics & BI Professional with 3+ years of experience building Power BI dashboards, ETL pipelines, and data models in production. Microsoft Power BI Certified.

**Skills:** Power BI · DAX · SQL · Tableau · Python · Alteryx · Azure · ETL · Data Modeling 

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/dishadineshjain)
[![AdventureWorks Report](https://img.shields.io/badge/Related-Power_BI_AdventureWorks-blue?style=for-the-badge)](https://github.com/dishajain-dataanalyst/powerbi-adventureworks)
[![SQL Warehouse](https://img.shields.io/badge/Related-SQL_Warehouse_Project-blue?style=for-the-badge)](https://github.com/dishajain-dataanalyst/sql-data-warehouse-project)
