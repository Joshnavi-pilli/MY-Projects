# 🍕 Pizza Sales Dashboard – Power BI Project

## 📊 Project Overview

This Power BI dashboard analyzes pizza sales data to uncover key business insights and performance metrics. It was developed using a combination of **SQL queries** for data extraction and **DAX functions** for dynamic calculations within Power BI. The report focuses on tracking core KPIs, identifying trends, and understanding customer preferences across categories and time.

---

## 🎯 Problem Statement

The objective of this dashboard is to analyze key performance indicators (KPIs) and visualize sales trends for a pizza business. The insights are intended to support data-driven decision-making and identify areas of strength and opportunity.

---

## 📈 KPI Requirements

The following KPIs are calculated:

1. **Total Revenue** – Sum of the total price of all pizza orders.
2. **Average Order Value** – Total revenue divided by the number of orders.
3. **Total Pizzas Sold** – Total quantity of all pizzas sold.
4. **Total Orders** – Count of unique order IDs.
5. **Average Pizzas Per Order** – Total pizzas sold divided by total orders.

These metrics are derived using SQL queries before being visualized in Power BI.

---

## 📊 Charts & Visualizations

The dashboard includes the following visual elements:

- **Daily Trend for Total Orders**: Bar chart showing order volume by weekday.
- **Monthly Trend for Total Orders**: Line chart highlighting order trends across different months.
- **Sales by Pizza Category**: Pie chart visualizing category-wise revenue contribution.
- **Sales by Pizza Size**: Pie chart showing revenue distribution across pizza sizes.
- **Total Pizzas Sold by Category**: Funnel chart to compare category sales volume.
- **Top 5 Best Sellers** (by Revenue, Quantity, Orders): Bar charts for identifying most popular pizzas.
- **Bottom 5 Sellers** (by Revenue, Quantity, Orders): Bar charts to highlight underperformers.

---

## 🛠️ Tools & Technologies Used

- **Power BI**: Dashboard development and data modeling
- **SQL**: Data preparation, aggregation, and cleaning
- **DAX**: Measures and dynamic calculations
- **Excel**: Source data format

---

## 📌 Key Insights

- Identified peak sales periods by day and month
- Revealed best and worst-performing pizzas
- Highlighted customer preferences by pizza category and size
- Delivered actionable insights on order trends and product performance

---

## Snapshots of Dashboard (Power BI Service)

![Image](https://github.com/user-attachments/assets/0bf6d333-a17b-40ce-925f-3759536ab2a4)

![Image](https://github.com/user-attachments/assets/1fe10783-6525-4d2f-b521-cbf95fe18269)

---

## 📊 SQL Queries Used

```sql
-- Total Revenue
SELECT SUM(total_price) AS Total_Revenue FROM pizza_sales;

-- Average Order Value
SELECT (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_order_Value FROM pizza_sales;

-- Total Pizzas Sold
SELECT SUM(quantity) AS Total_pizza_sold FROM pizza_sales;

-- Total Orders
SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales;

-- Average Pizzas Per Order
SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) / 
CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2)) AS Avg_Pizzas_per_order FROM pizza_sales;

-- Daily Trend for Total Orders
SELECT DATENAME(DW, order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders 
FROM pizza_sales GROUP BY DATENAME(DW, order_date);

-- Monthly Trend for Orders
SELECT DATENAME(MONTH, order_date) AS Month_Name, COUNT(DISTINCT order_id) AS Total_Orders 
FROM pizza_sales GROUP BY DATENAME(MONTH, order_date);

-- % of Sales by Pizza Category
SELECT pizza_category, CAST(SUM(total_price) AS DECIMAL(10,2)) AS total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) FROM pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales GROUP BY pizza_category;

-- % of Sales by Pizza Size
SELECT pizza_size, CAST(SUM(total_price) AS DECIMAL(10,2)) AS total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) FROM pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales GROUP BY pizza_size ORDER BY pizza_size;

-- Top/Bottom 5 Sellers (Revenue, Quantity, Orders)
SELECT TOP 5 pizza_name, SUM(total_price) AS Total_Revenue FROM pizza_sales GROUP BY pizza_name ORDER BY Total_Revenue DESC;
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold FROM pizza_sales GROUP BY pizza_name ORDER BY Total_Pizza_Sold DESC;
SELECT TOP 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales GROUP BY pizza_name ORDER BY Total_Orders DESC;

---

🔢 DAX Measures Used

-- Total Revenue
Total Revenue = SUM(pizza_sales[total_price])

-- Average Order Value
Average Order Value = DIVIDE([Total Revenue], [Total Orders])

-- Total Pizzas Sold
Total Pizzas Sold = SUM(pizza_sales[quantity])

-- Total Orders
Total Orders = DISTINCTCOUNT(pizza_sales[order_id])

-- Average Pizzas Per Order
Average Pizzas Per Order = DIVIDE([Total Pizzas Sold], [Total Orders])

