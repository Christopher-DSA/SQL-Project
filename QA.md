What are your risk areas? Identify and describe them.
Some risk areas with this data are making sure the logic is correct when answering questions about the data. For example let's say we wanted to get the number of distinct visitors on the website. We might think to do this:

```sql
SELECT COUNT(full_visitor_id) AS total_unique_visitors
FROM cleaned_all_sessions;
```

However doing this doesn't each full_visitor_is is actually unique meaning that we could double count visitors.
Instead we should do this:

```sql
SELECT COUNT(DISTINCT full_visitor_id) AS total_unique_visitors
FROM cleaned_all_sessions;
```

QA Process:
Describe your QA process and include the SQL queries used to execute it.

For example, when I was solving the question: compute the percentage, I used this QA process.

--This query calculate the percentage of visitors who made a purchase on the website.

--QA: Does the logic used to calculate these percentages make sense?

WITH count_of_sales AS (
SELECT
  COUNT(DISTINCT CASE WHEN total_transaction_revenue > 0 THEN full_visitor_id END) AS count_with_revenue,
  COUNT(DISTINCT CASE WHEN total_transaction_revenue = 0 THEN full_visitor_id END) AS count_zero_revenue,
  COUNT(DISTINCT full_visitor_id) AS total_distinct_visitors
FROM cleaned_all_sessions
)
SELECT * FROM count_of_sales

-- Step 3: Subquery Validation
-- Execute the subquery separately and verify the results against a sample dataset or known values.

SELECT *
FROM count_of_sales;
