Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

```sql
SELECT SUM(transaction_revenue), country, city FROM cleaned_all_sessions GROUP BY country, city ORDER BY SUM(transaction_revenue) DESC, country, city 
 ```

Answer:

The cities with the highest level of transaction revenues on the site are the United States and the two cities of Sunnyval and (Not available in demo dataset).

![image](https://github.com/Christopher-DSA/SQL-Project/assets/132075292/20f9ddd2-83ec-4235-ad10-c1fefc3c1c4a)

**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

```sql
SELECT AVG(product_quantity), country, city FROM cleaned_all_sessions WHERE product_quantity IS NOT NULL GROUP BY country, city ORDER BY AVG(product_quantity) DESC, country, city 
 ```

Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







