# DAX Measures — WarmeHands Inventory Analysis

All measures created during the case study, organized by chapter.

---

## Chapter 1: Revenue, COGS & Profit

### Revenue
Revenue = SUMX(Orders, Orders[Quantity] * RELATED(Price[Retail_Price]))

### COGS (Cost of Goods Sold)
COGS = SUMX(
Orders,
Orders[Quantity] * (
RELATED(Costs[raw_material]) +
RELATED(Costs[factory_labor]) +
RELATED(Costs[factory_equipment_rent]) +
RELATED(Costs[distribution]) +
RELATED(Costs[advertisement])
)
)

### Gross Profit
Gross Profit = [Revenue] - [COGS]

### Profit 2020
Profit_2020 = CALCULATE([Gross Profit], YEAR(Orders[InvoiceDate]) = 2020)

---

## Chapter 2: Inventory Metrics

### Average Inventory Value
Avg_Inventory_Value = (SUM(Stock[2021_start_stock]) * RELATED(Price[Retail_Price]) + [Ending Inventory Value]) / 2

### Inventory Turnover
Inventory_Turnover = DIVIDE([COGS], [Avg_Inventory_Value])

### Revenue 2021
Revenue_2021 = CALCULATE([Revenue], YEAR(Orders[InvoiceDate]) = 2021)

---

## Chapter 3: ABC Analysis

### Percent of Total Revenue
Pct_Total_Revenue = DIVIDE([Revenue_2021], CALCULATE([Revenue_2021], ALL(Stock)))

### Cumulative Percent of Revenue
CP_Revenue =
CALCULATE(
[Pct_Total_Revenue],
FILTER(
ALL(Stock),
[Revenue_2021] >= EARLIER([Revenue_2021])
)
)

### ABC Classification
ABC_Class =
IF([CP_Revenue] <= 0.70, "A [High Value]",
IF([CP_Revenue] <= 0.90, "B [Medium Value]", "C [Low Value]"))


