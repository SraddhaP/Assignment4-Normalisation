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
CREATE TABLE target_products AS 
SELECT grocery_name, product_price, product_rating, prd_id, qty, category, product_name
FROM target_products_df;

CREATE TABLE samsclub_products AS 
SELECT grocery_name, product_price, product_rating, prd_id, qty, category, product_name
FROM samsclub_products_df;

CREATE TABLE walmart_products AS 
SELECT grocery_name, product_price, product_rating, prd_id, qty, category, product_name
FROM walmart_products_df;

```
#### Creating the product link tables
```sql
CREATE TABLE walmart_product_links AS
SELECT prd_id, store_link, product_link
FROM walmart_products_df;

CREATE TABLE target_product_links AS
SELECT prd_id, store_link, product_link
FROM target_products_df;

CREATE TABLE samsclub_product_links AS
SELECT prd_id, store_link, product_link
FROM samsclub_products_df;
```
#### Creating the product location table
```sql
CREATE TABLE walmart_product_location AS
SELECT prd_id, branch_id, zipcode
FROM walmart_products_df;

CREATE TABLE target_product_location AS
SELECT prd_id, branch_id, zipcode
FROM target_products_df;

CREATE TABLE samsclub_product_location AS
SELECT prd_id, branch_id, zipcode
FROM samsclub_products_df;
```

### USE CASES WITH VIEWS:
1.Display the product ratings of walmart and target
```sql
Create view product_ratings as
SELECT 
    walmart_products.grocery_name,
    walmart_products.product_rating AS walmart_ratings,
    target_products.product_rating AS target_ratings
FROM
    walmart_products
        INNER JOIN
    target_products ON walmart_products.grocery_name = target_products.grocery_name;

```
2.Compare the prices of products in walmart and samsclub
```sql
Create view product_prices as
SELECT 
    s.grocery_name,
    w.product_price AS walmart_price,
    s.product_price AS samsclub_price
FROM
    walmart_products AS w
        INNER JOIN
    samsclub_products AS s ON w.grocery_name = s.grocery_name

```
3.Display all the product link that is common in Walmart and target
```sql
SELECT 
    w.product_link AS walmart_product_link,
    t.product_link AS target_product_link
FROM
    walmart_products_links w
        INNER JOIN
    target_products_links t ON t.grocery_name = w.grocery_name;  
```

4.Displaying all the categories in target and categories that is common in target and sams club
```sql
Create view categories as
SELECT 
  t.category
FROM
  target_products t
      LEFT JOIN
  samsclub_products s ON t.category = s.category;

```
5.Display all female employees in target and Walmart
```sql
Create view female_employees as
SELECT 
    w.first_name AS walmart_emp, t.first_name AS target_emp
FROM
    walmart_employees w,
    target_employees t
WHERE
    w.gender = 'Female'
        AND t.gender = 'Female'
ORDER BY w.emp_id , t.emp_id;
    
```
6.Display the total count of Diary products in target
```sql
Create view dairy_products as
SELECT 
    COUNT(grocery_name)
FROM
    target_products
WHERE
    category = 'Diary';

```

7.Get the address that has both Sam's club and Target
```sql
Create view address as
SELECT 
    t.address
FROM
    target_branch t
        INNER JOIN
    samsclub_branch s ON t.zipcode = s.zipcode;
```

8.Display the list of products and its price which is available in both walmart and target
```sql
Create view products_and_price as
SELECT 
    t.grocery_name, t.product_price
FROM
    target_products t
        INNER JOIN
    walmart_products i ON i.grocery_name = t.grocery_name;

```
9.Get the URL for Sam's Club
```sql
Create view samsclub_url as
SELECT 
    merchant_url
FROM
    merchant
WHERE
    mechant_name LIKE '%Sam%';
 ```
    
10.Get the address in which both Walmart and Target is Located
 ```sql
  Create view location_adderss as
SELECT 
    w.address
FROM
    walmart_branch w
        INNER JOIN
    target_branch t ON w.zipcode = t.zipcode;
 ```
 
