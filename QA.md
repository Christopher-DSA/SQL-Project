What are your risk areas? Identify and describe them.



QA Process:
Describe your QA process and include the SQL queries used to execute it.

For example, when I was solving the question: compute the percentage, I used this QA process.

--This query calculate the percentage of visitors who made a purchase on the website.

--QA: Does the logic used to calculate these percentages make sense?

WITH count_of_sales AS (
  SELECT
    COUNT(DISTINCT CASE WHEN total_transaction_revenue > 0 THEN full_visitor_id END) AS count_with_revenue,
    COUNT(DISTINCT CASE WHEN total_transaction_revenue = 0 THEN full_visitor_id END) AS count_zero_revenue,
    COUNT(DISTINCT full_visitor_id) AS total_distinct_visitors,
    (COUNT(DISTINCT CASE WHEN total_transaction_revenue > 0 THEN full_visitor_id END) * 100.0 / COUNT(DISTINCT full_visitor_id)) AS percentage_with_revenue,
    (COUNT(DISTINCT CASE WHEN total_transaction_revenue = 0 THEN full_visitor_id END) * 100.0 / COUNT(DISTINCT full_visitor_id)) AS percentage_zero_revenue
  FROM cleaned_all_sessions
)

-- Step 3: Subquery Validation
-- Execute the subquery separately and verify the results against a sample dataset or known values.

SELECT *
FROM count_of_sales;
