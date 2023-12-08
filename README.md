# Pizza Sales Analysis 

## Project Overview 

In this SQL portfolio project, I explore pizza sales from January to December 2015. The project will offer insights into pizza sales performance as well as make data-driven recommendations.

## Data Sources
The dataset used for this analysis is the 'pizza_sales.csv' file. It consists of pizza sales information by size, category, order date, seller name, price, order time and more. It is included in my repository.

## Tools
- Excel
    - Data Cleaning 
- MySQL
    - Data Analysis
## Exploratory Data Analysis
This involved answering the following questions:

- What is the total revenue of pizzas?
- How many total orders have been placed?
- What is the average amount spent per order?
- How many pizzas, in total, have been sold?
- What is the average number of pizzas per order?
- What is the daily trend for total orders?
- What is the hourly trend for total orders?
- What is the monthly trend for total orders?
- What percentage of sales is accounted for by each pizza category?
- What percentage of sales is attributed to each pizza size?
- How many pizzas have been sold for each pizza category?
- What are the top 5 best sellers by revenue?
- What are the bottom 5 best sellers by revenue?

## Data Analysis

My analysis included:

### Creating A Database 
```sql
CREATE DATABASE sales;
```
### Create Table
```sql
CREATE TABLE sales.pizza_sales (
    pizza_id INT NOT NULL,
    Order_id INT NULL,
    pizza_name_id VARCHAR(50),
    quantity INT NULL,
    order_date DATE NULL,
    order_time TIME NULL,
    unit_price FLOAT NULL,
    total_price FLOAT NULL,
    pizza_size VARCHAR(30) NULL,
    pizza_category VARCHAR(50) NULL,
    pizza_ingredients VARCHAR(250) NULL,
    pizza_name VARCHAR(100) NULL,
    PRIMARY KEY (pizza_id)
);
```

### Import Data

To see the blank table:
```sql
SELECT * FROM sales.pizza_sales;
```
On the result grid, I use the 'import data from an external file'  to upload the CSV file 

### Verify Imported data

```sql
use sales;
```

```sql
SELECT * FROM pizza_sales 
limit 10;
```
### Answering Questions
#### What is the total revenue of pizzas?
```sql
SELECT 
    SUM(total_price) AS Total_Revenue
FROM
    pizza_sales;
```
- How many total orders have been placed?
```sql

SELECT 
    COUNT(DISTINCT order_id)
FROM
    pizza_sales;
```
- What is the average amount spent per order?

```sql
SELECT 
    SUM(total_price) / COUNT(DISTINCT order_id) AS Average_Amount_Per_Order
FROM
    pizza_sales;
```
- How many pizzas, in total, have been sold?
```sql
SELECT 
    SUM(quantity)
FROM
    pizza_sales;
```
- What is the average number of pizzas per order?
```sql
SELECT 
    CAST(SUM(quantity) / COUNT(DISTINCT order_id) AS DECIMAL (10 , 2 )) AS Avg_Pizzas_per_order
FROM
    pizza_sales;
```
- What is the daily trend for total orders?
```sql
  SELECT 
    COUNT(DISTINCT order_id) AS Total_order,
    DAYNAME(order_date) AS day_of_week
  FROM
    pizza_sales
  GROUP BY DAYNAME(order_date);
```
- What is the hourly trend for total orders?

```sql
SELECT 
    COUNT(DISTINCT order_id) AS Total_order,
    HOUR(order_time) AS hour_of_the_day
FROM
    pizza_sales
GROUP BY HOUR(order_time);
```
- What is the monthly trend for total orders?

```sql
SELECT 
    COUNT(DISTINCT order_id) AS Total_order,
    MONTHNAME(order_date) AS MONTH
FROM
    pizza_sales
GROUP BY MONTHNAME(order_date);
```
- What percentage of sales is accounted for by each pizza category?
```sql
  SELECT
  pizza_category,
  Cast(sum(total_price)*100 /(select sum(total_price) from pizza_sales) As decimal(10,2)) As Percentage
  FROM pizza_sales
  group by pizza_category; 
```
What percentage of sales is attributed to each pizza size?

```sql 
SELECT 
    pizza_size,
    CAST(SUM(total_price) * 100 / (SELECT 
                SUM(total_price)
            FROM
                pizza_sales)
        AS DECIMAL (10 , 2 )) AS Percentage
FROM
    pizza_sales
GROUP BY pizza_size;

```
- How many pizzas have been sold for each pizza category?

```sql
SELECT 
    pizza_category, SUM(quantity) As Total_pizza_sold
FROM
    sales.pizza_sales
GROUP BY pizza_category;
```
- What are the top 5 best sellers by revenue?
```sql
SELECT 
    pizza_name, SUM(total_price) As Total_Revenue
FROM
    pizza_sales
GROUP BY pizza_name
order by total_revenue DESC
Limit 5;
```
- What are the bottom 5 best sellers by revenue?

```sql
SELECT 
    pizza_name, SUM(total_price) As Total_Revenue
FROM
    pizza_sales
GROUP BY pizza_name
order by total_revenue ASC
Limit 5;
```
## Results 
1. An average of two pizzas are sold for every order placed
2. Most Pizzas are sold on Friday and saturday and are ordered at 12:00 a.m.
3. July had the highest number of pizza orders
4. Classic Pizza had the highest sales compared to Veggie, Supreme, and chicken
5. 45% of all pizza sales were of the large size, followed by medium at 30%
6. The Thai Chicken Pizza had the highest sales in 2015, while Brie Carre Pizza had the lowest sales

### Recommendations 
Based on the analysis, we recommend the company to:
- Capitalize on the high pizza sales on Fridays and Saturdays. Consider running weekend specials or promotions during these days to attract more customers.
- Given that most pizzas are ordered at 12:00 a.m., consider launching lunchtime promotions or discounts to further boost sales during this popular ordering time.
- Since Classic Pizza had the highest sales, consider featuring Classic Pizza prominently in marketing materials and promotions. Additionally, consider introducing new variations or promotions for Veggie, Supreme, and Chicken pizzas to increase their sales.









