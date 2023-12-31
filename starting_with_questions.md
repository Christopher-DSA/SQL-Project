Answer the following questions and provide the SQL queries used to find the answer.

    
# **Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

```sql
--This query returns the total revenue generated each country/city combination.
SELECT SUM(total_transaction_revenue), country, city
FROM cleaned_all_sessions WHERE total_transaction_revenue > 0 AND city != 'not available in demo dataset'
GROUP BY DISTINCT full_visitor_id, country, city
ORDER BY SUM(total_transaction_revenue) DESC, country, city 
 ```

Answer:

The cities with the highest level of transaction revenues on the site pictured below.

It seems that most revenue is generated by visitors from the USA.

![image](https://github.com/Christopher-DSA/SQL-Project/assets/132075292/6c7a12ac-67c5-433e-ba5b-b047a9e738d1)

# **Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

```sql
--This query returns the average quantity each city/country combination orders in descending order
SELECT AVG(product_quantity) AS average_product_quantity_ordered, country, city
FROM cleaned_all_sessions
WHERE product_quantity IS NOT NULL AND city != 'not available in demo dataset' AND city != '(not set)'
GROUP BY country, city
ORDER BY AVG(product_quantity) DESC, country, city 
 ```
![image](https://github.com/Christopher-DSA/SQL-Project/assets/132075292/15977c43-0cd9-44c0-9d1b-86f300be640f)


Answer:
The average number of products ordered from visitors in each city and country pictured above.

# **Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

```sql
SELECT COUNT(v2product_category), v2product_category, country, city FROM cleaned_all_sessions WHERE v2product_category IS NOT NULL GROUP BY country, city, v2product_category ORDER BY COUNT(v2product_category) DESC, country, city 
```

Answer:
Yes, there are patterns in the types (product categories) of products ordered from visitors in each city and country:

### Top 3 product categorys by country:

### United States
![image](https://github.com/Christopher-DSA/SQL-Project/assets/132075292/e80a14b8-5409-446c-88a4-52b8cd48c23c)

### Germany
![image](https://github.com/Christopher-DSA/SQL-Project/assets/132075292/cded9821-54eb-45f9-b880-38f5090d8ecb)

### Canada
![image](https://github.com/Christopher-DSA/SQL-Project/assets/132075292/d302c057-5f99-4342-a817-90f2d5f102df)


# **Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**

SQL Queries:

This query will show the top selling product in each city/country combination.
```sql
SELECT country, city, v2product_name, product_count,
       RANK() OVER (PARTITION BY country ORDER BY product_count DESC) AS rank
FROM (
    SELECT country, city, v2product_name, COUNT(v2product_name) AS product_count
    FROM cleaned_all_sessions
    GROUP BY country, city, v2product_name
) AS subquery
WHERE country != '(not set)' AND country!= '(not set)' and city != ('not available in demo dataset')
ORDER BY country, rank;
```
This query will show the top selling product in each different country not accounting for cities.
```sql
SELECT country, v2product_name, product_count, rank
FROM (
    SELECT country, v2product_name, COUNT(v2product_name) AS product_count,
           RANK() OVER (PARTITION BY country ORDER BY COUNT(v2product_name) DESC) AS rank
    FROM cleaned_all_sessions
    GROUP BY country, v2product_name
) AS subquery
WHERE rank = 1 AND country != '(not set)'
ORDER BY product_count desc
```

Answer:
Examining the the top-selling product from each city/country, a pattern worth noting the products sold is that the United States has the most of any one product being sold compared to every other country.


# **Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

This query will show the total revenue generated by each city/country combination.
``` sql
SELECT country, city, SUM(total_transaction_revenue) AS total_revenue
FROM cleaned_all_sessions
GROUP BY country, city
HAVING SUM(total_transaction_revenue) > 0
ORDER BY country, city;
```

Answer:
Yes, we can summarize the impact of revenue generated from each city/country.




