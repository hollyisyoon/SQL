---
description: DB에서 Index란 무엇인가!
---

# DB Index

## Index가 중요한 이유

* full scan보다 더 빨리 찾도록 도와줌
* 시간 복잡도는 O(logN) (B-tree based index)
* 조건을 만족하는 튜플을 빠르게 조회하기 위해
* 빠르게 정렬하거나 그룹핑 하기 위해

## Index 거는 법

```sql
SELECT * FROM player WHERE name = 'Sonny';
SELECT * FROM player WHERE team_id = 105 and backnumber = 7;

CREATE INDEX player_name_idx ON player(name); //중복 허용
CREATE UNIQUE INDEX team_id_backnumber_idx ON player(team_id, backnumber); //중복 비허용
```

* 혹은 CREATE TABLE 하면서 만드는 법

Index 동작방식
