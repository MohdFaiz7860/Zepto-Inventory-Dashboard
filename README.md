# 📦 Zepto Inventory Dashboard — Power BI Project

![Dashboard Preview](Zepto_Dashboard.png)

---

## 📋 Project Overview

This is an **interactive Inventory Dashboard** built in Power BI using Zepto quick-commerce product data. The dashboard provides a complete view of stock status, category performance, inventory value, and discount analysis across 3,732 products in 14 categories.

---

## 📁 Project Files

| File | Description |
|---|---|
| `Zepto_Inventory_Dashboard.pbix` | Main Power BI project file |
| `zepto_inventory_powerbi.xlsx` | Cleaned data source (Excel) |
| `Zepto_Dashboard.png` | Dashboard screenshot preview |
| `README.md` | This file |

---

## 📊 Data Source

**Original file:** `zepto_v1.xlsx`  
**Cleaned file:** `zepto_inventory_powerbi.xlsx`

### Data Cleaning Applied
- Prices converted from **paise → ₹** (divided by 100)
- `outOfStock` TRUE/FALSE converted to readable `Stock_Status` (In Stock / Out of Stock)
- Added calculated columns: `Discount_Amount_INR`, `Inventory_Value_INR`, `MRP_Value_INR`
- Added category columns: `Discount_Tier`, `Stock_Level`, `Price_Segment`

### Sheets in Excel File

| Sheet | Rows | Purpose |
|---|---|---|
| `Inventory_Data` | 3,732 | Main fact table |
| `Category_Summary` | 14 | Pre-aggregated category KPIs |
| `Stock_Alerts` | 999 | Products with qty ≤ 2 |
| `Discount_Analysis` | 5 | Discount tier breakdown |

---

## 📐 Dashboard Structure

### Page 1 — Main Inventory Dashboard

```
┌─────────────────────────────────────────────────────┐
│  🛵 Zepto   10 minute delivery          [Header]    │
├──────────┬─────────────────┬────────────────────────┤
│  KPI     │  Category       │  Price_Segment  Stock  │
│  Cards   │  Slicer         │  Slicer         Slicer │
├──────────┼─────────────────┼────────────────────────┤
│ ₹2.2M    │  Stock Status   │  Products    Inventory │
│ Total    │  Donut Chart    │  by Category  Value by │
│ Inventory│                 │  Bar Chart   Category  │
│ 3732     │                 │              Bar Chart │
│ Products │                 │                        │
│ 3279     │                 │                        │
│ In Stock │                 │                        │
│ 453      │                 │                        │
│ OOS      │                 │                        │
└──────────┴─────────────────┴────────────────────────┘
```

### Visuals Included

| Visual | Type | Data Source |
|---|---|---|
| Total Inventory Value | KPI Card | `_Measures` |
| Total Products | KPI Card | `_Measures` |
| In Stock Count | KPI Card | `_Measures` |
| Out of Stock Count | KPI Card | `_Measures` |
| Stock Status Split | Donut Chart | `Inventory_Data` |
| Products by Category | Bar Chart | `Inventory_Data` |
| Inventory Value by Category | Bar Chart | `Category_Summary` |
| Category Filter | Slicer | `Inventory_Data` |
| Price Segment Filter | Slicer | `Inventory_Data` |
| Stock Status Filter | Slicer | `Inventory_Data` |

---

## 🧮 DAX Measures

All measures are stored in the `_Measures` table.

