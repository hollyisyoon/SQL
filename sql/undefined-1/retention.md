# Retention

* DATE\_ADD()
* DATE\_FORMAT()

## 클래식 리텐션

* customer\_id, order\_month, first\_order\_month 테이블을 먼저 만든다
* CASE WHEN으로 DATE\_ADD(first\_order\_month, INTERVAL N MONTH) = order\_month 을 만족하는 customer\_id의 count를 센다

```sql
WITH temp AS(
SELECT r.customer_id, r.order_id, DATE_FORMAT(r.order_date, '%Y-%m-01') order_month, DATE_FORMAT(c.first_order_date, '%Y-%m-01') first_order_month
FROM records r JOIN customer_stats c ON r.customer_id = c.customer_id )

SELECT first_order_month, 
  COUNT(DISTINCT order_id) month0, 
  COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 1 month) = order_month THEN order_id END) month1,
  COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 2 month) = order_month THEN order_id END) month2,
  COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 3 month) = order_month THEN order_id END) month3,
  COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 4 month) = order_month THEN order_id END) month4,
  COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 5 month) = order_month THEN order_id END) month5,
  COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 6 month) = order_month THEN order_id END) month6,
  COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 7 month) = order_month THEN order_id END) month7,
  COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 8 month) = order_month THEN order_id END) month8

FROM temp 
GROUP BY 1
```



## 롤링 리텐션

* customer\_id, order\_month, first\_order\_month, last\_order\_month 테이블을 먼저 만든다
* CASE WHEN으로 DATE\_ADD(first\_order\_month, INTERVAL N MONTH) 가 first\_order\_month와 last\_order\_month 사이에 있는지를 확인한다

```sql
WITH step1 AS (
  SELECT 
    customer_id, 
    DATE_FORMAT(order_date, '%Y-%m-01') order_month,
    DATE_FORMAT(MIN(order_date) OVER(PARTITION BY customer_id), '%Y-%m-01') first_order_month,
    DATE_FORMAT(MAX(order_date) OVER(PARTITION BY customer_id), '%Y-%m-01') last_order_month
  FROM records
),
step2 AS (
  SELECT 
    first_order_month,
    COUNT(DISTINCT customer_id) month0,
    COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 1 month) BETWEEN first_order_month AND last_order_month THEN customer_id END) month1,
    COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 2 month) BETWEEN first_order_month AND last_order_month THEN customer_id END) month2,
    COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 3 month) BETWEEN first_order_month AND last_order_month THEN customer_id END) month3,
    COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 4 month) BETWEEN first_order_month AND last_order_month THEN customer_id END) month4,
    COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 5 month) BETWEEN first_order_month AND last_order_month THEN customer_id END) month5,
    COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 6 month) BETWEEN first_order_month AND last_order_month THEN customer_id END) month6,
    COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 7 month) BETWEEN first_order_month AND last_order_month THEN customer_id END) month7,
    COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 8 month) BETWEEN first_order_month AND last_order_month THEN customer_id END) month8,
    COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 9 month) BETWEEN first_order_month AND last_order_month THEN customer_id END) month9,
    COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 10 month) BETWEEN first_order_month AND last_order_month THEN customer_id END) month10,
    COUNT(DISTINCT CASE WHEN DATE_ADD(first_order_month, INTERVAL 11 month) BETWEEN first_order_month AND last_order_month THEN customer_id END) month11
  FROM step1
  GROUP BY 1
)

SELECT *
FROM step2
```
