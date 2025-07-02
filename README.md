# Pizza-Sales

## 📌Project Overview :
This project aims to analyze pizza sales data using SQL for data exploration and KPI measurement, followed by the creation of an interactive dashboard in Excel using pivot tables. The objective is to extract actionable business insights that can help stakeholders understand performance trends and optimize sales strategies.

## 🎯Objectives
- Perform KPI analysis to evaluate overall business performance.
- Identify trends in customer purchasing behavior (daily, hourly).
- Analyze performance across pizza categories and sizes.
- Visualize findings through an Excel dashboard to aid in decision-making.

## 🧰 Tools & Technologies
1. SQL Server Management Studio : Data querying, analysis, and KPI calculation
2. Microsoft Excel: Dashboard creation using Pivot Tables & Charts


## ❓Data Analysis & Findings
The following SQL Queries were developed to answer spesific business questions:
### KPI's

1. **Total Revenue**:
```sql
SELECT SUM(total_price) AS Total_Revenue
FROM pizza_sales;
```
![image](https://github.com/user-attachments/assets/1ecd780d-0571-40ec-8e38-b696023bcb68)

2. **Average Order Value**:
```sql
SELECT (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_order_Value
FROM pizza_sales
```
![image](https://github.com/user-attachments/assets/d05f7bf4-c5c5-4c3b-8ae0-06db4cbb7a23)

3. **Total Pizza Sold**:
```sql
SELECT SUM(quantity) AS Total_pizza_sold
FROM pizza_sales
```
![image](https://github.com/user-attachments/assets/7a89ba73-3575-4e7c-945d-6dd870e69643)

4. **Total Orders**:
```sql
SELECT COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
```
![image](https://github.com/user-attachments/assets/8b992e5b-ecea-47f0-8dd2-6cf274349e40)

5. **Average Pizza Per Order**:
```sql
SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) / 
CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2))
AS Avg_Pizzas_per_order
FROM pizza_sales
```
![image](https://github.com/user-attachments/assets/a878f586-8fcc-425d-b2bf-00063443b896)

### B. Daily Trend for Total Orders
```sql
SELECT DATENAME(DW, order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders 
FROM pizza_sales
GROUP BY DATENAME(DW, order_date)
ORDER BY 2
```
![image](https://github.com/user-attachments/assets/4a029552-0252-4dd4-a77a-0aa461e685de)

### C. Hourly Trend for Orders
```sql
SELECT DATEPART(HOUR, order_time) as order_hours, COUNT(DISTINCT order_id) as total_orders
from pizza_sales
group by DATEPART(HOUR, order_time)
order by DATEPART(HOUR, order_time)
```
![image](https://github.com/user-attachments/assets/66e695cb-993d-414d-9228-6d0ceeac54e8)

### D. % of Sales by Pizza Category
```sql
SELECT pizza_category, CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_category
```
![image](https://github.com/user-attachments/assets/61ba0679-5a5f-4f2d-95b4-00b46eb34500)

### E. % of Sales by Pizza Size
```sql
SELECT pizza_size, CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_size
ORDER BY pizza_size
```
![image](https://github.com/user-attachments/assets/65366345-a5a7-4a46-917a-c59d65c52e0e)

### F. Total Pizzas Sold by Pizza Category
```sql
SELECT pizza_category, SUM(quantity) as Total_Quantity_Sold
FROM pizza_sales
WHERE MONTH(order_date) = 2
GROUP BY pizza_category
ORDER BY Total_Quantity_Sold DESC
```
![image](https://github.com/user-attachments/assets/f064ec49-7bab-4590-801f-59b19fbc316b)

### G. TOp 5 Best Sellers by Total Pizzas Sold
```sql
SELECT Top 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold DESC
```
![image](https://github.com/user-attachments/assets/a1ed71a7-f484-459d-ab36-820670a56bae)

### H. Bottom 5 Best Sellers by Total Pizzas Sold
```sql
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold ASC
```
![image](https://github.com/user-attachments/assets/cf72e224-a7b0-4b98-8f56-841764af7879)

## 📈 Dashboard Features (Excel)
The dashboard was developed using Pivot Tables and includes:
- KPI summary cards
- Daily & hourly trend charts
- Pie charts for sales % by category & size
- Bar charts for best/worst selling pizzas
- Slicers for interactivity (e.g., filter by category or size)

![image](https://github.com/user-attachments/assets/2b00b6a0-201f-483a-a8a7-bf4db20541ab)

## 🚀 Insights & Business Impact
✅Peak orders occur during lunch (12 PM–2 PM) and dinner hours (5 PM–8 PM).
→ Suggests focusing staff and inventory during these hours.
✅Orders are highest on weekends, especially Friday and Saturday.
→ Weekend promotions and staffing plans are recommended.
✅The Classic category contributes the highest percentage of sales.
→ Ensure reliable stock and consider marketing this popular category.
✅Large size pizzas generate the most revenue.
→ Opportunity to upsell to larger sizes or bundle deals.
✅Top 5 best-selling pizzas identified.
→ Focus promotions on high performers to maximize revenue.
✅Bottom 5 worst-selling pizzas highlighted.
→ Evaluate for improvement, promotion, or potential removal.
