# 🍕 Pizza Sales Performance Dashboard

## 📌 Objective
Analyze pizza sales trends and product performance to support marketing and business decisions.

## 🧰 Tools Used
SQL (MySQL), Power BI, Excel

## 📊 Overview
An interactive Power BI dashboard built using raw sales data and SQL queries to extract insights. Includes:
- Revenue & order KPIs
- Sales breakdown by time, size, and category
- Best & worst selling pizzas
- Dynamic slicers for filtering

## 💡 Key Insights
- Friday & Saturday are the busiest days
- July and January have the highest order volume
- Classic category and Large size lead in sales
- Brie Carre consistently underperforms

## 🧾 SQL Queries Used

The data was extracted and pre-aggregated using SQL before being imported into Power BI. Below are the key queries used to generate insights and drive visualizations:

### 🔹 A. KPIs


```sql
-- 1. Total Revenue
SELECT sum(total_price) AS Total_revenue
FROM pizza_sales;

-- 2. Average Order Value
SELECT sum(total_price) / count(distinct order_id) AS Avg_order_value
FROM pizza_sales;

-- 3. Total Pizzas Sold
SELECT sum(quantity) AS Total_pizza_sold
FROM pizza_sales;

-- 4. Total Orders Placed
SELECT count(DISTINCT order_id) AS Total_orders
FROM pizza_sales;

-- 5. Average Pizzas Per Order
SELECT sum(quantity) / count(DISTINCT order_id) AS Avg_pizzas_per_order
FROM pizza_sales;


### 🔹 B. Chart Queries

```sql
-- 1. Daily Trend for Total Orders
SELECT DAYNAME(order_date_clean) AS order_day,
       COUNT(DISTINCT order_id) AS total_orders
FROM pizza_sales
GROUP BY order_day
ORDER BY FIELD(order_day, 'Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday');

-- 2. Monthly Trend for Total Orders
SELECT MONTHNAME(order_date_clean) AS month_name,
       COUNT(DISTINCT order_id) AS total_orders
FROM pizza_sales
GROUP BY month_name
ORDER BY total_orders DESC;

-- 3. % of Sales Per Pizza Category (January Only)
SELECT pizza_category, 
       SUM(total_price) AS total_sales,
       SUM(total_price) * 100 / 
       (SELECT SUM(total_price) FROM pizza_sales WHERE MONTH(order_date_clean) = 1) AS PCT
FROM pizza_sales
WHERE MONTH(order_date_clean) = 1
GROUP BY pizza_category;

-- 4. % of Sales by Pizza Size (Q1 Only)
SELECT pizza_size, 
       CAST(SUM(total_price) AS DECIMAL(10,2)) AS total_sales,
       CAST(SUM(total_price) * 100 / 
       (SELECT SUM(total_price) FROM pizza_sales WHERE QUARTER(order_date_clean) = 1) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
WHERE QUARTER(order_date_clean) = 1
GROUP BY pizza_size
ORDER BY PCT DESC;


### 🔹 C. Product Performance

```sql
-- 5. Top 5 Best Sellers by Revenue
SELECT pizza_name, SUM(total_price) AS Total_revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_revenue DESC
LIMIT 5;

-- 6. Bottom 5 Sellers by Revenue
SELECT pizza_name, SUM(total_price) AS Total_revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_revenue ASC
LIMIT 5;

-- 7. Top 5 Sellers by Quantity
SELECT pizza_name, SUM(quantity) AS Total_quantity
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_quantity DESC
LIMIT 5;

-- 8. Bottom 5 Sellers by Quantity
SELECT pizza_name, SUM(quantity) AS Total_quantity
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_quantity ASC
LIMIT 5;

-- 9. Top 5 Sellers by Orders
SELECT pizza_name, COUNT(DISTINCT order_id) AS Total_orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_orders DESC
LIMIT 5;

-- 10. Bottom 5 Sellers by Orders
SELECT pizza_name, COUNT(DISTINCT order_id) AS Total_orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_orders ASC
LIMIT 5;






