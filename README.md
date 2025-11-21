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
total_amount     int NOT NULL,
order_date       TIMESTAMP NOT NULL,
customer_id      UUID NOT NULL,
category_id      UUID NOT NULL
FOREIGN KEY (category_id) REFERENCES category(category_id)
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
order_id        UUID NOT NULL
FOREIGN KEY (order_id) REFERENCES order(order_id)
)
```
==============
# ERD daigram
<img width="748" height="771" alt="e-commerceERD drawio" src="https://github.com/user-attachments/assets/4925069b-6402-4952-a9b6-a86e47eaaf45" />


