---
description: SELECT 서브쿼리
---

# Stickiness

* 앱 또는 웹 서비스의 사용자가 얼마나 자주 서비스를 이용하는지를 나타내는 지표
* 즉, 사용자들이 앱 또는 웹 사이트에 얼마나 "달라붙어(stick)" 있는지를 측정하는 것을 의미함
* 보통 하단처럼 구함
* $$
  Stickiness=WAU/DAU​
  $$

```sql
with step1 AS (
  SELECT 
    order_date AS dt,
    COUNT(DISTINCT customer_id) dau,
    (SELECT COUNT(DISTINCT r2.customer_id)
    FROM records r2 
    WHERE r2.order_date BETWEEN DATE_SUB(r1.order_date, INTERVAL 6 DAY) AND r1.order_date) wau
  FROM records r1
  GROUP BY 1
)

SELECT dt, dau, wau, ROUND(dau/wau,2) stickiness
FROM step1
WHERE dt LIKE '2020-11%'
```
