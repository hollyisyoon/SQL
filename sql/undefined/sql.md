# SQL 사용자 정의 함수

* 임시로 함수를 제작하여 선언한 쿼리 내에서만 사용

```sql
CREATE TEMPORARY FUNCTION dayOfWeek(x TIMESTAMP) AS (
    ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']
    [ORDINAL(EXTRACT(DAYOFWEEK FROM x))]
);

CREATE TEMPORARY FUNCTION getDate(x TIMESTAMP) AS (
    EXTRACT(DATE FROM x)
);

WITH overnight_trips AS (
    SELECT duration, dayOfWeek(start_date) AS start_day
    FROM 'bigquery-public-data'.london_bicycles.cycle_hire
    WHERE getDate(start_date) != getDate(end_date)
)

SELECT 
    start_day, 
    COUNT(*) AS num_overnight_rentals,
    AVG(duration)/3600 AS avg_duration_hours
FROM 
    overnight_trips
GROUP BY 
    start_day
ORDER BY
    num_overnight_rentals DESC

```



* 데이터셋에 함수를 저장하고, 쿼리에서 참조하는 방법

```sql
CREATE OR REPLACE FUNCTION ch08eu.dayOfWeek(x TIMESTAMP) AS 
(
    ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']
        [ORDINAL(EXTRACT(DAYOFWEEK FROM x)))]
);
```



* 공개 UDF를 사용

{% embed url="https://github.com/GoogleCloudPlatform/bigquery-utils" %}

```sql
SELECT 
    start_station_name,
    COUNT(*) AS num_trips,
    bqutil.fn.median(ARRAY_AGG(tripduration)) AS typical_duration
FROM
    'bigquery-public-data'.new_york_citibike.citibike_trips
GROUP BY start_station_name
HAVING num_trips > 1000
ORDER BY typical_duration DESC
LIMIT 5
```



'bigquery-public-data'.london\_bicycles.cycle\_hire
