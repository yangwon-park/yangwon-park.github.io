---
layout: single
title: 코딩 테스트 - 06. 동적 프로그래밍 (Python3)
categories: cote
tag: [python, algorithm, coding_test, DP]
toc: true 
author_profile: false
sidebar:
    nav: "docs"
---



<br/>

# 동적 프로그래밍 (Dynamic Programming)

- 특정 문제를 메모리 공간을 좀 더 사용하여 연산 속도를 비약적으로 증가시킬 수 있는 방법
- 큰 문제를 작게 나누고, 같은 문제라면 한 번씩만 풀어 문제를 효율적으로 해결하는 알고리즘 기법
- 즉, <span style="color:red">문제를 푸는 과정에서 이전 결과를 다음 문제에 적용하기 위해 별도의 저장용 리스트를 생성한 후, 거기에 결과를 저장하여 활용</span>
- ***[나무 위키 참고](https://namu.wiki/w/%EB%8F%99%EC%A0%81%20%EA%B3%84%ED%9A%8D%EB%B2%95)***
- 종종 결과를 특정한 매우 큰 수로 나눈 값을 출력하라는 내용이 포함됨
- 선행 조건
    1. 큰 문제를 작은 문제로 나눌 수 있다
    2. 작은 문제에서 구한 정답은 그것을 포함하는 큰 문제에서도 동일하다
- 방식
    1. Top-Down
        - 재귀 함수를 이용하여 작성하는 경우
        - 큰 문제를 해결하기 위해 작은 문제를 호출
        - 스택 크기가 한정되어 있을 수도 있음 (sys 라이브러리의 setrecursionlimit() 함수 이용해서 완화)
    2. Bottom-up (권장)
        - 단순히 반복문을 이용하여 소스코드를 작성하는 경우
        - 작은 문제부터 차근 차근 답을 도출    

## 피보나치 수열
- 동적 프로그래밍의 대표적인 예시

### 재귀 함수로 구현한 피보나치 수열
- n이 커질수록 수행 시간이 기하급수적으로 늘어남
- 시간 복잡도 : O(2**N)


```python
def fibo(x):
    if x==1 or x==2:
        return 1
    return fibo(x-1) + fibo(x-2)

print(fibo(6))
```

    8


### 메모이제이션(탑 다운) 기법을 사용한 피보나치 수열
- 한 번 구한 결과를 메모리 공간에 메모해두고 같은 식을 다시 호출하면 메모한 결과를 가져오는 기법
- **캐싱**이라고도 한다
- 한 번 구한 정보를 리스트에 저장
- 시간 복잡도 : O(N)

#### 탑 다운 방식


```python
# 한 번 계산된 결과를 메모이제이션하기 위한 리스트 초기화
d = [0] * 100

# 피보나치 함수를 재귀함수로 구현
def fibo(x):
    if x == 1 or x == 2:
        return 1
    
    # 이미 계산한 적 있는 문제라면 그대로 반환
    if d[x] != 0:
        return d[x]
    
    # 아직 계산하지 않은 문제라면 점화식에 따라서 피보나치 결과 반환
    d[x] = fibo(x-1) + fibo(x-2)
    return d[x]

print(fibo(99))
```

    218922995834555169026



```python
d[5] == (d[4] + d[3])
```




    True



#### 탑 다운 - 호출 되는 함수 확인


```python
d = [0] * 100

def fibo(x):
    print('f(' + str(x) + ')', end = ' ')
    if x == 1 or x == 2:
        return 1
    if d[x] != 0:
        return d[x]
    
    d[x] = fibo(x - 1) + fibo(x-2)
    
    return d[x]

fibo(6)
```

    f(6) f(5) f(4) f(3) f(2) f(1) f(2) f(3) f(4) 




    8



### 바텀 업 방식
- 단순히 반복문을 이용


```python
d = [0] * 100 # 결과 저장용 리스트 (DP 테이블)

d[1] = 1
d[2] = 1
n = 99

for i in range(3, n+1):
    d[i] = d[i - 1] + d[i - 2]
    
print(d[n])    
```

    218922995834555169026


# 문제

## 1. 1로 만들기
- 정수 x를 4가지 연산을 통해 1을 만들 때, 연산을 사용하는 최소 횟수를 구해라
- 모든 경우에 +1 이라는 숫자의 의미 
  - DP 테이블에 최종적으로 연산 횟수를 저장
  - 즉, 연산이 시행됐으면 1회만큼 늘려줘야하니까 +1을 더 함


```python
# 정수 x가 주어짐
x = int(input())

# DP 테이블 생성
# 연산 횟수를 담음
# x의 범위 (1 ~ 30000)
d = [0] * 30001

# 바텀업
for i in range(2, x+1):    
    # 현재의 수에서 1을 빼는 경우
    # 2, 3, 5 모두 나누어 떨어지지 않으면 남은건 -1    
    # 이 식으로 d[i]값이 정해지더라도 i가 2, 3, 5 중에 나누어 지는 수가 있다면 해당 조건문에서 d[i]가 덮어써진다 
    # 바로 직전의 수 +1 => 현재 수 연산 횟수
    d[i] = d[i - 1] + 1
    
    # 위에서 +1을 한 채로 d[i]가 내려왔으니 밑에 조건문에서는 d[i]에는 +1 해주지 않는다
    # (바로 직전의 수까지의 연산 횟수 +1)과 (i로 나누었을 때의 연산 횟수) +1 중 더 작은 값으로 횟수 설정
    if i % 2 == 0:
        d[i] = min(d[i], d[i // 2] + 1)
        
    # 현재의 수가 3으로 나누어 떨어지는 경우
    if i % 3 == 0:
        d[i] = min(d[i], d[i // 3] + 1)
        
    # 현재의 수가 5로 나누어 떨어지는 경우
    if i % 5 == 0:
        
        d[i] = min(d[i], d[i // 5] + 1)
        
        
print(d[x])
```

    8
    3


## 2. 개미 전사
- 일직선상에 존재하는 식량창고 약탈
- 서로 인접한 식략창고는 약탈하지 않고, 최소 한 칸 이상 떨어진 식량창고를 약탈
- 이 때, 약탈할 수 있는 식량의 최대값을 구하라


```python
# 식량창고 개수
n = int(input())
store = list(map(int, input().split()))

# DP 테이블 생성 
# 약탈할 수 있는 식량의 양을 담음
# n의 범위 (1 ~ 100)
d = [0] * 101

d[0] = store[0]
d[1] = max(store[0], store[1])

for i in range(2, n):
    d[i] = max(d[i-1], d[i-2] + store[i])

# n-1이 마지막 식량창고 인덱스
print(d[n-1])
```

    9
    1 2 3 4 5 6 7 8 1
    20


## 3. 바닥 공사
- 가로가 n, 세로가 2인 바닥
- 1x2, 2x1, 2x2의 덮개를 이용해 채울 때, 가능한 모든 경우의 수를 구하라
- 출력 조건 : 경우의 수 % 796796 (결괏값이 굉장히 커질 수도 있기 때문에)


```python
n = int(input())

# DP 테이블 생성
# n의 범위 (1 ~ 1000)
d = [0] * 1001

d[1] = 1
d[2] = 3

for i in range(3, n + 1):
    d[i] = (d[i - 1] + 2 * d[i - 2]) % 796796
    
print(d[n])
```

    3
    5


## 4. 효율적인 화폐 구성
- n가지 종류의 화폐
- 개수를 최소한으로 이용하여 그 가치의 합이 m원이 되도록 선택
- 중복 사용 가능
- 구성이 같지만 순서만 다르면 같은 경우로 구분


```python
# n : 화폐 종류 개수, m : 얻고자 하는 금액
n, m = map(int, input().split())

# 화폐의 종류(가치)를 담을 리스트
money = [int(input()) for _ in range(n)]

# DP 테이블 생성
# 사용된 화폐의 개수를 담음
# m의 범위(1 ~ 10000)

# 0이 아니라 10001인 이유       =>  m의 범위가 10000까지이기 때문에 만들 수 없는 금액이 나오려면 10000보다 커야함
# 리스트 길이가 (m + 1)인 이유  =>  1원부터 원하는 화폐 금액까지의 모든 화폐 단위에 식을 적용하기 위해서
d = [10001] * (m + 1)

# 0원을 만들 수 있는 방법 => 0
d[0] = 0

for i in range(n):
    
    # money[i] => 가지고 있는 화폐의 종류 (i가 곧 화폐 액수와 동일)
    # 
    for j in range(money[i], m +1):
        
        # m원을 만드는 방법이 존재하는 경우
        if d[j - money[i] != 10001]:
            d[j] = min(d[j], d[j - money[i]] + 1)
            
if d[m] == 10001:
    print(-1)
else:
    print(d[m])
```

    2 6
    2
    3
    2

