# E-commerce-mentorship-program

# DB schema script


Product table
==============

```sql
CREATE TABLE product (
    product_id      UUID NOT NULL PRIMARY KEY,
    product_name    VARCHAR(20) NOT NULL,
    description     VARCHAR(50),
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
customer_id      UUID NOT NULL PRIMARY KEY,
first_name       VARCHAR(30) NOT NULL,
last_name        VARCHAR(30) NOT NULL,
email            VARCHAR(30) NOT NULL,
password         VARCHAR(10) NOT NULL
)
```

Category table
==============
```sql
CREATE TABLE category (
category_id       UUID NOT NULL PRIMARY KEY,
category_name     VARCHAR(20) NOT NULL
)
```

Order table
============
```sql
CREATE TABLE order (
order_id         UUID NOT NULL PRIMARY KEY,
total_amount     DECIMAL(10,2) NOT NULL,
order_date       TIMESTAMP NOT NULL,
customer_id      UUID NOT NULL,
product_id       UUID NOT NULL
FOREIGN KEY (product_id) REFERENCES product(product_id),
FOREIGN KEY (customer_id) REFERENCES customer(customer_id)
)
```

Order_details table
===================
```sql
CREATE TABLE order_details (
order_details_id   UUID NOT NULL PRIMARY KEY,
quantity           int NOT NULL,
unit_price         DECIMAL(10,2) NOT NULL,
order_id           UUID NOT NULL,
FOREIGN KEY (order_id) REFERENCES order(order_id)
)
```
==============
# ERD daigram
<img width="768" height="771" alt="e-commerceERD drawio" src="https://github.com/user-attachments/assets/928cb2b9-eff6-4dde-b652-dd7e0a569311" />


Write an SQL query to generate a daily report of the total revenue for a specific date.
===================
```sql
SELECT o.order_date , SUM(od.quantity * od.unit_price) AS total_revenue
FROM order o join order_details od
ON o.order_id = od.order_id
WHERE o.order_date::date = '2025-11-21'
GROUP BY o.order_date
```
Write an SQL query to generate a monthly report of the top-selling products in a given month.
===================
```sql
SELECT p.product_name , SUM(od.quantity) AS sold_quantity, DATE_TRUNC('month', o.order_date) AS month
FROM product p
JOIN order o ON p.product_id = o.product_id
JOIN order_details od ON od.order_id = o.order_id
WHERE EXTRACT(MONTH FROM o.order_date) = 2
GROUP BY p.product_name , DATE_TRUNC('month', o.order_date)
ORDER BY sold_quantity desc
```
Write a SQL query to retrieve a list of customers who have placed orders totaling more than $500 in the past month.
Include customer names and their total order amounts.
===================
```sql
SELECT CONCAT(c.first_name,c.last_name) , SUM(od.quantity * od.unit_price) >= 500
FROM customer c
JOIN order o ON c.customer_id = o.customer_id
JOIN order_details od ON od.order_id = o.order_id
WHERE
GROUP BY
ORDER BY
```

