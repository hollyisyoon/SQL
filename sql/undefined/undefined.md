# 고급쿼리 문제풀이

* 활용할 데이터셋은 다음과 같습니다.

```googlesql
bigquery-public-data.london_bicycles.cycle_hire
```



* 각 대여소에서 가장 대여시간이 긴 3개의 행을 가져오기

````sql
```googlesql
WITH longest_trips AS (
  SELECT start_station_id, duration, RANK() OVER(PARTITION BY start_station_id ORDER BY duration DESC) AS nth_longest
  FROM bigquery-public-data.london_bicycles.cycle_hire
)

SELECT start_station_id, ARRAY_AGG(duration ORDER BY nth_longest LIMIT 3) AS durations
FROM longest_trips
GROUP BY start_station_id
LIMIT 5
```
````

![](../../.gitbook/assets/image.png)



* 날짜별로 직전 3개월과 이후 1달의 duration값의 평균 구하기

{% embed url="https://dataonair.or.kr/db-tech-reference/d-lounge/expert-column/?mod=document&uid=52209" %}

````sql
```googlesql

WITH temp AS (
  SELECT 
    DATE(start_date) AS date,
    FORMAT_DATE('%Y-%m', start_date) AS ym,
    AVG(duration) AS duration
  FROM bigquery-public-data.london_bicycles.cycle_hire
  GROUP BY 1,2
  ORDER BY 1
)

SELECT date, ROUND(AVG(duration) OVER (ORDER BY date ROWS BETWEEN 3 PRECEDING AND 1 FOLLOWING),2) avg_duration
FROM temp
ORDER BY 1;

```
````

* 대여 건수가 가장 많은 20%의 자전거 대여 스테이션과 가장 적은 20%의 자전거 대여 스테이션 **ntile()**

````sql
```googlesql
SELECT start_station_id, cnt
FROM (
  SELECT start_station_id, COUNT(*) cnt, NTILE(5) OVER(ORDER BY COUNT(*) DESC) rnk
  FROM bigquery-public-data.london_bicycles.cycle_hire
  GROUP BY 1 
  ORDER BY 3 ) temp
WHERE rnk = 1 OR rnk = 5
ORDER BY cnt DESC
```
````

```sql
WITH 대여_스테이션별_대여건수 AS (
  SELECT start_station_id, COUNT(*) AS 대여_건수
  FROM bigquery-public-data.london_bicycles.cycle_hire
  WHERE EXTRACT(YEAR FROM start_date) = 2019
  GROUP BY start_station_id
)

SELECT start_station_id, 대여_건수
FROM 대여_스테이션별_대여건수
WHERE NTILE(5) OVER(ORDER BY 대여_건수 DESC) = 1 -- 가장 많은 20%의 스테이션

UNION ALL

SELECT start_station_id, 대여_건수
FROM 대여_스테이션별_대여건수
WHERE NTILE(5) OVER(ORDER BY 대여_건수 ASC) = 1 -- 가장 적은 20%의 스테이션

```
