# Restaurant Operations Analysis Case Study

## Solution

### Objectives 1: Explore the menu_items to get an idea of what's on the new menu 

1. Find number of items

````sql
SELECT COUNT(*) AS number_items FROM menu_items;
````
**Answer:**

<img width="141" alt="image" src="https://user-images.githubusercontent.com/81607668/128624690-edfb1c8e-2a46-4bd9-a699-d2e678d42664.png">

There are 554 unique users in the dataset.

***

2. Least and most expensive items

````sql
SELECT * FROM menu_items
ORDER BY price;
````
**Answer:**

<img width="141" alt="image" src="https://user-images.githubusercontent.com/81607668/128624690-edfb1c8e-2a46-4bd9-a699-d2e678d42664.png">

There are 554 unique users in the dataset.

***

### Objectives 2: Explore the order_details to get an idea of the data collected

1. What is the date range of the table? How many orders were made within this date range? How many items were ordered within this date range?

````sql
SELECT MIN(order_date) AS first_day, MAX(order_date) AS last_day, DATEDIFF(MAX(order_date), MIN(order_date)) AS day_difference, COUNT(DISTINCT(order_id)) AS orders_made, COUNT(item_id) AS items_count 
FROM order_details;
````
**Answer:**

<img width="141" alt="image" src="https://user-images.githubusercontent.com/81607668/128624690-edfb1c8e-2a46-4bd9-a699-d2e678d42664.png">

There are 554 unique users in the dataset.

***

### Objectives 3: Use both tables to understand how customers are reacting to the new menu

1. Combine 2 tables into 1 single table. What were the least and most ordered items? what categories were they in?

````sql
WITH details AS (
SELECT od.order_id, mi.item_name, mi.category, mi.price, od.order_date, od.order_time FROM order_details od
LEFT JOIN menu_items mi
ON od.item_id = mi.menu_item_id
)
SELECT COUNT(item_name) AS amount, item_name, category FROM details
GROUP BY item_name
ORDER BY amount
````
**Answer:**

<img width="141" alt="image" src="https://user-images.githubusercontent.com/81607668/128624690-edfb1c8e-2a46-4bd9-a699-d2e678d42664.png">

There are 554 unique users in the dataset.

***

2. What were top 5 orders spent the most money?

````sql
WITH details AS (
SELECT od.order_id, mi.item_name, mi.category, mi.price, od.order_date, od.order_time FROM order_details od
LEFT JOIN menu_items mi
ON od.item_id = mi.menu_item_id
)
SELECT order_id, SUM(price) AS total from details
GROUP BY order_id
ORDER BY total DESC
LIMIT 5;
````
**Answer:**

<img width="141" alt="image" src="https://user-images.githubusercontent.com/81607668/128624690-edfb1c8e-2a46-4bd9-a699-d2e678d42664.png">

There are 554 unique users in the dataset.

***

3. View details of the highest spend order. what insights can you gather from results?

````sql
WITH details AS (
SELECT od.order_id, mi.item_name, mi.category, mi.price, od.order_date, od.order_time FROM order_details od
LEFT JOIN menu_items mi
ON od.item_id = mi.menu_item_id
)
SELECT order_id, item_name, category, COUNT(item_name) AS amount, SUM(price) AS total_price from details
WHERE order_id = 440
GROUP BY category
ORDER BY category;
````
**Answer:**

<img width="141" alt="image" src="https://user-images.githubusercontent.com/81607668/128624690-edfb1c8e-2a46-4bd9-a699-d2e678d42664.png">

There are 554 unique users in the dataset.

***

4. How many orders and sales per month (cohort)

````sql
WITH cohorts AS (
    SELECT 
        COUNT(od.order_id) as total_orders,
        DATE_FORMAT(od.order_date, '%Y-%m') AS cohort_month,
        SUM(mi.price) as total_price
    FROM order_details od
    LEFT JOIN menu_items mi
    ON od.item_id = mi.menu_item_id
    GROUP BY cohort_month
)
SELECT * FROM cohorts;
````
**Answer:**

<img width="141" alt="image" src="https://user-images.githubusercontent.com/81607668/128624690-edfb1c8e-2a46-4bd9-a699-d2e678d42664.png">

There are 554 unique users in the dataset.

***

5. How many customers per month?

````sql
WITH cohorts AS (
    SELECT 
        COUNT(order_id) as total_orders,
        DATE_FORMAT(order_date, '%Y-%m') AS cohort_month
    FROM order_details
    GROUP BY order_id
)
SELECT COUNT(total_orders) as total_customers, cohort_month FROM cohorts
GROUP BY cohort_month;
````
**Answer:**

<img width="141" alt="image" src="https://user-images.githubusercontent.com/81607668/128624690-edfb1c8e-2a46-4bd9-a699-d2e678d42664.png">

There are 554 unique users in the dataset.

***

6. How many orders are cancelled?

````sql
WITH details AS (
SELECT od.order_id, mi.item_name, mi.category, mi.price, od.order_date, od.order_time FROM order_details od
LEFT JOIN menu_items mi
ON od.item_id = mi.menu_item_id
)
SELECT COUNT(order_id) AS cancel_orders FROM details
WHERE item_name IS NULL;
````
**Answer:**

<img width="141" alt="image" src="https://user-images.githubusercontent.com/81607668/128624690-edfb1c8e-2a46-4bd9-a699-d2e678d42664.png">

There are 554 unique users in the dataset.

***
