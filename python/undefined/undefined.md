---
description: 제 1장,, 가장 근본 알고리쥼
---

# 그리디

## 그리디 알고리즘

* 현재 상황에서 지금 당장 좋은 것만 선택하는 방법
* 그리디 알고리즘 유형의 문제는 창의력, 문제를 풀기 위한 최소한의 아이디어를 떠올릴 수 있는 능력이 중요



## 1. 거스름돈 문제

* 문제 : 카운터에 거스름돈으로 사용할 500, 100, 50, 10원 동전이 무한히 존재한다. 손님에게 거슬러줘야할 N원일 때 거슬러줘야할 동전의 최소 개수를 구해라. 거슬러줘야할 돈 N은 10의 배수이다.
* 가지고 있는 동전 중에서 큰 단위가 항상 작은 단위의 배수라서 작은 단위의 동전들을 종합해 다른 해가 나올 수 없다.
* 대부분의 그리디 알고리즘 문제는 문제풀이를 위한 최소한의 아이디어를 떠올리고 이것이 정당한지 검토할 수 있어야 한다.

{% code lineNumbers="true" %}
````python
```notebook-python
n=1260
count=0

coin_types = [500, 100, 50, 10]
for coin in coin_types:
    count += n//coin
    n %= coin

print(count)
```
````
{% endcode %}



## 2. 큰 수 문제

{% code lineNumbers="true" %}
````python
```notebook-python
#- 혼자 풀어본 것 -#
n, m, k = map(int, input().split())
data = list(map(int, input().split()))

data.sort() # 오름차순
first = data[n-1] 
second = data[n-2]

range = m//(k+1)
rest = m%(k+1)
range * (k*first + second) + rest*first
```
````
{% endcode %}

* 답안

<pre class="language-python" data-line-numbers><code class="lang-python"><strong>n, m , k = map(int, input().split())
</strong><strong>data = list(map(int, input().split()))
</strong><strong>
</strong><strong>data.sort()
</strong><strong>first = data[n-1]
</strong><strong>second = data[n-2]
</strong><strong>
</strong><strong>count = int(m/(k+1))*k
</strong><strong>count += m%(k+1)
</strong><strong>
</strong><strong>result = 0
</strong><strong>result += (count)*first
</strong><strong>result += (m-count)*second
</strong><strong>
</strong><strong>print(result)
</strong></code></pre>



## 3. 숫자 카드 게임

```python
n, m = map(int, input().split())

result = 0
for i in range(n):
    data = list(map(int, input().split()))
    min_value = min(data)
    result = max(result, min_value)

print(result)    
```



## 4. 1이 될 때까지

```python
n, k = map(int, input().split())
result = 0
while n > 1 :
  if n%k != 0 : 
      n = n-1
  else :
      n = n//k
  result += 1

print(result)
```



* 오 나누어 떨어지는 수가 될 때까지 1 빼는 횟수를 먼저 고려하면 `N`이 매우 큰 경우에도 더 빠르게 작아질 수 있으며, 나누기 연산은 뺄셈보다 훨씬 빠르기 때문에 효율적임

```python
n, k = map(int, input().split())
result = 0

while True:
    # N==K로 나누어떨어지는 수가 될때까지 1 빼기
    target = (n//k)*k
    result += (n-target)
    n = target
    # N이 K보다 작을 때 반복문 탈출
    if n<k:
        break
    result += 1
    n //= k
result += (n-1)
print(result)
```

