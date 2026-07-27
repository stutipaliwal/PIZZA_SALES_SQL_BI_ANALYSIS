# Enterprise Pizza Sales Analysis & Performance Dashboard

## Executive Summary
This project delivers an end-to-end business intelligence solution evaluating transactional sales performance, customer purchasing behavior, and product portfolio profitability. Using raw transactional data, key business metrics were aggregated via **SQL Server (T-SQL)** and compiled into an interactive executive dashboard using **Microsoft Power BI**.

---

## Key Performance Indicators (KPIs)

| Metric | Business Definition | SQL Logic |
| :--- | :--- | :--- |
| **Total Revenue** | Cumulative monetary value of all orders | `SUM(total_price)` |
| **Average Order Value (AOV)** | Mean spend per customer transaction | `SUM(total_price) / COUNT(DISTINCT order_id)` |
| **Total Units Sold** | Total volume of individual pizzas fulfilled | `SUM(quantity)` |
| **Order Volume** | Total number of unique orders processed | `COUNT(DISTINCT order_id)` |
| **Average Units Per Order** | Mean volume of pizzas per individual order | `SUM(quantity) / COUNT(DISTINCT order_id)` |

---

## Core Analytical Focus Areas

### 1. Temporal & Demand Analysis
* **Daily Order Distribution:** Evaluated transaction density by day of week (`DATENAME(DW)`) to identify peak operational demand.
* **Monthly Sales Trajectory:** Tracked order volume fluctuations across months (`DATENAME(MONTH)`) to evaluate seasonality.

### 2. Product Portfolio & Category Performance
* **Category Share:** Revenue and volume contribution breakdown across product lines (Classic, Veggie, Supreme, Chicken).
* **Size Mix:** Sales proportion across sizing tiers (S, M, L, XL, XXL) analyzed by quarterly periods.

### 3. Product Ranking (Top/Bottom Performers)
* Performance evaluation of product SKUs ranked by **Total Revenue**, **Unit Volume**, and **Total Order Count**.

---

## Technical Implementation & SQL Scripts

### Core Performance Metrics
```sql
-- Total Revenue
SELECT 
    SUM(total_price) AS Total_Revenue 
FROM pizza_sales;

-- Average Order Value (AOV)
SELECT 
    SUM(total_price) / COUNT(DISTINCT order_id) AS Average_Order_Value 
FROM pizza_sales;

-- Total Pizzas Sold
SELECT 
    SUM(quantity) AS Total_Pizza_Sold 
FROM pizza_sales;

-- Total Distinct Orders
SELECT 
    COUNT(DISTINCT order_id) AS Total_Orders 
FROM pizza_sales;

-- Average Pizzas Per Order
SELECT 
    CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) / CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2)) AS Average_Pizzas_Per_Order 
FROM pizza_sales;

-- Daily Order Density
SELECT 
    DATENAME(DW, order_date) AS Order_Day, 
    COUNT(DISTINCT order_id) AS Total_Orders 
FROM pizza_sales 
GROUP BY DATENAME(DW, order_date);

-- Monthly Order Volume (Sorted by Density)
SELECT 
    DATENAME(MONTH, order_date) AS Order_Month, 
    COUNT(DISTINCT order_id) AS Total_Orders 
FROM pizza_sales 
GROUP BY DATENAME(MONTH, order_date) 
ORDER BY Total_Orders DESC;

-- Top 5 SKUs by Revenue
SELECT TOP 5 
    pizza_name, 
    CAST(SUM(total_price) AS DECIMAL(10,2)) AS Total_Revenue 
FROM pizza_sales 
GROUP BY pizza_name 
ORDER BY Total_Revenue DESC;

-- Top 5 SKUs by Volume
SELECT TOP 5 
    pizza_name, 
    SUM(quantity) AS Total_Quantity 
FROM pizza_sales 
GROUP BY pizza_name 
ORDER BY Total_Quantity DESC;

├── pizza_sales.csv               # Raw transactional dataset (Source)
├── pizza_sales_queries.sql       # SQL Data Extraction & Transformation Script
├── pizza_sales_dashboard.pbix    # Power BI Dashboard File
└── README.md                     # Project Technical Documentation
