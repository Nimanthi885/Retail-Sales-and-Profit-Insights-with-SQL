-- Preview data from order_details table
SELECT * FROM order_details LIMIT 10;
-- Preview data from order_list table
SELECT * FROM order_list LIMIT 10;
-- Preview data from sales_target table
SELECT * FROM sales_target LIMIT 10;

-- 1. Total Sales and Profit Analysis
-- Total Sales by Category, State, and Month
SELECT ol.State, od.Category,
   MONTH(STR_TO_DATE( ol.Order_Date,'%d-%m-%Y')) AS Month, 
   SUM(od.Amount) AS Total_Sales, 
   SUM(od.Profit) AS Total_Profit
FROM order_list AS ol
JOIN order_details AS od ON od.Order_ID=ol.Order_ID
GROUP BY ol.State, od.Category,Month;

-- Identify Top-Performing Categories and Regions
SELECT ol.State, od.Category, SUM(od.Profit) AS `Total Profit`
FROM order_list AS ol
JOIN order_details AS od ON od.Order_ID=ol.Order_ID
GROUP BY ol.State, od.Category
ORDER BY `Total Profit` DESC
LIMIT 5; -- Top 5 regions and categories

-- 2. Sales Target Achievement
-- Compare Actual Sales with Sales Targets:
SELECT 
   st.Month_of_Order_Date, 
   st.Category, 
   SUM(od.Amount) AS Total_Sales, 
   st.Target, 
   (SUM(od.Amount) - st.Target) AS Difference
FROM sales_target st
LEFT JOIN order_details od 
   ON st.Category = od.Category 
GROUP BY st.Month_of_Order_Date, st.Category, st.Target
HAVING SUM(od.Amount) IS NOT NULL;

-- Identify Categories that Met or Missed Targets:
SELECT 
   st.Category, 
   st.Month_of_Order_Date, 
   SUM(od.Amount) AS Total_Sales, 
   st.Target, 
   CASE 
      WHEN SUM(od.Amount) >= st.Target THEN 'Met'
      ELSE 'Missed'
   END AS Target_Status
FROM sales_target st
LEFT JOIN order_details od 
   ON st.Category = od.Category 
   GROUP BY st.Category, st.Month_of_Order_Date, st.Target;

-- 4. Customer Analysis
-- Most Profitable Customers:
-- Identify customers contributing the most profit.
SELECT ol.Customer_Name, SUM(od.Profit) AS `Total Profit`
FROM order_list AS ol
JOIN order_details AS od ON od.Order_ID=ol.Order_ID
GROUP BY ol.Customer_Name
ORDER BY `Total Profit` DESC
LIMIT 10;

-- Customer Behavior by Region:
-- Aggregate customer behavior by region to see trends and insights.
SELECT 
   ol.City, 
   ol.State, 
   COUNT(DISTINCT ol.Customer_Name) AS Total_Customers, 
   SUM(od.Amount) AS Total_Sales, 
   SUM(od.Profit) AS Total_Profit
FROM order_list ol
JOIN order_details od ON ol.Order_ID = od.Order_ID
GROUP BY ol.City, ol.State;
