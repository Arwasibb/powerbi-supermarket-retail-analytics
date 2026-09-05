# 🛒 Supermarket Retail Analytics & Dimensional Modeling | Power BI

![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Advanced_Measures-blue?style=for-the-badge)
![Data Modeling](https://img.shields.io/badge/Data_Modeling-Star_Schema-green?style=for-the-badge)

## 📌 Executive Summary
An end-to-end dimensional retail business intelligence solution designed to evaluate sales performance, branch operational efficiency, and product category margins. Built upon a robust **Star Schema** data model, this interactive dashboard empowers retail stakeholders to shift from reactive reporting to predictive, data-driven margin optimization.

---

## 🎯 Business Problem & Key Objectives
Retail decision-makers often struggle to dissect revenue drivers across distributed branch networks and diverse product lines. Key analytical questions addressed:
1. **Sales Performance:** What are the MoM (Month-over-Month) sales dynamics and seasonality surges?
2. **Operational Efficiency:** How do individual regional branches rank by total revenue vs. average transaction value?
3. **Product Profitability:** Which merchandise categories generate volume vs. high-margin contribution?
4. **Basket Size Dynamics:** What is the average basket spend per customer transaction across product categories?

---

## 🏗️ Data Model Architecture (Star Schema)
The underlying data model follows Kimball dimensional modeling principles, separating transactional events from descriptive business dimensions:

* **Fact Table:**
  * `Fact_Sales` (Transaction ID, DateKey, BranchKey, ProductKey, Quantity, UnitPrice, Discount, TotalRevenue, TotalCost, Margin)
* **Dimension Tables:**
  * `Dim_Branches` (Branch ID, City, Region, Branch Type)
  * `Dim_Products` (Product ID, Product Name, Category, Sub-category, Unit Cost)
  * `Dim_Date` (Full Date, Year, Quarter, Month, Weekday, IsWeekend)

> **Model Relationship:** 1-to-Many (`1:*`) relationships enforced from dimension tables to the central fact table with single-direction cross-filtering to guarantee optimal query performance.

---

## ⚡ Core DAX Measures & Business Logic
Here are sample DAX measures engineered for dynamic analysis:

```dax
// 1. Total Margin %
Margin % = 
DIVIDE(
    [Total Revenue] - [Total Cost],
    [Total Revenue],
    0
)

// 2. Month-over-Month (MoM) Revenue Growth
MoM Sales Growth % = 
VAR CurrentMonthSales = [Total Revenue]
VAR PriorMonthSales = CALCULATE([Total Revenue], DATEADD(Dim_Date[Date], -1, MONTH))
RETURN
DIVIDE(CurrentMonthSales - PriorMonthSales, PriorMonthSales, 0)

// 3. Average Basket Spend (Average Transaction Value)
Avg Basket Spend = 
DIVIDE(
    [Total Revenue],
    DISTINCTCOUNT(Fact_Sales[Transaction_ID]),
    0
)
