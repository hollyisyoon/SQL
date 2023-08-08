---
description: 머릿속의 알고리즘을 정확하고 빠르게 작성하기
---

# 구현

## 구현 유형

* 풀이를 떠올리는 것은 쉽지만 소스코드로 옮기기 어려운 문제 유형
* 완전 탐색 : 모든 경우의 수를 다 계산하는 해결 방법
* 시뮬레이션 : 문제에서 제시하는 알고리즘을 한 단계 씩 차례대로 직접 수행해야하는 문제 유형
* 파이썬, 리스트의 길이에 따라 메모리 사용량 주의 (리스트 길이 1000 -> 메모리 약 4kb)



## 예제 4-1. 상하좌우

```python
n = int(input())
x, y = 1,1
plans = input().split()
dx = [0,0,-1,1]
dy = [-1,1,0,0]
move_types = ['L', 'R', 'U', 'D']

for plan in plans :
    for i in range(len(move_types)):
        if plan == move_types[i]:
            nx = x + dx[i]
            ny = y + dy[i]
        if nx<1 or ny<1 or nx>n or ny>n:
            continue
        x, y = nx, ny
print(x,y)
```

