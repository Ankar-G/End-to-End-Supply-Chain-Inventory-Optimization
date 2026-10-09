# End-to-End Supply Chain & Inventory Optimization

**A four-page Power BI analytics solution for inventory health, demand planning, replenishment and supplier performance**

![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Measures-0078D4?style=flat)
![Domain](https://img.shields.io/badge/Domain-Supply_Chain_Analytics-2E7D32?style=flat)
![Data](https://img.shields.io/badge/Dataset-Simulated-lightgrey?style=flat)

**Domain:** Supply Chain Analytics | Inventory Optimization | Demand Planning | Procurement Analytics

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Problem Statement](#2-problem-statement)
3. [Business Objectives](#3-business-objectives)
4. [Project Workflow](#4-project-workflow)
5. [Dataset and Data Model](#5-dataset-and-data-model)
6. [Data Cleaning and Preparation](#6-data-cleaning-and-preparation)
7. [Key Calculations and Metrics](#7-key-calculations-and-metrics)
8. [Dashboard Overview](#8-dashboard-overview)
9. [Dashboard 1: Executive Supply Chain Overview](#9-dashboard-1-executive-supply-chain-overview)
10. [Dashboard 2: Inventory Analysis](#10-dashboard-2-inventory-analysis)
11. [Dashboard 3: Demand & Replenishment](#11-dashboard-3-demand--replenishment)
12. [Dashboard 4: Supplier Performance](#12-dashboard-4-supplier-performance)
13. [Key Project Insights](#13-key-project-insights)
14. [Business Value](#14-business-value)
15. [Business Questions Answered](#15-business-questions-answered)
16. [Technical Skills and Tools](#16-technical-skills-and-tools)
17. [Project Structure](#17-project-structure)
18. [Data Validation Notes](#18-data-validation-notes)
19. [Dashboard Preview](#19-dashboard-preview)
20. [Connect With Me](#20-connect-with-me)

---

## 1. Project Overview

This project simulates a real-world supply chain environment for a **fictional multinational company** and analyzes it through an interactive Power BI report. It uses a **simulated dataset**. It does not represent any real company, and no real-world deployment or results are claimed.

The analysis covers:

- Inventory levels and inventory valuation
- Stockouts and excess inventory
- Inventory turnover and working capital exposure
- Demand patterns and demand-supply gaps
- Reorder risks and replenishment requirements
- Supplier delivery reliability and fill rates
- Purchase order performance
- Warehouse- and product-level inventory performance

The four dashboards are designed to support inventory decisions, procurement planning, product availability and working capital management.

---

## 2. Problem Statement

Supply chain teams must keep products available without tying up excess capital in stock. This project addresses questions such as:

- Which products and categories hold the most inventory value?
- Where are stockouts and excess inventory concentrated?
- Which products require replenishment?
- How does demand vary over time?
- How well is demand being fulfilled?
- Which categories have higher stockout risk?
- Which suppliers have poor on-time delivery performance?
- How do supplier fill rates and lead times vary?
- Which warehouses or suppliers need operational improvement?
- How can inventory investment be better controlled?

---

## 3. Business Objectives

- **Inventory optimization:** Identify where inventory capital is concentrated and where stock is excessive or insufficient.
- **Demand analysis:** Understand demand seasonality and the gap between ordered and fulfilled quantities.
- **Replenishment planning:** Flag products below reorder point and estimate replenishment needs.
- **Supplier evaluation:** Compare suppliers on on-time delivery, fill rate, lead time and rating.
- **Warehouse performance analysis:** Compare stockout rates across distribution centers.
- **Operational decision support:** Give planners and managers a single, filterable view of supply chain health.

---

## 4. Project Workflow

| Phase | Purpose |
|---|---|
| 1. Data Collection | Assemble six related tables covering products, suppliers, demand, inventory, purchase orders and warehouses. |
| 2. Data Cleaning and Preparation | Check data quality and consistency before modeling (see Section 6). |
| 3. Data Modeling and Relationships | Link tables through `Product_ID`, `Supplier_ID` and `Warehouse_ID` so slicers filter across all pages. |
| 4. KPI and DAX Measure Development | Build measures for sales, inventory, turnover, stockout, fill rate, reorder risk and supplier metrics. |
| 5. Dashboard Development | Design four interlinked report pages with shared navigation and slicers. |
| 6. Business Insight Generation and Recommendations | Summarize findings and practical actions on each page. |

> Power Query is not listed as a project step because its use has not been confirmed. Add it here if it was used.

---

## 5. Dataset and Data Model

The dataset is simulated. Six tables are used.

### Tables

| Table | Intended Grain | Columns |
|---|---|---|
| **Products** | One row per product | `Product_ID`, `Product_Name`, `Category`, `Sub_Category`, `Unit_Cost`, `Unit_Price`, `Supplier_ID`, `Lead_Time_Days`, `Safety_Stock`, `Reorder_Point`, `Minimum_Order_Qty` |
| **Suppliers** | One row per supplier | `Supplier_ID`, `Supplier_Name`, `Supplier_Category`, `Country`, `Supplier_Rating`, `Payment_Terms`, `Contract_Status` |
| **Sales_Demand** | One row per order (or order line) | `Order_ID`, `Order_Date`, `Product_ID`, `Customer_ID`, `Warehouse_ID`, `Quantity_Ordered`, `Quantity_Fulfilled`, `Sales_Value`, `Order_Status` |
| **Inventory** | Daily snapshot per product and warehouse | `Inventory_Date`, `Product_ID`, `Warehouse_ID`, `Opening_Stock`, `Stock_Received`, `Units_Sold`, `Closing_Stock`, `Damaged_Units`, `Stockout_Flag`, `Inventory_Value` |
| **Purchase_Orders** | One row per purchase order (or PO line) | `PO_ID`, `PO_Date`, `Supplier_ID`, `Product_ID`, `Warehouse_ID`, `Quantity_Ordered`, `Quantity_Received`, `Expected_Date`, `Actual_Receipt_Date`, `Unit_Cost`, `PO_Status` |
| **Warehouses** | One row per warehouse | `Warehouse_ID`, `Warehouse_Name`, `City`, `State`, `Region`, `Warehouse_Type`, `Storage_Capacity`, `Operating_Cost` |

### Relationships

| From (One) | To (Many) | Key |
|---|---|---|
| Products | Sales_Demand | `Product_ID` |
| Products | Inventory | `Product_ID` |
| Products | Purchase_Orders | `Product_ID` |
| Suppliers | Products | `Supplier_ID` |
| Suppliers | Purchase_Orders | `Supplier_ID` |
| Warehouses | Sales_Demand | `Warehouse_ID` |
| Warehouses | Inventory | `Warehouse_ID` |
| Warehouses | Purchase_Orders | `Warehouse_ID` |

### Important Modeling Note: Inventory Snapshots

The `Inventory` table holds **repeated daily snapshots** for each product and warehouse. Historical `Closing_Stock` and `Inventory_Value` must **not** be summed across dates to get current inventory. Current inventory units and value should be taken from the **latest snapshot date** in the selected filter context.

### Calendar Table

The dashboards include Year slicers and monthly axes. A dedicated Calendar table and its relationships to `Inventory[Inventory_Date]`, `Sales_Demand[Order_Date]` and `Purchase_Orders[PO_Date]` have **not been confirmed**, so none is documented here. Add the relationships below if a Calendar table exists:

- `[Add confirmed relationship: Calendar → Inventory[Inventory_Date]]`
- `[Add confirmed relationship: Calendar → Sales_Demand[Order_Date]]`
- `[Add confirmed relationship: Calendar → Purchase_Orders[PO_Date]]`

---

## 6. Data Cleaning and Preparation

Items marked **Evident** are supported by how the dashboards behave. Items marked **Confirm** are good-practice checks that should be marked complete only if they were actually performed.

| Step | Status |
|---|---|
| Check duplicate records and missing values | Confirm |
| Standardize category, product, supplier and warehouse fields | Confirm (consistent labels appear across pages) |
| Validate date and numeric columns | Confirm |
| Check `Quantity_Fulfilled` against `Quantity_Ordered` | Confirm |
| Review inventory snapshot consistency | Confirm |
| Check `Actual_Receipt_Date` against `Expected_Date`; handle POs with no receipt date | Confirm |
| Create relationships between tables | Evident (slicers filter measures across tables) |
| Use the latest inventory snapshot for current-stock measures | Confirm (see Section 18 for open reconciliation items) |

---

## 7. Key Calculations and Metrics

The **Status** column separates logic stated in the project design from items that must be checked against the actual DAX before publishing.

- **Defined:** the logic is a stated project principle.
- **Confirm:** the logic is a reasonable reading that must be verified against the model.

| Metric | Business Meaning | Calculation / Logic | Status |
|---|---|---|---|
| Total Sales | Revenue from orders | Sum of `Sales_Value` | Confirm |
| Total Inventory Units | Current stock on hand | Sum of `Closing_Stock` on the **latest snapshot** (not summed across dates) | Defined |
| Inventory Value | Capital tied up in stock | Sum of `Inventory_Value` on the **latest snapshot** | Defined |
| Average Inventory | Typical stock level over the period | Average inventory units across the selected period; exact basis to be documented | Confirm |
| Inventory Turnover | How quickly stock is sold through | `Units Sold ÷ Average Inventory Units` **if** that is the implemented formula. It is not assumed to be a COGS-based turnover. | Confirm |
| Stockout Rate | Share of observations with no stock | Stockout observations (`Stockout_Flag`) ÷ total inventory observations | Confirm |
| Fill Rate | How much ordered demand was fulfilled | `Quantity_Fulfilled ÷ Quantity_Ordered` | Defined |
| Excess Inventory Units | Stock above the desired level | Units above the threshold (for example, the product's `Reorder_Point`); threshold to be documented | Confirm |
| Excess Inventory Value | Capital tied up in excess stock | Excess units × unit cost (or inventory value per unit); to be documented | Confirm |
| Excess Inventory Rate | Excess as a share of inventory | Excess ÷ total inventory; numerator and denominator basis to be documented (see Section 18) | Confirm |
| Total Demand | Total quantity customers requested | Sum of `Quantity_Ordered` in `Sales_Demand` | Confirm |
| Average Daily Demand | Typical daily demand | Total demand ÷ number of days (or daily average); basis to be documented | Confirm |
| Reorder Risk | Products at risk of running short | Count of products whose latest closing stock is below `Reorder_Point` | Confirm |
| Reorder Gap | Shortfall versus reorder point | `MAX(0, Reorder_Point − Closing_Stock)` using the latest snapshot | Defined |
| Recommended Replenishment Quantity | Suggested quantity to restock | Implemented logic to be documented. **Limitations:** may not account for open purchase orders or `Minimum_Order_Qty`. | Confirm |
| Average Supplier Lead Time | Typical supplier lead time | Average `Lead_Time_Days` (supplier master data) unless actual delivery lead time is implemented | Confirm |
| Supplier On-Time Delivery % | Reliability of deliveries | POs received **on or before** `Expected_Date` ÷ POs **with an actual receipt date** (POs with no receipt date are excluded from the denominator) | Defined |
| Supplier Fill Rate % | Completeness of supplier deliveries | `Quantity_Received ÷ Quantity_Ordered` | Defined |
| Total Purchase Order Value | Procurement spend | Sum of `Quantity_Ordered × Unit_Cost` across POs | Defined |
| Total Purchase Orders | Procurement volume | Count of `PO_ID` | Confirm |
| Total Quantity Purchased | Procurement volume in units | Sum of `Quantity_Ordered` **or** `Quantity_Received`; the basis must be stated | Confirm |

> The currency is not specified in the dataset or the dashboards. Monetary values in this README are shown without a currency symbol.

---

## 8. Dashboard Overview

| # | Dashboard | Purpose | Decisions Supported |
|---|---|---|---|
| 1 | Executive Supply Chain Overview | One-page health check of sales, inventory, stockouts and fulfillment | Where management should focus first |
| 2 | Inventory Analysis | Inventory value, health, turnover and excess stock | Which categories and products need inventory intervention |
| 3 | Demand & Replenishment | Demand trends, reorder risk, lead times and replenishment needs | What to reorder, and how to prepare for demand peaks |
| 4 | Supplier Performance | Delivery reliability, fill rate, lead time and PO activity | Which suppliers to develop, monitor or diversify away from |

All pages share consistent navigation and slicers (Region, Warehouse, Category, Supplier or Product, and Year, depending on the page).

---

## 9. Dashboard 1: Executive Supply Chain Overview

**Business question:** How healthy is the overall supply chain, and where should management focus first?

### KPIs

| KPI | Value |
|---|---:|
| Total Sales | 368M |
| Inventory Value | 28.84M |
| Inventory Turnover | 9.86 |
| Stockout Rate | 4.59% |
| Fill Rate | 92.85% |
| Excess Inventory Value | 1.08M |

### Visuals

| Visual | What It Shows | What It Helps the Business Understand |
|---|---|---|
| Monthly Sales Trend | Sales peak in May at 46M; H2 months are much lower (20M–25M) | Seasonality and when to build stock |
| Inventory Value by Category | Electronics Accessories 37.91%, Food & Staples 15.01%, Personal Care 10.67%, Health & Wellness 9.92%, Home Care 8.66%, Baby Care 5.29%, Snacks & Confectionery 4.39%, Home & Kitchen 3.33% | Where inventory capital is concentrated |
| Top 10 Products by Inventory Value | Portable Bluetooth Speaker and Power Bank 10000 mAh lead | Which individual products dominate inventory investment |
| Stockout Rate by Warehouse | Lucknow 6.76%, Kolkata 6.09%, Hyderabad 5.61%, Chennai 5.45%, Bengaluru 5.13%, Bhiwandi 3.51%, Gurugram 3.37%, Ahmedabad 1.79% | Which distribution centers have the weakest availability |

> **Needs correction before publishing:** The Top 10 chart in the current screenshot shows bar values such as 231M (Portable Bluetooth Speaker) and 133M (Power Bank 10000 mAh). These exceed the 28.84M total Inventory Value, so they are likely not latest-snapshot values. The corrected values are not available here. Use `[Add corrected value]` placeholders until the measure is fixed. See Section 18.

### Insights

- Sales peak in May at 46M. First-half months (January–June) account for roughly 64% of annual sales, based on the chart labels.
- Electronics Accessories holds the largest share of inventory value (37.91%).
- Lucknow and Kolkata have the highest stockout rates, more than three times the rate of Ahmedabad.
- Fill rate of 92.85% means about 7% of ordered quantity is not being fulfilled, even though excess inventory value exists (1.08M). This points to an inventory mix imbalance rather than a simple shortage or surplus.

### Recommendations

- Prepare inventory ahead of seasonal demand peaks.
- Prioritize replenishment checks in high-stockout warehouses (Lucknow, Kolkata).
- Review stock allocation between categories to reduce excess and improve order fulfillment.

---

## 10. Dashboard 2: Inventory Analysis

**Business question:** Where is inventory capital tied up, and which products need inventory intervention?

### KPIs

| KPI | Value |
|---|---:|
| Total Inventory Units | 154K |
| Inventory Value | 28.84M |
| Average Inventory | 143.85K |
| Inventory Turnover | 9.86 |
| Excess Inventory Rate | 0.90% |
| Stockout Rate | 4.59% |

### Inventory Value by Category

| Category | Inventory Value |
|---|---:|
| Electronics Accessories | 10.94M |
| Food & Staples | 4.33M |
| Personal Care | 3.08M |
| Health & Wellness | 2.86M |
| Home Care | 2.50M |
| Baby Care | 1.52M |
| Snacks & Confectionery | 1.27M |
| Beverages | 1.02M |
| Home & Kitchen | 0.96M |
| Stationery | 0.37M |

The ten categories sum to about 28.85M, which is consistent with the 28.84M total after rounding.

### Other Visuals and Findings

| Visual | Findings |
|---|---|
| Inventory Health | Low Stock 106K, Healthy 37K, Excess 11K, Stockout 0K (shown as a separate category on the chart). The four values sum to about 154K, matching Total Inventory Units. |
| Inventory Turnover by Category | Snacks & Confectionery 22x, Beverages 19x, Food & Staples 15x, Home Care 14x, Stationery 14x, Baby Care 10x, Personal Care 10x, Health & Wellness 9x, Electronics Accessories 5x (chart is scrollable; some categories may be hidden) |
| Top Products by Excess Inventory Value | Whey Protein 1 kg 998.28K, Laundry Detergent Powder 2 kg 41.2K, Baby Lotion 200 ml 30.49K, Dishwash Liquid 750 ml 4.32K, Gel Pens Box of 10 4.09K (the visual is titled "Top 10" but five products are visible) |
| Stockout vs Excess Inventory by Category | The values shown (Health & Wellness 998.28K, Home Care 45.52K, Baby Care 30.49K, Stationery 4.09K) match **excess inventory value** by category. No separate stockout measure is visible, so the visual should be retitled or a second measure added. |

### Insights

- Electronics Accessories holds the most inventory value (10.94M) and has the slowest visible turnover (5x).
- Snacks & Confectionery turns over about four times faster than Electronics Accessories.
- Low Stock units (106K) far exceed Excess units (11K), so under-stocking is the larger issue in unit terms.
- Excess inventory value is highly concentrated. Whey Protein 1 kg accounts for 998.28K of the 1.08M total (over 90%).

### Recommendations

- Review slow-moving inventory, especially Electronics Accessories, and avoid overstocking.
- Replenish products below safety-stock levels.
- Consider transferring excess stock to warehouses facing shortages.
- Review demand forecasts before placing new orders for Whey Protein 1 kg.

> **Reconciliation needed:** the Excess Inventory Rate (0.90%) does not match Excess units ÷ Total units (11K ÷ 154K ≈ 7%) or Excess value ÷ Inventory Value (1.08M ÷ 28.84M ≈ 3.7%). Confirm the numerator, denominator and the health-category thresholds. See Section 18.

---

## 11. Dashboard 3: Demand & Replenishment

**Business question:** Which products require replenishment, and how can the company prepare for demand and supply gaps?

### KPIs

| KPI | Value |
|---|---:|
| Total Demand | 2M |
| Average Daily Demand | 4.21K |
| Stockout Rate | 4.59% |
| Reorder Risk | 39 products |
| Average Lead Time | 14.33 days |
| Recommended Replenishment Quantity | 173K |

### Visuals and Findings

| Visual | Findings |
|---|---|
| Demand Trend | Monthly demand peaks in May at 292K; lowest months are July–September (118K–123K) |
| Demand vs Supply by Month | Ordered quantity exceeds fulfilled quantity in every month; the gap is visible in the peak months |
| Stockout Risk by Category | Snacks & Confectionery 11.75%, Beverages 7.69%, Stationery 4.70%, Home Care 4.27%, Electronics Accessories 3.92%, Food & Staples 3.39%, Personal Care 2.22%, Health & Wellness 1.60%, Baby Care 0.77%, Home & Kitchen 0.64% |
| Products Below Reorder Point | USB-C Fast Charging Cable 1 m (reorder gap 20K), Milk Chocolate Bar 40 g (12K), 20W Wall Charger (11K), Instant Masala Noodles 12-Pack (9K), Cream Biscuits 400 g (8K), Refined Sunflower Oil 1 L (8K), Portable Bluetooth Speaker (7K), Iodised Salt 1 kg (6K) |
| Average Lead Time by Category | Electronics Accessories 37 days, Home & Kitchen 31, Personal Care 14.6, Health & Wellness 12, Baby Care 11.5, Home Care 8.25, Stationery 7.5 |

### Insights

- Demand peaks in May (292K), and about 63% of annual demand falls in January–June (based on the chart labels).
- Snacks & Confectionery has the highest stockout risk (11.75%) and also the highest turnover (22x). It is a fast-moving category where availability is hard to maintain.
- Electronics Accessories combines the longest lead time (37 days) with the largest inventory value and slowest turnover.
- 39 products are at reorder risk. USB-C Fast Charging Cable 1 m has the largest reorder gap (20K).

### Recommendations

- Forecast seasonal demand and place purchase orders early for the January–June peak.
- Prioritize replenishment of high-risk products, starting with the largest reorder gaps.
- Increase safety stock where demand variability and long lead times justify it, particularly for fast-moving snacks and beverages.
- Review long-lead-time categories (Electronics Accessories, Home & Kitchen) for earlier ordering or alternative sourcing.

> **Validate before acting:** The 173K Recommended Replenishment Quantity should be checked against available inventory, open purchase orders and minimum order quantities if these are not built into the measure.

---

## 12. Dashboard 4: Supplier Performance

**Business question:** Which suppliers are reliable, and where are procurement risks concentrated?

### KPIs

| KPI | Value |
|---|---:|
| Supplier On-Time Delivery | 67.43% |
| Supplier Fill Rate | 98.20% |
| Average Lead Time | 14.33 days |
| Total Purchase Order Value | 271.54M |
| Total Quantity Purchased | 2M |

### Supplier Comparison (from the dashboard charts and scorecard)

| Supplier | On-Time Delivery | Fill Rate | Avg Lead Time (days) | Avg Rating |
|---|---:|---:|---:|---:|
| PureGlow Personal Care Ltd | 87.96% | 99.84% | 9.00 | 4.50 |
| Kisan Harvest Foods Pvt Ltd | 82.35% | 99.57% | n/a | 4.70 |
| Little Sprouts Baby Products Pvt Ltd | 82.14% | 99.76% | 11.50 | 4.20 |
| PaperCraft Stationers Ltd | 80.77% | 99.69% | n/a | 4.30 |
| CoolWave Beverages Pvt Ltd | 74.23% | 97.98% | n/a | 4.10 |
| HomeFresh Chemicals Pvt Ltd | 71.81% | 98.96% | 8.25 | 3.40 |
| Siam Beauty Labs Co Ltd | 71.15% | 99.65% | 23.00 | 3.80 |
| Greenfield Agro Processing Ltd | 65.08% | 99.17% | n/a | 3.60 |
| Saigon Plastics & Housewares JSC | 52.17% | 98.69% | 31.00 | n/a |
| VitaWell Nutraceuticals Pvt Ltd | 51.32% | 98.38% | 12.00 | 3.50 |
| Crispo Snacks Industries | 49.10% | 96.36% | n/a | n/a |
| Guangzhou TechParts Co Ltd | 43.14% | 98.62% | 33.00 | n/a |
| Shenzhen Brightlink Electronics Co Ltd | 32.76% | 97.81% | 39.00 | n/a |

*n/a = not visible in the screenshots (charts and scorecard are scrollable). Overall average rating shown in the scorecard total is 3.65.*

### Other Visual

| Visual | Findings |
|---|---|
| Purchase Order Performance Trend | Monthly PO count peaks at 182 in May; lowest months are July–December (73–90) |

### Insights

- Supplier fill rate is strong (98.20%), but on-time delivery is weak (67.43%). Suppliers deliver nearly the full quantity, but often late.
- Shenzhen Brightlink has the lowest on-time delivery (32.76%) and the longest lead time (39 days).
- Guangzhou TechParts and Saigon Plastics also combine long lead times (33 and 31 days) with on-time delivery at or below about 52%.
- Supplier ratings broadly follow on-time performance (Kisan, PureGlow, PaperCraft, Little Sprouts rate highest), but not perfectly. HomeFresh has 71.81% on-time delivery yet the lowest visible rating (3.40).
- PO activity is concentrated in the first half of the year, in line with demand.

### Recommendations

- Investigate delays with the lowest on-time suppliers, starting with Shenzhen Brightlink, Guangzhou TechParts and Crispo Snacks.
- Track on-time delivery and fill rate separately; high fill rate can hide late deliveries.
- Use the scorecard in supplier reviews and compare static ratings against actual delivery performance.
- Evaluate alternative or backup sourcing for critical products supplied by low-reliability, long-lead-time suppliers.
- Build delivery delays into reorder points and safety stock for these suppliers.

### Lead Time: Master Data vs. Actual

The Average Lead Time (14.33 days overall; 39 days for Shenzhen Brightlink) appears to be based on **supplier master-data lead time** (`Products[Lead_Time_Days]`). The values are whole numbers or simple averages, such as 8.25 and 11.50. It is **not confirmed** to be actual delivery lead time (receipt date compared with PO or expected date). If an actual-lead-time or delay-in-days measure is added, present it separately from the master-data figure.

> **Visual issue:** In the Purchase Order Performance Trend, the "PO On-Time Delivery %" series appears as a flat line at 1 across all months. This looks like a scaling or formatting issue, so no on-time trend is reported here.

---

## 13. Key Project Insights

### Inventory Efficiency
- Electronics Accessories holds 37.91% of inventory value but has the slowest turnover among visible categories (5x).
- Snacks & Confectionery (22x) and Beverages (19x) turn over fastest.
- Overall turnover is 9.86 (formula to be confirmed, see Section 7).

### Demand and Replenishment
- Demand peaks in May (292K) and falls sharply from July.
- 39 products are at reorder risk, and the recommended replenishment quantity is 173K units, to be validated against open POs and minimum order quantities.
- Fill rate is 92.85%, so about 7% of ordered quantity goes unfulfilled.

### Warehouse Performance
- Stockout rates range from 1.79% (Ahmedabad) to 6.76% (Lucknow).
- Lucknow and Kolkata are the clearest candidates for stock rebalancing and replenishment review.

### Supplier Reliability
- On-time delivery (67.43%) is the main supplier weakness; fill rate (98.20%) is not.
- Long-lead-time suppliers (Shenzhen Brightlink, Guangzhou TechParts, Saigon Plastics) show some of the lowest on-time rates.

### Working Capital and Operational Risk
- Inventory value is 28.84M, of which 1.08M is excess.
- Excess value is highly concentrated: Whey Protein 1 kg alone accounts for 998.28K.
- Low-stock units (106K) outweigh excess units (11K), so the main risk is availability rather than overstock.

---

## 14. Business Value

If applied to a real operation, this type of solution could potentially help supply chain teams with:

| Area | Potential Benefit |
|---|---|
| Inventory optimization | Spot slow-moving and excess stock and rebalance inventory investment |
| Stockout prevention | Identify high-risk categories, warehouses and products earlier |
| Working capital monitoring | Track inventory value and excess stock against turnover |
| Demand and replenishment planning | Prepare for seasonal peaks and prioritize reorders |
| Supplier performance management | Support supplier reviews with consistent delivery and fill-rate metrics |
| Warehouse performance monitoring | Compare availability across distribution centers |
| Data-driven procurement decisions | Inform sourcing and safety-stock decisions with lead-time and reliability data |

These are potential benefits. The project uses simulated data, and no actual savings or stockout reductions are claimed.

---

## 15. Business Questions Answered

**Executive Overview**
1. How much have we sold, and in which months does sales peak?
2. What is the total inventory value, and how is it split by category?
3. Which products hold the most inventory value?
4. Which warehouses have the highest stockout rates?
5. What share of ordered quantity is being fulfilled?

**Inventory Analysis**
6. How many inventory units are on hand, and how are they classified (low, healthy, excess)?
7. Which categories turn over inventory fastest and slowest?
8. Which products carry the most excess inventory value?
9. How concentrated is excess inventory across categories?

**Demand & Replenishment**
10. How does demand vary by month?
11. How large is the gap between ordered and fulfilled quantities?
12. Which categories face the highest stockout risk?
13. Which products are below their reorder point, and by how much?
14. How many products are at reorder risk, and what replenishment quantity is suggested?
15. Which categories have the longest supplier lead times?

**Supplier Performance**
16. How reliable are suppliers in delivering on time?
17. Which suppliers have the lowest and highest fill rates?
18. How do lead time, delivery performance and rating compare across suppliers?
19. How much is spent on purchase orders, and how does PO volume vary by month?

---

## 16. Technical Skills and Tools

| Category | Tool / Skill | Status |
|---|---|---|
| BI and visualization | Microsoft Power BI | Used |
| Calculations | DAX measures | Used |
| Data modeling | Relational model (six tables, one-to-many relationships) | Used |
| Reporting | Interactive multi-page dashboards, slicers, page navigation | Used |
| Analytics | Inventory, demand, replenishment and supplier analysis | Used |

> Excel, SQL and Power Query are not listed because their use in this project has not been confirmed. Add them here only if they were actually used (for example, Excel or CSV for source data, Power Query for transformations).

---

## 17. Project Structure

Filenames are **placeholders**. Replace them with the actual names in the repository.

```
End-to-End-Supply-Chain-Inventory-Optimization/
│
├── data/
│   ├── Products.[csv/xlsx]               # placeholder
│   ├── Suppliers.[csv/xlsx]              # placeholder
│   ├── Sales_Demand.[csv/xlsx]           # placeholder
│   ├── Inventory.[csv/xlsx]              # placeholder
│   ├── Purchase_Orders.[csv/xlsx]        # placeholder
│   └── Warehouses.[csv/xlsx]             # placeholder
│
├── dashboard/
│   └── Supply_Chain_Inventory_Optimization.pbix   # placeholder
│
├── screenshots/
│   ├── 01_executive_overview.png         # placeholder
│   ├── 02_inventory_analysis.png         # placeholder
│   ├── 03_demand_replenishment.png       # placeholder
│   └── 04_supplier_performance.png       # placeholder
│
└── README.md
```

---

## 18. Data Validation Notes

These items could not be validated from the dashboard screenshots. They should be resolved before the project is shared.

| # | Item | Observation | Action |
|---|---|---|---|
| 1 | Top 10 Products by Inventory Value | Bars show 231M, 133M, 102M and similar values, which exceed the 28.84M total inventory value. The chart probably sums historical snapshots. | Rebuild the visual using the latest-snapshot Inventory Value measure and update the README values. |
| 2 | Excess Inventory Rate | 0.90% does not match 11K ÷ 154K (about 7%) or 1.08M ÷ 28.84M (about 3.7%). | Document the exact numerator and denominator, or correct the measure. |
| 3 | Inventory Health thresholds | Low Stock (106K) is about 69% of units, while Reorder Risk is only 39 products. Thresholds for Low, Healthy and Excess are not documented. | Document threshold logic and confirm it uses the latest snapshot. |
| 4 | Stockout in Inventory Health | Stockout shows 0K on the latest snapshot while the Stockout Rate is 4.59% across history. | Confirm this is expected given different bases (latest snapshot vs. all observations). |
| 5 | "Stockout vs Excess Inventory" visual | Only excess inventory value by category is visible; no stockout measure is shown. The visual title contains a typo ("Inventroy"). | Retitle or add a stockout measure. |
| 6 | "Top 10 Products by Excess Inventory Value" | Only five products are visible. | Confirm whether additional products exist or retitle the visual. |
| 7 | Inventory Turnover (9.86) | Cannot be reproduced from displayed figures. 9.86 × 143.85K average inventory is about 1.42M units, which does not match Total Demand (about 2.2M by monthly sum). | Document the exact formula and numerator. |
| 8 | Average Daily Demand (4.21K) | Monthly demand sums to about 2.2M, which implies about 528 days at 4.21K per day. Date range and averaging basis are unclear. | Confirm the date range and whether Year filters combine multiple years. |
| 9 | Lead time basis | Values look like master-data averages, not actual delivery lead time. | Confirm, and add an actual-delay measure if needed. |
| 10 | Total Quantity Purchased | Not stated whether this is ordered or received quantity. | Specify the basis. |
| 11 | PO On-Time Delivery % trend | Series appears as a flat line at 1. | Fix the axis or format, or remove the series. |
| 12 | Currency | Dashboard insight text uses "$", while warehouses are in India and the currency is not defined in the data. | Confirm and apply a consistent currency label. |
| 13 | Calendar table | Existence not confirmed. | Confirm and document relationships, or remove the Year/Month logic note. |
| 14 | Navigation labels | "Executive Overview" appears misspelled ("Execctuive") on page buttons. | Fix the label text. |

---

## 19. Dashboard Preview

### 1. Executive Supply Chain Overview
![Workforce Overview](https://github.com/Ankar-G/Workforce-Planning-Employee-Performance-Analytics/blob/main/Dashboards%20Screenshot/Screenshot%202026-10-03%20124756.png)

<!-- Example: ![Executive Overview](screenshots/01_executive_overview.png) -->

### 2. Inventory Analysis
`[Add Inventory Analysis screenshot URL]`

### 3. Demand & Replenishment
`[Add Demand & Replenishment screenshot URL]`

### 4. Supplier Performance
`[Add Supplier Performance screenshot URL]`

---

## 20. Connect With Me

If you have feedback or would like to discuss this project, feel free to reach out.

- **LinkedIn:** `[Add LinkedIn profile URL]`
- **Email:** `[Add email address]`

---

*This project uses a simulated dataset for a fictional company and is intended for portfolio and learning purposes only.*
