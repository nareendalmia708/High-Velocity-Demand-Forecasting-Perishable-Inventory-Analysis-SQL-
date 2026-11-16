# ⏱️ High-Velocity Demand Forecasting & Perishable Inventory Analysis (SQL)

## 📌 Project Overview

This project analyzes a high-frequency relational transactional database (**21,350 orders** and **49,574 units**) using **MySQL** to solve core **Quick-Commerce / Perishable Supply Chain, Hourly Demand Forecasting, and Shift Capacity Planning** challenges.

In perishable and fast-turnaround supply chains, inaccurate demand forecasting causes either costly ingredient spoilage or peak-hour fulfillment bottlenecks. The objective of this analysis was to map **intraday demand peaks, category-level ingredient turnover, and SKU ABC contribution** to optimize raw material procurement and workforce shift scheduling.

---

## 🗄️ Relational Schema & Supply Chain Data Model

The database consists of four normalized tables connecting **Order Timestamps**, **Line-Item Fulfillment**, and **SKU Master Data**:

- **`Orders` (Order Fulfillment Header):** Captures order timestamps (`date`, `time`) for intraday and seasonal demand velocity tracking.
- **`Order_Details` (Line-Item Demand):** Tracks exact unit quantities fulfilled per order line.
- **`Pizzas` (SKU & Pricing Master):** Links specific SKU sizes/variants to unit pricing.
- **`Pizza_Types` (Product Category & BOM Grouping):** Categorizes SKUs into 4 core operational groups (`Classic`, `Supreme`, `Veggie`, `Chicken`) for raw material and prep planning.

### Entity Relationship Flow
```
Orders (Timestamps) ──► Order_Details (Unit Demand) ──► Pizzas (SKU Variants) ──► Pizza_Types (Category/BOM)
```
![Table_Structure](screenshots/table_structure.pdf)

---

## 🛠️ Tools & SQL Techniques Applied

- **Database Engine:** MySQL Workbench
- **Core SQL Operations:** Multi-table `INNER JOIN`s, Time-Based Extractions (`HOUR()`, `DATE()`), Aggregations (`SUM`, `COUNT`, `AVG`)
- **Advanced Analytics:** Subqueries, Window Functions (`SUM() OVER` for cumulative demand trajectories), and Ranking Functions (`RANK() OVER (PARTITION BY ...)` for category-level SKU prioritization)

---

## 🎯 Supply Chain & Operations Problems Solved

### 🔹 Intraday Demand Velocity & Capacity Planning
- **Hourly Order Distribution:** Mapped order arrival rates across 24-hour cycles to pinpoint peak fulfillment windows and prevent kitchen/dispatch bottlenecks.
- **Average Daily Throughput:** Calculated baseline daily unit demand (**138 units/day**) to establish daily par levels for perishable raw materials.

### 🔹 Perishable Inventory & Category Turnover
- **Category-Wise Consumption Volume:** Quantified unit movement across 4 product categories (`Classic`, `Supreme`, `Veggie`, `Chicken`) to align raw ingredient procurement with actual consumption velocity.
- **Peak-Item Stock Prioritization:** Identified top-moving SKUs by unit volume to prevent stock-outs of high-turnover ingredients during rush hours.

### 🔹 ABC SKU Classification & Cumulative Demand
- **SKU Revenue & Volume Contribution (%):** Calculated each SKU's percentage contribution to total throughput to separate **Class-A core items** from slow-moving **Class-C items**.
- **Category-Level Top Performers:** Used `RANK() OVER (PARTITION BY category)` to isolate the top 3 SKUs within each category for targeted inventory buffering.
- **Cumulative Throughput Trajectory:** Tracked cumulative demand over time using running-total window functions to monitor long-term growth trends.

---

## 📊 Key Operational & Demand Metrics

