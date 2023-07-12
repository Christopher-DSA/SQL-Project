Consider the data you have available to you. You can use the data to: - find all duplicate records - find the total number of unique visitors (fullVisitorID) - find the total number of unique visitors by referring sites - find each unique product viewed by each visitor - compute the percentage of visitors to the site that actually makes a purchase

In the starting_with_data.md file, write 3 - 5 new questions that you could answer with this database. For each question, include The queries you used to answer the question The answer to the question

Question 1:  What are the toal number of unique vistors to the website?

SQL Queries: 

``` sql
SELECT COUNT(DISTINCT full_visitor_id) AS total_unique_visitors
FROM cleaned_all_sessions;
```
![image](https://github.com/Christopher-DSA/SQL-Project/assets/132075292/b23bd889-f17b-46d5-adc8-c9205cda0c4c)


Answer: 
 The website had 14223 unique visitors.


Question 2: Which countries contained the most amount of unique vistors, what are the top 3?

SQL Queries:

``` sql
SELECT country, COUNT(DISTINCT full_visitor_id) AS visitor_count
FROM cleaned_all_sessions
GROUP BY country
ORDER BY visitor_count DESC
LIMIT 3;
```
Answer:
![image](https://github.com/Christopher-DSA/SQL-Project/assets/132075292/c63b9a0d-3519-413a-a672-5a9b9a33ef90)


The countries with most uniques visitors are the United States with 8118 visitors, India with 693 visitors and Unitied Kingdom with 641 vistors.
This confirms that the website is most popular with customers from the united states.

Question 3:  Compute the percentage of visitors to the site that actually makes a purchase.

SQL Queries:

``` sql
WITH count_of_sales AS (
SELECT
  COUNT(DISTINCT CASE WHEN total_transaction_revenue > 0 THEN full_visitor_id END) AS count_with_revenue,
  COUNT(DISTINCT CASE WHEN total_transaction_revenue = 0 THEN full_visitor_id END) AS count_zero_revenue,
  COUNT(DISTINCT full_visitor_id) AS total_distinct_visitors,
  (COUNT(DISTINCT CASE WHEN total_transaction_revenue > 0 THEN full_visitor_id END) * 100.0 / COUNT(DISTINCT full_visitor_id)) AS percentage_with_revenue,
  (COUNT(DISTINCT CASE WHEN total_transaction_revenue = 0 THEN full_visitor_id END) * 100.0 / COUNT(DISTINCT full_visitor_id)) AS percentage_zero_revenue
FROM cleaned_all_sessions
)

SELECT count_with_revenue/total_distinct_visitors::numeric, count_zero_revenue/total_distinct_visitors::numeric
FROM count_of_sales
```
Answer: 99.48 of customers do not make a purchase when they visit the website. Perhaps the website could use some more design changes to increase sales.
