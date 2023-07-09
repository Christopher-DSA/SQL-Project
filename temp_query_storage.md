```sql
WITH appears_twice AS 
(
SELECT full_visitor_id FROM all_sessions
GROUP BY full_visitor_id
HAVING COUNT(full_visitor_id) > 1
)

SELECT * FROM all_sessions WHERE full_visitor_id IN(SELECT full_visitor_id FROM appears_twice) ORDER BY full_visitor_id
```       
