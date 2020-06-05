# Tables and Rules 

## Accounts Table 

imagine that this table has about 70,000 rows, and that hierarchies can go to about 5 levels deep

| account_id | account_parent_id | account_name | first_purchase_year | account_segment |
|------------|-------------------|--------------|---------------------|-----------------|
| 1          | NULL              | ABC Hospital | 2016                | Strategic       |
| 2          | NULL              | DEF Hospital | 2017                | Client          |
| 3          | 2                 | GHI Hospital | 2016                | NULL            |
| 4          | 3                 | JKL Hospital | NULL                | Client          |

### Account Hierarchy

specified by parent. top level does not have a parent. for above, hierarchy would look like this:
* ABC
* DEF
  * GHI
    * JKL

## Magic Buckets Table

| first_purchase_year | account_segment | magic_bucket_name |
|---------------------|-----------------|-------------------|
| 2016                | Strategic       | Green             |
| 2016                | NULL            | Yellow            |
| 2017                | Client          | Orange            |
| NULL                | Client          | Red               |

## Products Table 

| product_id | product_category |      product_name     |
|------------|------------------|-----------------------|
| 1          | service          | widget repair         |
| 2          | subscription     | sprocket subscription |
| 3          | subscription     | widget subscription   |

## Sales Table

Rules: 
* if sale_term_months is null, treat it as 12
* if start_date is null, treat it as the sale_close_date

| sale_id | product_id | account_id | sale_is_successful | sale_close_date | sale_price | sale_term_months | start_date |
|---------|------------|------------|--------------------|-----------------|------------|------------------|------------|
| 1       | 1          | 1          | 0                  | 2019-01-01      | 10         | 12               | 2019-01-01 |
| 2       | 1          | 1          | 1                  | 2019-12-01      | 100        | 12               | 2019-12-01 |
| 3       | 3          | 3          | 1                  | 2019-12-01      | 1000       | NULL             | 2020-01-01 |
| 4       | 2          | 2          | 1                  | 2019-12-01      | 10         | 12               | NULL       |
| 5       | 1          | 3          | 1                  | 2020-06-01      | 100        | 12               | 2020-06-01 |
| 6       | 2          | 1          | 1                  | 2020-06-01      | 100        | 12               | 2020-09-01 |
| 7       | 2          | 1          | 1                  | 2020-06-01      | 1000       | 24               | 2020-06-01 |


# Quickstart

```sql
create table #accounts (
    account_id  int identity(1,1) primary key,
    account_parent_id int null,
    account_name varchar(50) not null,
    first_purchase_year int null,
    account_segment varchar(50) null
);

insert into #accounts values
    (NULL,'ABC Hospital', 2016, 'Strategic'),
    (NULL,'DEF Hospital', 2017, 'Client'),
    (2,'GHI Hospital', 2016, NULL),
    (3,'JKL Hospital', NULL, 'Client');

create table #magic_buckets( 
    first_purchase_year int null,
    account_segment varchar(50) null,
    magic_bucket_name varchar(50) primary key
);

insert into #magic_buckets values
    (2016,'Strategic','Green'),
    (2016,NULL,'Yellow'),
    (2017,'Client','Orange'),
    (NULL,'Client','Red');

create table #products (
    product_id  int identity(1,1) primary key,
    product_category varchar(50) not null,   
    product_name varchar(50) not null,
);

insert into #products values
    ('service','widget repair'),
    ('subscription','sprocket subscription'),
    ('subscription','widget subscription');

create table #sales(
    sale_id  int identity(1,1) primary key,
    product_id int not null,
    account_id int not null,
    sale_is_successful int not null,
    sale_close_date date not null,
    sale_price decimal(18,2) not null,
    sale_term_months int null,
    start_date date null
);

insert into #sales values
    (1,1,0,'2019-01-01',10,12,'2019-01-01'),
    (1,1,1,'2019-12-01',100,12,'2019-12-01'),
    (3,3,1,'2019-12-01',1000,NULL,'2020-01-01'),
    (2,2,1,'2019-12-01',10,12,NULL),
    (1,3,1,'2020-06-01',100,12,'2020-06-01'),
    (2,1,1,'2020-06-01',100,12,'2020-09-01'),
    (2,1,1,'2020-06-01',1000,24,'2020-06-01')
```