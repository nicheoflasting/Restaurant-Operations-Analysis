# Restaurant Operations Analysis Case Study

## Solution

### Objectives 1: Explore the menu_items to get an idea of what's on the new menu 

1. Find number of items

````sql
SELECT COUNT(*) AS number_items FROM menu_items;
````
**Answer:**

<img width="170" height="56" alt="image" src="https://github.com/user-attachments/assets/81f9ec36-fb50-47bf-8b38-53d978c88d46" />

There are 554 unique users in the dataset.

***

2. Least and most expensive items

````sql
SELECT * FROM menu_items
ORDER BY price;
````
**Answer:**

<img width="651" height="57" alt="image" src="https://github.com/user-attachments/assets/d993abc3-117d-4ec1-942d-635e8a97fad1" />

<img width="650" height="25" alt="image" src="https://github.com/user-attachments/assets/114ef61c-0d8b-4a83-a6f2-75adf157ad6c" />

There are 554 unique users in the dataset.

***

### Objectives 2: Explore the order_details to get an idea of the data collected

1. What is the date range of the table? How many orders were made within this date range? How many items were ordered within this date range?

````sql
SELECT MIN(order_date) AS first_day, MAX(order_date) AS last_day, DATEDIFF(MAX(order_date), MIN(order_date)) AS day_difference, COUNT(DISTINCT(order_id)) AS orders_made, COUNT(item_id) AS items_count 
FROM order_details;
````
**Answer:**

<img width="645" height="72" alt="image" src="https://github.com/user-attachments/assets/0591de04-b457-4e8f-a377-8e96d5d7a60c" />

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

<img width="389" height="63" alt="image" src="https://github.com/user-attachments/assets/23dd7d8d-af05-4d92-b70f-45dfe373e1ef" />

<img width="382" height="23" alt="image" src="https://github.com/user-attachments/assets/41fcd5ce-fcbc-4699-b568-a9bc0510a91d" />

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

<img width="206" height="160" alt="image" src="https://github.com/user-attachments/assets/9ee15cef-397c-4fc9-b4f1-b52145acf89d" />

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

<img width="567" height="144" alt="image" src="https://github.com/user-attachments/assets/372da39c-1fda-4d0c-a7e8-1b77a4236796" />

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

<img width="389" height="116" alt="image" src="https://github.com/user-attachments/assets/32a4266b-3bf4-4c6f-b0ba-1a9faefc70e5" />

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

<img width="317" height="113" alt="image" src="https://github.com/user-attachments/assets/59b11c5b-7a1e-4468-8d00-99f4e14d74c7" />

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

<img width="173" height="63" alt="image" src="https://github.com/user-attachments/assets/070f1ec2-418e-41c4-9027-aab2d3c356c1" />

There are 554 unique users in the dataset.

***
