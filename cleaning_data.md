What issues will you address by cleaning the data?
Issue 1: The unit cost in the data needs to be divided by 1,000,000.

Issue 2: The column 'userid' in the analytics table is null for every row and it is also not used in any other table as a primary key or a foreign key. 'fullvisitorID' is unique to every visitor so we don't really need this column at all.

Issue 3: 'units_sold', 'timeonsite' and 'revenue' are all completely null as well, I have confirmed this by using a query to filter at all rows that have null values in each of these columns. They all returned 0 rows meaning that every row does in fact have null as the value.

## Missing Values in units_sold column
For the units_sold column in the analytics table there are columns where units_sold is NULL. It is unclear whether this is meant to represent 0 or that the data is missing. By checking other columns in the same 'analytics' table, for example 'unit_price' there are infact rows with the value of 0 to represent zero sales instead of NULL. Also by checking whether or not there any rows in the 'units_sold' table have a value zero, we can see that none exist. Therefore moving forward we are going to go with the assumption that these NULL values do infact represent 0 'units_sold'.


# Queries:
Below, provide the SQL queries you used to clean your data.

## 1. How the issue was discovered
``` sql
1. SELECT (unit_price / 1000000) FROM analytics
```
## 3. How the issue was discovered
``` sql
3. SELECT * FROM analytics WHERE userid IS NOT NULL
```

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
