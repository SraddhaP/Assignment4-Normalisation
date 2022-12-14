# Normalization

### Members: 
Shruti Milind Randive, Amretasre Rengarajan Thiruvengadam, Sraddha Pedda Gangireddy Gari, Dharma Thanishq Nimmala.


### Introduction:
The Grocery_Recommendation_System database contains data on all the products available in merchants such as Walmart, Target, Samsclub, and the online delivery application Instacart. This data has been web scrapped from the Google Shopping web platform which includes millions of products from various merchants. The merchant table contains these merchant details along with their web URLs. The product tables contain columns such as product name, price, product inks, ratings, location in terms of zip code, etc. The Branch table data have been scrapped from the "Yellow Pages" website which contains the address and zip codes of all the merchant locations that are available in Boston City. The generated Employee tables contain data concerning employee data such as employee id, first_name, last_name, and age for the selected merchants.

## Create Table SQL Queries: ##

```sql
CREATE DATABASE Grocery_Recommendation_System;
```

```sql
CREATE table merchant (
merchant_id INT NOT NULL auto_increment,
mechant_name VARCHAR(20),
merchant_URL Varchar(50),
primary key(merchant_id)
);
```

```sql
CREATE table walmart_branch (
merchant_name VARCHAR(20),
address VARCHAR(50),
location VARCHAR(50),
zipcode INT NOT NULL,
contact INT NOT NULL,
branch_id INT NOT NULL,
merchant_id INT NOT NULL auto_increment,
primary key(merchant_id)
);
```

```sql
CREATE table walmart_products (
grocery_name VARCHAR(20),
product_price INT NOT NULL,
store VARCHAR(20),
product_link VARCHAR(50),
product_rating FLOAT,
store_link VARCHAR(50),
category VARCHAR(50),
qty VARCHAR(10),
name VARCHAR(50),
availability  VARCHAR(20),
zipcode INT NOT NULL,
branch_id INT NOT NULL,
prd_id INT NOT NULL auto_increment,
primary key(prd_id)
);
```

```sql
CREATE table target_branch (
merchant_name VARCHAR(20),
address VARCHAR(50),
location VARCHAR(50),
zipcode INT NOT NULL,
contact INT NOT NULL,
branch_id INT NOT NULL,
merchant_id INT NOT NULL auto_increment,
primary key(merchant_id)
);
```

```sql
CREATE table target_products (
grocery_name VARCHAR(20),
product_price INT NOT NULL,
store VARCHAR(20),
product_link VARCHAR(50),
product_rating FLOAT,
store_link VARCHAR(50),
category VARCHAR(50),
qty VARCHAR(10),
name VARCHAR(50),
availability  VARCHAR(20),
zipcode INT NOT NULL,
branch_id INT NOT NULL,
prd_id INT NOT NULL auto_increment,
primary key(prd_id)
);
```

```sql
CREATE table samsclub_branch (
merchant_name VARCHAR(20),
address VARCHAR(50),
location VARCHAR(50),
zipcode INT NOT NULL,
contact INT NOT NULL,
branch_id INT NOT NULL,
merchant_id INT NOT NULL auto_increment,
primary key(merchant_id)
);
```

```sql
CREATE table samsclub_products (
grocery_name VARCHAR(20),
product_price INT NOT NULL,
store VARCHAR(20),
product_link VARCHAR(50),
product_rating FLOAT,
store_link VARCHAR(50),
category VARCHAR(50),
qty VARCHAR(10),
name VARCHAR(50),
availability  VARCHAR(20),
zipcode INT NOT NULL,
branch_id INT NOT NULL,
prd_id INT NOT NULL auto_increment,
primary key(prd_id)
);
```

