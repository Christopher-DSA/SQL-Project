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
		social_engagement_type,
		CASE WHEN units_sold::int IS NULL THEN 0 ELSE units_sold END,
		pageviews,
		timeonsite,
		bounces,
		revenue,
		ROUND(unit_price/1000000,2) AS unit_price
FROM analytics
```
