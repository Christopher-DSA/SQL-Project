``` sql
SELECT COUNT(*) AS identical_count
FROM all_sessions_backup
WHERE total_transaction_revenue IS NOT DISTINCT FROM transaction_revenue;

SELECT * FROM all_sessions_backup
```
