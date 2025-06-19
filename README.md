# Pizza_Sales_Data_Analysis
Exploratory data analysis of pizza sales using SQL — includes KPIs, trends, and category-level insights.
#  Pizza Sales Data Analysis (SQL Project)

This project explores sales data from a fictional pizza store to derive meaningful business insights using SQL queries.

##  Dataset

- `pizza_sales excel file.xlsx`: Contains raw data including order ID, order date & time, pizza name, category, size, quantity, and total price.

##  Tools Used

- Microsoft SQL Server
- Excel
- (Optional) Power BI for dashboarding

##  Key Business Questions Answered

- What is the **total revenue** generated?
- What is the **average order value**?
- How many **total pizzas** were sold?
- How many **total orders** were placed?
- What is the **average number of pizzas per order**?
- What are the **daily and hourly sales trends**?
- What is the **sales distribution by pizza category and size**?
- Which are the **top 5 and bottom 5 selling pizzas**?

##  Sample SQL Queries

```sql
-- 1. Total Revenue
SELECT SUM(total_price) AS Total_Revenue FROM pizza_sales;

-- 2. Average Order Value
SELECT (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_order_Value FROM pizza_sales;

-- 3. Total Pizzas Sold
SELECT SUM(quantity) AS Total_pizza_sold FROM pizza_sales;

-- 4. Total Orders
SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales;

-- 5. Average Pizzas Per Order
SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) / 
CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2)) 
AS Avg_Pizzas_per_order
FROM pizza_sales;

-- 6. Daily Trend for Orders
SELECT DATENAME(DW, order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders 
FROM pizza_sales
GROUP BY DATENAME(DW, order_date);

-- 7. Hourly Trend for Orders
SELECT DATEPART(HOUR, order_time) as order_hours, COUNT(DISTINCT order_id) as total_orders
FROM pizza_sales
GROUP BY DATEPART(HOUR, order_time)
ORDER BY DATEPART(HOUR, order_time);

-- 8. % of Sales by Pizza Category
SELECT pizza_category, CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_category;

-- 9. Top 5 Best Sellers
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold DESC;

-- 10. Bottom 5 Best Sellers
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold ASC;