| Supply Chain KPI | Finding / Metric |
| :--- | :--- |
| **Total Orders Fulfilled** | **21,350** high-frequency orders |
| **Total Revenue / Throughput** | **$817,860** |
| **Baseline Daily Demand** | **138 units/day** average throughput |
| **Primary Peak Window (Lunch)** | **12:00 PM** (2,520 orders — highest hourly surge) |
| **Secondary Peak Window (Dinner)** | **6:00 PM** (2,399 orders) |
| **Off-Peak Window** | Sharp demand drop-off after **10:00 PM** |
| **Highest Unit-Value SKU** | *The Greek Pizza* (Classic Category) — **$35.95** |

---

## 🔍 Key Supply Chain & Inventory Insights

### 1. Intraday Demand Surges & Shift Bottlenecks
- **Bimodal Demand Curve:** Order volume spikes sharply at **12:00 PM (2,520 orders)** and **6:00 PM (2,399 orders)**, before dropping off significantly after **10:00 PM**.
- **Operational Impact:** Flat staffing and uniform prep schedules create fulfillment delays during lunch/dinner peaks and idle labor/spoilage late at night.

### 2. Category-Level Ingredient Velocity
| Category | Total Units Sold | Inventory & Procurement Priority |
| :--- | :--- | :--- |
| **Classic** | **14,888** | **Highest Unit Turnover:** Requires highest daily par levels and frequent replenishment |
| **Supreme** | **11,987** | **High Multi-Ingredient Prep:** Steady demand requiring balanced component stocking |
| **Veggie** | **11,649** | **Perishable Produce Sensitivity:** Requires tight FEFO (First-Expired, First-Out) rotation |
| **Chicken** | **11,050** | **High Revenue-Per-Unit Driver:** Top 3 revenue SKUs all belong to the Chicken category |

### 3. Top-Performing SKUs (Class-A Inventory)
- **By Unit Velocity (Replenishment Focus):** *The Classic Deluxe* (2,453 units), *The Barbecue Chicken* (2,432 units), *The Hawaiian* (2,422 units), *The Pepperoni* (2,418 units), and *The Thai Chicken* (2,371 units).
- **By Revenue Contribution:** *Thai Chicken* (**5.31%**), *Barbecue Chicken* (**5.23%**), and *California Chicken* (**5.06%**) lead revenue generation—making chicken protein stock-outs the highest financial risk.

---

## 📸 Query Execution & Output Previews

### Total Orders Fulfilled
![Total Orders](screenshots/total_orders.png)

### Total Throughput Value
![Total Revenue](screenshots/total_revenue.png)

### Intraday Hourly Demand Distribution
![Hourly Distribution](screenshots/hourly_distribution.png)

### Cumulative Demand Trajectory Over Time
![Cumulative Revenue](screenshots/cumulative_revenue.png)

---

## 💡 Actionable Supply Chain Recommendations

1. **Dynamic Shift & Prep Scheduling:** Align workforce shifts and pre-batch ingredient prep **60–90 minutes ahead of the 12:00 PM and 6:00 PM surges**, while scaling down active prep after **9:30 PM** to minimize end-of-day perishable waste.
2. **Protein Safety Stock Protection:** Maintain strict safety stock buffers for **Chicken category ingredients**, as the top 3 revenue-generating SKUs (*Thai, Barbecue, and California Chicken*) rely on this single protein stream.
3. **FEFO Perishable Rotation:** Enforce **First-Expired, First-Out (FEFO)** inventory controls on *Veggie* and *Classic* toppings (which drive 14,888 units) to reduce spoilage while maintaining 100% fill rates.
4. **SKU Rationalization & Combo Bundling:** Bundle slow-moving Class-C menu variants with high-velocity *Classic Deluxe* or *Pepperoni* orders to accelerate ingredient turnover without expanding cold-storage requirements.

---

## 📂 Repository Structure

- `analysis_queries.sql` — Complete SQL script containing hourly demand distribution, category turnover, window functions, and ranking queries.
- `screenshots/` — Query output visualizations and relational schema diagram.
