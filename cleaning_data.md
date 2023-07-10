What issues will you address by cleaning the data?

# 1. Cleaning the 'analytics' table

## 1. The 'unit_cost' in the data needs to be divided by 1,000,000.
By examining the unit cost of the products in this dataset, they are very unsually high for the types of products being sold. 

To investigate further I checked the 'total_transaction_revenue' column in the 'all_sessions' table. An example of what I found. Nest® Cam Outdoor Security Camera - USA Where the 'product_price' was 119000000. This seems a little bit high for a security camera so I googled the actual price: 179.72 United States Dollar. This is when I realized that there were extra zeros in the entry. 

Dividing by 1,000,000 would make it $119 USD which makes alot more sense for this kind of product. We will be diving all values with these extra 0's to remove the extra six extra 0's added in error.

## 2. Missing Values in units_sold column
For the units_sold column in the analytics table there are columns where units_sold is NULL. It is unclear whether this is meant to represent 0 or that the data is missing. 

By checking other columns in the same 'analytics' table, for example 'unit_price' there are infact rows with the value of 0 to represent zero sales instead of NULL. Also by checking whether or not there any rows in the 'units_sold' table have a value zero, we can see that none exist. 

Therefore moving forward we are going to go with the assumption that these NULL values do infact represent 0 'units_sold'.

## 3. Duplicate Rows exist in the analytics table.
These duplicate rows contain the same information yet they exist as two seperate rows. 

By selecting just distinct rows, the number of rows is less than the number of rows when we just select all rows with no filtering.

Therefore before using this data to answer questions, we will remove the duplicate rows.

## 4. The column 'userid' in the analytics table is redundant?
The value of user_id is null for every row and it is also not used in any other table as a primary key or a foreign key.

'fullvisitorID' is unique to every visitor so we don't really need this column at all. We can use 'fullvisitorID' to represent each visitor instead of userID since it is unique to each vistitor regardless of whether or not the leave the website and come back later or not. 

Was this column implemented for future proofing?

## 5. Changing Unix Time stamps to human readable times.
In the 'visit_start_time' from the analytics table the times are saved as unix time stamps. We will convert these to human readable timestamps.

# Queries:
Below, provide the SQL queries you used to clean your data.

## 1. Fixing the unit_price six extra 0's:
``` sql
SELECT unit_price FROM analytics
```
Observation: These prices seem too high for the types of products being sold.
``` sql
SELECT (unit_price / 1000000) AS unitprice FROM analytics 

-- OR can also use the below query to remove unnecessary extra 0's from the end of the price
SELECT ROUND(unit_price/1000000,2) AS unitprice FROM analytics
```
Purpose: Now with the extra 0's removed the prices are accurately represented.

## 2. How the issue was discovered: (Missing Values in units_sold column):
``` sql
SELECT * FROM analytics WHERE units_sold IS NULL
```
Returned: 4205975 Rows where units_sold IS NULL, Which means there are many rows with NULL as their 'units_sold' value
### Checking to make sure there are no 0's anywhere in this column.
``` sql
SELECT units_sold FROM analytics WHERE units_sold = 0 
```
Returned: 0 Rows where units_sold = 0
, This means we are safe to assume that NULL was likely meant to represent 0, Which means there are no rows with 0 as their 'units_sold' value.
### Solution: We will change these NULL Values to 0
``` sql
SELECT CASE WHEN units_sold IS NULL THEN 0 ELSE unit_price END FROM analytics 
```

## 3. Duplicate Rows in the analytics table:
``` sql
SELECT * FROM analytics
Returns: 4301122 rows
```
And then we run this next query:
``` sql
SELECT DISTINCT * FROM analytics
Returns: 1739308 rows
```
We have now determined that there duplicate rows in the dataset, so we will filter them out

Solution:
``` sql
SELECT DISTINCT * FROM analytics
```

## 4. Value for user_id is NULL in every row:
``` sql
3. SELECT * FROM analytics WHERE userid IS NOT NULL
```
Result: Returned 0 rows.
### Solution
Delete the user_id column? Populate it? For now, no action as been taken.

## 5. Changing Unix timestamps to human readable timestamps:
``` sql
SELECT to_timestamp(visit_start_time::int)::text AS "vist_start_time" FROM analytics
```

# 6. Combine all the above conditions in order to get a clean table ready for analysis:
``` sql
CREATE TABLE analytics_cleaned AS 
SELECT DISTINCT
		visit_number,
		visit_id,
		visit_start_time, 
		to_timestamp(visit_start_time::int)::timestamptz AS "full_time_stamp",
		visit_date,
		full_visitor_id,
		userid,
		channel_grouping,
		social_enagement_type AS social_engagement_type,
		CASE WHEN units_sold::int IS NULL THEN 0 ELSE units_sold END AS units_sold,
		pageviews,
		timeonsite,
		bounces,
		revenue,
		ROUND(unit_price/1000000,2) AS unit_price
FROM new_analytics_type
```
### Our 'analytics' table has now been cleaned

# NEXT: Cleaning the all_sessions_table

First, let's take a look at the data.

``` sql
SELECT * FROM all_sessions
```

CREATE TABLE analytics_cleaned AS 
  SELECT DISTINCT
		visit_number,
		visit_id,
		visit_start_time, 
		to_timestamp(visit_start_time::int)::timestamptz AS "full_time_stamp",
		visit_date,
		full_visitor_id,
		userid,
		channel_grouping,
		social_engagement_type,
		CASE WHEN units_sold::int IS NULL THEN 0 ELSE units_sold END,
		pageviews,
		timeonsite,
		bounces,
		revenue,
		ROUND(unit_price/1000000,2) AS unit_price
FROM analytics

