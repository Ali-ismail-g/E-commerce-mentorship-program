# E-commerce-mentorship-program

# DB schema script


Product table
==============
```sql
CREATE TABLE product (
    product_id      UUID PRIMARY KEY,
    product_name    VARCHAR(100) NOT NULL,
    description     VARCHAR(255),
    price           DECIMAL(10,2) NOT NULL,
    stock_quantity  INT DEFAULT 0,
    category_id     UUID NOT NULL,
    FOREIGN KEY (category_id) REFERENCES category(category_id)
);
```

Customer table
==============
```sql
CREATE TABLE customer (
    customer_id      UUID PRIMARY KEY,
    first_name       VARCHAR(100) NOT NULL,
    last_name        VARCHAR(100) NOT NULL,
    email            VARCHAR(100) NOT NULL,
    password         VARCHAR(100) NOT NULL
);
```

Category table
==============
```sql
CREATE TABLE category (
    category_id       UUID PRIMARY KEY,
    category_name     VARCHAR(50) NOT NULL
);
```

Order table
============
```sql
CREATE TABLE orders (
order_id         UUID NOT NULL PRIMARY KEY,
total_amount     DECIMAL(10,2) NOT NULL,
order_date       TIMESTAMP NOT NULL,
customer_id      UUID NOT NULL,
FOREIGN KEY (customer_id) REFERENCES customer(customer_id)
);
```

Order_details table
===================
```sql
CREATE TABLE orders_details (
order_details_id   UUID NOT NULL PRIMARY KEY,
quantity           INT NOT NULL,
unit_price         DECIMAL(10,2) NOT NULL,
order_id           UUID NOT NULL,
product_id         UUID NOT NULL,
FOREIGN KEY (order_id) REFERENCES orders(order_id),
FOREIGN KEY (product_id) REFERENCES product(product_id)
);
```
==============
# ERD daigram
<img width="768" height="771" alt="e-commerceERD drawio" src="https://github.com/user-attachments/assets/31dc9ce8-59bd-421d-84ea-32ad85ee82dc" />


Write an SQL query to generate a daily report of the total revenue for a specific date.
===================
```sql
SELECT o.order_date , SUM(od.quantity * od.unit_price) AS total_revenue
FROM orders o join orders_details od
ON o.order_id = od.order_id
WHERE o.order_date::date = '2025-11-21'
GROUP BY o.order_date
```
Write an SQL query to generate a monthly report of the top-selling products in a given month.
===================
```sql
SELECT p.product_name , SUM(od.quantity) AS sold_quantity, DATE_TRUNC('month', o.order_date) AS month
FROM product p
JOIN orders o ON p.product_id = o.product_id
JOIN orders_details od ON od.order_id = o.order_id
WHERE EXTRACT(MONTH FROM o.order_date) = 2
GROUP BY p.product_name , DATE_TRUNC('month', o.order_date)
ORDER BY sold_quantity desc
```
Write a SQL query to retrieve a list of customers who have placed orders totaling more than $500 in the past month.
Include customer names and their total order amounts.
===================
```sql
SELECT CONCAT(c.first_name,' ',c.last_name) AS full_name , SUM(od.quantity * od.unit_price) AS total_price
FROM customer c
JOIN orders o ON c.customer_id = o.customer_id
JOIN orders_details od ON od.order_id = o.order_id
WHERE o.order_date BETWEEN DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '1 month' 
      AND DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '1 second'
GROUP BY c.first_name, c.last_name
HAVING SUM(od.quantity * od.unit_price) >= 500
ORDER BY total_price
```
How we can apply a denormalization mechanism on customer and order entities.
===================
Denormalization is the process of combining tables or adding redundant data to make queries faster.
so applying this concept on customer and order entities will make queries read faster and it's ok if it will reduce complex joins so the result entity will be:
```sql
ALTER TABLE customer
ADD COLUMN total_amount DECIMAL(10,2) DEFAULT 0,
ADD COLUMN order_date TIMESTAMP,
ADD COLUMN order_count INT DEFAULT 0;
```
Write a SQL query to search for all products with the word "camera" in either the product name or description.
===================
```sql
SELECT *
FROM product
WHERE product_name LIKE '%camera%' OR description LIKE '%camera%';
```
Can you design a query to suggest popular products in the same category for the same author,
excluding the Purchased product from the recommendations?
===================
```sql
SELECT product_id, name, description, category_id FROM product
WHERE product_id NOT IN
(SELECT Product_ID FROM order_history WHERE customer_id = <customer_id>)
AND category_id IN
(SELECT category_id FROM order_history WHERE customer_id = <cusotmer_ID>)
```