```dax
Total Products = COUNTROWS(Inventory_Data)

In Stock Count =
COUNTROWS(FILTER(Inventory_Data, Inventory_Data[Stock_Status] = "In Stock"))

Out of Stock Count =
COUNTROWS(FILTER(Inventory_Data, Inventory_Data[Stock_Status] = "Out of Stock"))

OOS Rate % =
DIVIDE([Out of Stock Count], [Total Products]) * 100

Total Inventory Value =
SUMX(Inventory_Data, Inventory_Data[Inventory_Value_INR])

Avg Discount % =
AVERAGE(Inventory_Data[Discount_Percent])

Avg Selling Price =
AVERAGE(Inventory_Data[Selling_Price_INR])

Zero Stock =
COUNTROWS(FILTER(Inventory_Data, Inventory_Data[Available_Qty] = 0))

Low Stock =
COUNTROWS(FILTER(Inventory_Data, Inventory_Data[Available_Qty] > 0 && Inventory_Data[Available_Qty] <= 2))

Total Alerts =
COUNTROWS(FILTER(Inventory_Data, Inventory_Data[Available_Qty] <= 2))
```

---

## 🎨 Theme & Colors

| Element | Color |
|---|---|
| Background | `#0B1622` |
| Card Background | `#121F30` |
| Border | `#1E3250` |
| Accent Blue | `#4FC3F7` |
| In Stock Green | `#00C875` |
| Out of Stock Red | `#FF6B6B` |
| Warning Yellow | `#FFD166` |
| Purple Accent | `#A78BFA` |
| Text | `#E2EAF4` |
| Muted Text | `#6B8CAE` |

**Theme file:** Import `zepto_theme.json` via **View → Themes → Browse for themes**

---

## 📈 Key Metrics (as of data snapshot)

| Metric | Value |
|---|---|
| Total Products | 3,732 |
| Total Categories | 14 |
| In Stock | 3,279 (87.9%) |
| Out of Stock | 453 (12.1%) |
| Total Inventory Value | ₹22,43,081 |
| Low Stock Alerts (Qty ≤ 2) | 999 products |
| Largest Category | Cooking Essentials & Munchies (514 each) |
| Highest Inventory Value | Cooking Essentials & Munchies (₹3,37,369 each) |

---

## 🚀 How to Open

1. Install **Power BI Desktop** — [Download free](https://powerbi.microsoft.com/desktop)
2. Open `Zepto_Inventory_Dashboard.pbix`
3. If data doesn't load → **Home → Transform Data → Data Source Settings** → update path to `zepto_inventory_powerbi.xlsx`
4. Click **Refresh** to reload data

---

## 🔄 How to Refresh Data

If the Excel file is updated with new data:

1. Open the `.pbix` file in Power BI Desktop
2. Click **Home → Refresh**
3. All visuals update automatically

To change the data source path:
1. **Home → Transform Data → Data Source Settings**
2. Click **Change Source**
3. Browse to the new Excel file location

---

## 🛠 Requirements

| Tool | Version |
|---|---|
| Power BI Desktop | Latest (free) |
| Microsoft Excel | 2016 or later (to view Excel file) |
| OS | Windows 10/11 |

---

## 📌 Column Reference

| Column | Description | Source |
|---|---|---|
| `Category` | Product category | Original |
| `Product_Name` | Product name | Cleaned |
| `MRP_INR` | Maximum retail price in ₹ | Calculated (÷100) |
| `Selling_Price_INR` | Discounted price in ₹ | Calculated (÷100) |
| `Discount_Amount_INR` | MRP minus selling price | Calculated |
| `Discount_Percent` | Discount percentage | Original |
| `Available_Qty` | Units available (0–6) | Original |
| `Stock_Status` | In Stock / Out of Stock | Calculated |
| `Inventory_Value_INR` | Selling price × available qty | Calculated |
| `MRP_Value_INR` | MRP × available qty | Calculated |
| `Discount_Tier` | 0% / 1-10% / 11-20% / 21-30% / 30%+ | Calculated |
| `Stock_Level` | Zero / Low / Medium / High | Calculated |
| `Price_Segment` | Under ₹50 / ₹50-₹99 / ₹100-₹199 etc. | Calculated |

---

## 👤 Author

**Project:** Zepto Inventory Dashboard  
**Tool:** Microsoft Power BI Desktop    

---

*Built with Power BI Desktop. Data cleaned and prepared using Python (pandas + openpyxl).*
