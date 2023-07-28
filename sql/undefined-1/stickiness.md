# Stickiness

* 재방문 정도를 확인하기 위해 Stickiness (고객 고착도)를 확인
* 일반적으로 Stickiness는 DAU를 WAU 또는 MAU로 나누어 계산

$$
Stickiness=WAUDAU​
$$

DAU, WAU, MAU의 의미는 아래와 같습니다.

* DAU (Daily Active User) - 일간 활성 고객 수
* WAU (Weekly Active User) - 주간 활성 고객 수
* MAU (Monthly Active User) - 월간 활성 고객 수



```sql
WITH temp AS (
  SELECT 
    r.order_date dt, 
    COUNT(distinct r.customer_id) dau,
    (SELECT COUNT(distinct r2.customer_id) FROM records r2
    WHERE r2.order_date BETWEEN DATE_SUB(r.order_date, INTERVAL 6 DAY) AND r.order_date ) wau
  FROM records r
  WHERE DATE_FORMAT(order_date, '%Y-%m') = '2020-11'
  GROUP BY 1 )

SELECT *, ROUND(dau/wau,2) AS stickiness
FROM temp
```
