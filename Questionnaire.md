# Questionairre

## General
1. Assume you've acquired a new dataset from your business stakeholders. Your job is to integrate it with other existing data into a cohesive model. What are your next steps?   
2. Have you ever proposed changes to increase data quality / accuracy? How did it turn out and why?
3. What's your favorite progamming language to work with? Why?
4. Tell us about the favorite problem you've solved in the past year.

## SQL Questions see [Setup](setup.md) for table structure and data

1. write a query showing accounts with more than one successfull sale on a single date
2. write a query showing end_date for each sale. end date is the last of the month of the date that is sale_term_months-1 months following the start_date
3. write a query showing successful sales, returning product_name, account_name, magic_bucket_name and average monthly price (sale_price/sale_term_months) per sale_close_date
4. write a query showing the end date of all sales for products with the product_category of subscription
5. write a query showing row number (any order you like) for the magic buckets table (first_purchase_year, account_segment,magic_bucket_name)
6. (bonus) write a query showing the top level account for each account (i.e., GHI and JKL should show DEF as their top level parent, whereas ABC and DEF would each show themselves as top level parent )