```sql 
CREATE table instacart_products (
grocery_name VARCHAR(20),
product_price INT NOT NULL,
store VARCHAR(20),
product_link VARCHAR(50),
product_rating FLOAT,
store_link VARCHAR(50),
category VARCHAR(50),
qty VARCHAR(10),
name VARCHAR(50),
availability  VARCHAR(20),
branch_id INT NOT NULL,
prd_id INT NOT NULL auto_increment,
primary key(prd_id)
);
```

```sql
CREATE table walmart_employees(
empid INT NOT NULL auto_increment,
first_name VARCHAR(20),
last_name VARCHAR(20),
contact INT NOT NULL,
gender VARCHAR(10),
age INT NOT NULL,
primary key(empid)
);
```

```sql
CREATE table samsclub_employees(
empid INT NOT NULL auto_increment,
first_name VARCHAR(20),
last_name VARCHAR(20),
contact INT NOT NULL,
gender VARCHAR(10),
age INT NOT NULL,
primary key(empid)
);
```

```sql
CREATE table target_employees(
empid INT NOT NULL auto_increment,
first_name VARCHAR(20),
last_name VARCHAR(20),
contact INT NOT NULL,
gender VARCHAR(10),
age INT NOT NULL,
primary key(empid)
);
```

### Adding constraints to tables: ###

	Adding constraints foreign key constraints for all the tables

```sql
ALTER TABLE walmart_branch
ADD CONSTRAINT `wal_branch_idk`
FOREIGN KEY (merchant_id) REFERENCES merchant(merchant_id); 
```

```sql
ALTER TABLE target_branch
ADD CONSTRAINT `tar_branch_idk`
FOREIGN KEY (merchant_id) REFERENCES merchant(merchant_id); 
```

```sql
ALTER TABLE samsclub_branch
ADD CONSTRAINT `sams_branch_idk`
FOREIGN KEY (merchant_id) REFERENCES merchant(merchant_id); 
```


### 1st Normal Form: 

	The scraped data from google shopping has the quantity as a part of the product name, hence to achieve the first normal form we have split the product name into quantity and the product name. This is implemented through pandas, directly in the data frame.

### 2nd Normal Form: 

 	All the columns in the product tables have no partial dependencies or no candidate keys, thus a second normal form is achieved.

### 3rd Normal Form: 

	Initially columns such as product_link, store_link, product_rating, etc were a part of the product table. Now, the product table is splitted into three tables namely product, links and product and branch relation table. The product  and the links table for each merchant  is only dependent on the primary key which is the product id. Whereas the product and branch relation table depends on both the product id and the branch id which is the foreign key from merchant_branch table. Therefore, third normal form is achieved. 

#### Creating the product tables to achieve Normalization ####

```sql
CREATE TABLE walmart_product AS 
SELECT grocery_name, product_price, product_rating, prd_id, qty, category, product_name
FROM walmart_products;
```

```sql
CREATE TABLE target_product AS 
SELECT grocery_name, product_price, product_rating, prd_id, qty, category, product_name
FROM target_products;
```

```sql
CREATE TABLE samsclub_product AS 
SELECT grocery_name, product_price, product_rating, prd_id, qty, category, product_name
FROM samsclub_products;
```

```sql
Creating the product link tables
CREATE TABLE walmart_product_links AS
SELECT prd_id, store_link, product_link
FROM walmart_product;
```

```sql
CREATE TABLE target_product_links AS
SELECT prd_id, store_link, product_link
FROM target_product;
```

```sql
CREATE TABLE samsclub_product_links AS
SELECT prd_id, store_link, product_link
FROM samsclub_product;
```

```sql
Creating the product branch relation table
CREATE TABLE walmart_product_branch_relation AS
SELECT prd_id, branch_id, zipcode
FROM walmart_products;
```

```sql
CREATE TABLE target_product_branch_relation AS
SELECT prd_id, branch_id, zipcode
FROM target_products;
```

```sql
CREATE TABLE samsclub_product_branch_relation AS
SELECT prd_id, branch_id, zipcode
FROM samsclub_products;
```

### USE CASES WITH VIEWS:


