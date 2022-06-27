---
layout: single
title: 백준 2740 - 행렬 곱셈 (Python3)
categories: baekjoon
tag: [python, algorithm, coding_test]
toc: true 
author_profile: false
sidebar:
    nav: "docs"
---

## 코드

```python
n, m = map(int, input().split())

A = []
B = []

for _ in range(n):
    A.append(list(map(int, input().split())))
    
m, k = map(int, input().split())

for _ in range(m):
    B.append(list(map(int, input().split())))
    
# 최종 출력할 행렬을 미리 만들어둠 (0으로 초기화)
result = [[0 for _ in range(k)] for _ in range(n)]

for i in range(n):
    for j in range(k):
        for x in range(m):
            # 곱을 result[i][j]에 더해줌
            result[i][j] += A[i][x] * B[x][j]
            
for row in result:
    for m in row:
        print(m, end= ' ')
    print()                
```



## 문제 해설

행렬의 곱을 코드로 표현하는 문제.

행렬 곱셈의 성질을 알아야만 풀 수 있는 문제이다. 

---

행렬 곱셈의 성질

- (N x M)크기의 행렬 A와 (M x K)크기의 행렬 B가 주어졌을 때, 두 행렬의 곱 X는 (N x K)의 크기를 가짐
- A의 행과 B의 열의 크기가 같아야 곱셈 가능 

---

학창 시절에 배우는 매우 기초적인 내용의 수학이라 어려움은 따로 없었지만, 이를 코드화 시키는 것은 전혀 다른 차원의 문제였다. 최종적으로 출력할 행렬을 미리 0으로 초기화하여 만들어준 후, 아래의 3중 for문을 활용하여 최종 출력 행렬을 갱신해준다.

```python
for i in range(n):
    for j in range(k):
        for x in range(m):
            # 곱을 result[i][j]에 더해줌
            result[i][j] += A[i][x] * B[x][j]
```

