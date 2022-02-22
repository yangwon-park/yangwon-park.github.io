---
layout: single
title: 코딩 테스트 - 01. Greedy (Python3)
categories: cote
tag: [python, algorithm, coding_test, greedy]
toc: true 
author_profile: false
sidebar:
    nav: "docs"
---

<br/>

# Greedy 알고리즘

- 탐욕법
- 단순하지만 강력한 문제 해결 방법
- 현재 상황에서 지금 당장 좋은 것만 고르는 방법
- 사전에 외우고 있지 않아도 풀 수 잇을 가능성이 높은 문제 유형
- **가장 큰 순서대로** 혹은 **가장 작은 순서대로**와 같은 기준을 알게 모르게 제시해줌
    - 이 기준은 정렬 알고리즘을 사용했을 때 만족 가능 => 자주 정렬 알고리즘과 짝을 이루어 출제됨
- 문제 풀이를 위한 최소한의 아이디어를 떠올리고 이것이 **정당한지 검토**할 수 있어야 답을 도출할 수 있음
- 문제 유형 파악이 어렵다면 그리디 알고리즘 => 다이나믹 프로그래밍 or 그래프 알고리즘 등으로 고민

<br/>

<br/>

# 문제

## 01. 거스름돈 문제
- 거슬러줘야 할 동전의 최소 개수 구하기 => 금액이 큰 동전부터 차례대로 거슬러 주면 최소 개수!!!
- 가지고 있는 동준 중 큰 단위가 항상 작은 단위의 배수 => 작은 단위의 동전들을 종합해 다른 해가 나올 수 없다 => 정당성 확인



```python
n = int((input("n : ")))
```

    n : 5600



```python
print(n)
```

    5600



```python
count = 0

# 화폐의 종류만큼 반복 수행 => 시간 복잡도 : O(K)
# 일단 화폐를 내림차순으로 정렬해야함
coin = [500, 100, 50, 10]

for c in coin:
    count += n // c
    n %= c

print(count)
```

    0


## 02. 큰 수의 법칙
- 가장 큰 수를 K번 더하고 두 번째로 큰 수를 한 번 더하는 연산 반복

### 02-1 단순 답안


```python
# n, m, k를 공백으로 구분하여 입력받음
# n : 배열의 크기, m : 숫자가 더해지는 횟수, K : 해당 수가 연속으로 더해질 수 있는 최대 횟수
n, m, k = map(int, input().split())

# N개의 수를 공백으로 구분하여 입력받음
data = list(map(int, input().split()))

# 최종 결과를 저장할 변수
result = 0

data.sort()
first = data[n-1]  # 정렬했으므로 가장 큰 수
second = data[n-2] # 정렬했으므로 두 번째로 큰 수

while True:
    for i in range(k):
        if m == 0:
            break  # for문 탈출
        result += first
        m -= 1     # 횟수를 1회씩 줄여야 무한루프 탈출
    if m == 0:
        break      # while문 탈출
    result += second
    m -= 1         # 횟수를 1회씩 줄여야 무한루프 탈출
        
print(result)

```

    5 7 3
    2 4 5 4 6
    41


### 02-2. 모범 답안
- 위의 답안 경우, M의 범위가 매우 커지면 시간 초과 판정이 될 것


```python
n, m, k = map(int, input().split())
data = list(map(int, input().split()))

data.sort()
first = data[n-1]
second = data[n-2]

count = ((m // (k + 1)) * k) + (m % (k + 1))

result = 0
result += (count) * first
result += (m - count) * second

print(result)
```

    5 8 3
    2 4 5 4 6
    46


## 03. 숫자 카드 게임
- N x M 형태의 카드 중 뽑고자 하는 카드가 있는 행 선택 후, 그 행에서 가장 작은 숫자를 출력
    - 각 행 기준으로 가장 낮은 숫자들 중에서 가장 높은 숫자를 뽑는 게임


```python
n, m = map(int, input().split())
```

    2 4



```python
data = [[0 for i in range(n)] for i in range(m)]
```


```python
data
```




    [[0, 0], [0, 0], [0, 0], [0, 0]]




```python
result = 0

for i in range(n):
    # 안에서 돌려야 행마다 가장 작은 값을 뽑아냄
    data = list(map(int, input().split()))
    
    min_value = min(data)
    
    result = max(result, min_value)
    
    print(result)
```

    3 3 3 4
    3
    7 3 1 8
    3



```python
# list 전체에서 가장 작은 값 => min(map(min, data))

# result = 0

# 행 기준으로 뽑으니까 n(행)만큼만 반복
# for i in range(n):
#     min_value = min(map(min, data))
```


```python
print(result)
```

    3


## 04. 1이 될 때까지
- N이 1이 될 때 까지
    1. N에서 1을 뺀다
    2. N을 K로 나눈다 (단, 나누어떨어질 때만 선택 가능)


```python
n, k = map(int, input().split())
```

    3 2



```python
cnt = 0

while True:
    if n == 1:
        break
    
    if n % k == 0:
        n = n // k
        cnt += 1
    else:
        n -= 1
        cnt += 1
```


```python
print(cnt)
```

    2



```python
result = 0

while True:
    target = (n // k ) * k
    result += (n - target)
    n = target
    
    if n < k:
        break
    
    result += 1
    n //= k

result += (n-1)
print(result)
```

    3
