# Data Model — WarmeHands Inventory Analysis

## Schema Overview
Categories (dim)          Price (dim)       Costs (dim)
│                      │                  │
└──────────────┬────────┘                  │
│                           │
Stock (dim) ────────────────────── │
│                           │
Orders (fact) ─────── Country (dim)
│
Customer (dim)

## Tables

| Table | Type | Rows | Key Fields |
|---|---|---|---|
| Orders | Fact | ~13,300 | InvoiceNo, SKU, Quantity, InvoiceDate, Country |
| Stock | Dimension | 104 | SKU-ID, Description, 2020_units_sold, 2021_start_stock |
| Price | Dimension | 104 | ID (SKU), Retail_Price |
| Costs | Dimension | 106 | SKU, raw_material, factory_labor, factory_equipment_rent, distribution, advertisement |
| Categories | Dimension | 104 | ID (SKU), category |
| Customer | Dimension | ~6,500 | CustomerID, InvoiceDate |
| Country | Dimension | ~13,300 | InvoiceNo, Country |

## Relationships

| From | To | Key | Type |
|---|---|---|---|
| Orders[SKU] | Stock[SKU-ID] | SKU | Many-to-One |
| Orders[SKU] | Price[ID] | SKU | Many-to-One |
| Orders[SKU] | Costs[SKU] | SKU | Many-to-One |
| Orders[SKU] | Categories[ID] | SKU | Many-to-One |
| Orders[InvoiceNo] | Country[InvoiceNo] | InvoiceNo | Many-to-One |
| Orders[InvoiceNo] | Customer[InvoiceDate] | InvoiceDate | Many-to-One |

## Categories
Products are grouped into 5 categories:
- **Home Accessories** — highest profit contributor (~$110K in 2020)
- **Decoration**
- **Toys & Edibles**
- **Office & School**
- **Jewelry**
