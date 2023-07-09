What issues will you address by cleaning the data?

# 1. Cleaning the 'analytics' table

## The 'unit_cost' in the data needs to be divided by 1,000,000.
By examining the unit cost of the products in this dataset, they are very unsually high for the types of products being sold. 

To investigate further I checked the 'total_transaction_revenue' column in the 'all_sessions' table. An example of what I found. Nest® Cam Outdoor Security Camera - USA Where the 'product_price' was 119000000. This seems a little bit high for a security camera so I googled the actual price: 179.72 United States Dollar. This is when I realized that there were extra zeros in the entry. 

Dividing by 1,000,000 would make it $119 USD which makes alot more sense for this kind of product. We will be diving all values with these extra 0's to remove the extra six extra 0's added in error.

## Missing Values in units_sold column
For the units_sold column in the analytics table there are columns where units_sold is NULL. It is unclear whether this is meant to represent 0 or that the data is missing. By checking other columns in the same 'analytics' table, for example 'unit_price' there are infact rows with the value of 0 to represent zero sales instead of NULL. Also by checking whether or not there any rows in the 'units_sold' table have a value zero, we can see that none exist. Therefore moving forward we are going to go with the assumption that these NULL values do infact represent 0 'units_sold'.

## The column 'userid' in the analytics table is redundant?
The value of user_id is null for every row and it is also not used in any other table as a primary key or a foreign key. 'fullvisitorID' is unique to every visitor so we don't really need this column at all. We can use 'fullvisitorID' to represent each visitor instead of userID since it is unique to each vist. Was this column implemented for future proofing?

# Queries:
Below, provide the SQL queries you used to clean your data.

## 1. How the six extra 0's were discovered
``` sql
1. SELECT unit_price FROM analytics
```
Observation: These prices seem too high for the types of products being sold.
``` sql
1. SELECT (unit_price / 1000000) FROM analytics
```
Purpose: Now with the extra 0's removed the prices are accurately represented.

## 2. Missing user_id in all columns
``` sql
3. SELECT * FROM analytics WHERE userid IS NOT NULL
```
Result: Returned 0 rows.


## 4. How the issue was discovered: (Missing Values in units_sold column)
``` sql
SELECT * FROM analytics WHERE units_sold IS NULL
```
Returned: 4205975 Rows where units_sold IS NULL
### Checking to make sure there are no 0's anywhere in this column.
``` sql
SELECT units_sold FROM analytics WHERE units_sold = 0 
```
Returned: 0 Rows where units_sold = 0
, This means we are safe to assume that NULL was likely meant to represent 0.
### We will change these NULL Values to 0
``` sql
SELECT CASE WHEN units_sold IS NULL THEN 0 ELSE unit_price END FROM analytics 
```
