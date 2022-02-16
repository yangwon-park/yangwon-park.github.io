---
layout: single
title: 코딩 테스트 - 05. Binary Search (Python3)
categories: cote
tag: [python, algorithm, coding_test]
toc: true 
author_profile: false
sidebar:
    nav: "docs"
---

<br/>

# 순차 탐색

- 가장 기본적인 탐색 방법
- 리스트 안에 있는 특정한 데이터를 찾기 위해 앞에서부터 데이터를 하나씩 차례대로 확인하는 방법
- 시간만 충분하다면 항상 원하는 원소를 찾을 수 있다 (정렬 상태에 상관없이 사용 가능)
- 순차 탐색 사용
    - 리스트에서 특정한 값을 가지는 원소의 개수를 세는 count() 메소드
    - 리스트에서 특정 값의 원소가 있는지 체크할 때
- 시간 복잡도 : O(N) (최악의 경우)


```python
def sequential_search(n, target, array):
    for i in range(n):
        if array[i] == target:
            return i + 1
        
print("생성할 원소 개수를 입력한 다음 한 칸 띄우고 찾을 문자열을 입력하세요,")
input_data = input().split()
n = int(input_data[0])    # 원소의 개수
target = input_data[1]    # 찾고자 하는 문자열

print("앞서 적은 원소 개수만큼 문자열을 입력하세요. 구분은 띄어쓰기 한 칸으로 합니다")
array = input().split()

print(sequential_search(n, target, array))
```

    생성할 원소 개수를 입력한 다음 한 칸 띄우고 찾을 문자열을 입력하세요,
    5 Hey
    앞서 적은 원소 개수만큼 문자열을 입력하세요. 구분은 띄어쓰기 한 칸으로 합니다
    Hey how are you
    1

<br/>

# 이진 탐색

- 반으로 쪼개면서 탐색하기
- 데이터가 **정렬되어 있어야만** 사용 가능
- 위치를 나타내는 변수 3개 (시작점, 끝점, 중간점)을 사용
- 찾으려는 데이터와 중간점 위치에 있는 데이터를 반복적으로 비교해서 원하는 데이터를 찾음
- 절반씩 데이털르 줄어들도록 만든다는 점에서 퀵 정렬과 비슷
- 재귀 함수 or 반복문을 이용한다
- 시간 복잡도 : O(logN)

## 재귀 함수를 이용한 이진 탐색


```python
def binary_search(array, target, start, end):
    if start > end:
        # 시작점과 끝점이 교차되면 종료
        return None
    # 중간점 생성
    mid = (start + end) // 2
    
    # 중간점 값 == 타겟 => 중간점 인덱스 반환
    if array[mid] == target:
        return mid
    # 중간점 값 > 타겟 => 왼쪽 확인
    # 중간점이 끝점이 됨
    elif array[mid] > target:
        return binary_search(array, target, start, mid - 1)
    else:
        return binary_search(array, target, mid + 1, end)
    
n, target = list(map(int, input().split()))

array = list(map(int, input().split()))

# 이진 탐색 결과 출력
result = binary_search(array, target, 0, n-1)

if result == None:
    print("원소가 존재하지 않습니다")
else:
    print(result + 1)
```

    10 7
    1 3 5 7 9 11 13 15 17 19
    4


## 반복문으로 구현한 이진 탐색


```python
def binary_search(array, target, start, end):
    # 시작점과 끝점이 교차하면 종료
    while start <= end:
        mid = (start+ end) // 2
        
        # 중간값과 타겟이 일치하면 탐색 완료
        if array[mid] == target:
            return mid
        # 중간점 값 > 타겟 => 왼쪽 확인
        # 중간점 앞의 점이 끝점이 됨
        elif array[mid] >  target:
            end = mid - 1
        else:
        # 중간점 뒤의 점이 시작점이 됨
            start = mid + 1

    return None

n, target = list(map(int, input().split()))
array = list(map(int, input().split()))

result = binary_search(array, target, 0, n-1)

if result == None:
    print("원소가 존재하지 않습니다")
else:
    print(result+1)
```

    10 7
    1 3 5 7 9 11 13 15 17 19
    4

<br/>

# 코딩 테스트에서의 이진 탐색

- 단순하다고 느낄 수 있지만 참고할 소스코드가 없는 상태에서 구현하는 건 생각보다 어렵다
- 코딩 테스트에서 단골로 나오는 문제이므로 가급적으로 외우자
- 탐색 범위가 2000만을 넘어가면 이진 탐색 문제로 접근해보자
- 처리해야 할 데이터의 개수나 값이 1000만 단위 이상으로 넘어가면 이진 탐색과 같이 O(logN)의 속도를 내야하는 알고리즘을 사용하자

<br/>

# 트리 자료구조

- 노드와 노드의 연결로 표현하며 그래프 자료구조의 일종
- DB 시스템이나 파일 시스템과 같은 곳에서 많은 양의 데이터를 관리하기 위한 목적으로 사용
- 주요 특징
    1. 부모 노드와 자식 노드의 관계로 표현
    2. 최상단 노드 : 루트 노드
    3. 최하단 노드 : 단말 노드
    4. 트리에서 일부를 때어내도 트리 구조이며 이를 서브 트리라고 한다
    5. 계층적이고 정렬된 데이터를 다루기에 적합하다

<br/>

# 이진 탐색 트리

- 트리 자료구조 중 가장 간단한 형태
- 주요 특징    
    1. 부모 노드보다 왼쪽 자식 노드의 값이 작다
    2. 부목 노드보다 오른쪽 자식 노드의 값이 크다

<br/>

# 빠르게 입력받기

- 이진 탐색 문제는 입력 데이터가 많거나, 탐색 범위가 넓은 편
- 이 경우, input() 함수는 동작 속도가 느리므로 sys 라이브러이의 readline() 함수를 이용하자


```python
import sys

# 개행 문자까 함께 입력되므로 rstrip()을 사용
input_d = sys.stdin.readline().rstrip()

print(input_d)
```


​    <br/>

# 문제

## 1. 부품 찾기
- 가게 안에 부품이 모두 있는지 확인하는 프로그램 작성

### 1-1. 내 풀이
- 탐색 대상이 되는 parts를 list로 구현했는데 문제 요구사항 + 성능 면에서 set으로 바꿔주는게 더 좋다


```python
n = int(input())
parts = list(map(int, input().split()))
    
m = int(input())
wants = list(map(int, input().split()))

for i in wants:
    if i in parts:
        print('yes', end = ' ')
    else:
        print('no', end = ' ')
```

    5
    8 3 7 9 2
    3
    5 7 9
    no yes yes 

### 1-2. 이진 탐색 풀이
- 부품의 개수가 많은 경우 시간 복잡도를 줄이기 위해 이진 탐색 사용


```python
# 이진 탐색 소스 코드 구현 (반복문)
def binary_search(array, target, start, end):
    while start <= end:
        mid = (start + end) // 2
        
        if array[mid] == target:
            return mid
        elif array[mid] > target:
            end = mid - 1
        else:
            start = mid + 1
    return None

# 부품 리스트
n = int(input())
parts = list(map(int, input().split()))

# 손님이 찾는 부품 리스트
m = int(input())
wants = list(map(int, input().split()))

# 이진 탐색을 수행하기 위해 정렬 실행
parts.sort()

for i in wants:
    # 해당 부품이 존재하는지 확인
    result = binary_search(parts, i, 0, n-1)
    if result != None:
        print('yes', end = ' ')
    else:
        print('no', end = ' ')
```

    5
    8 3 7 9 2
    3
    5 7 9
    no yes yes 

### 1-3. 계수 정렬 풀이
1. 모든 원소의 번호를 포함할 수 있는 크기의 리스트를 만듬 
2. 리스트의 인덱스에 직접 접근하여 특정한 번호의 부품이 매장에 존재하는지 확인


```python
# 부품 리스트
n = int(input())

# 모든 원소의 번호 포함 가능한 리스트
array = [0] * 10001

# 가게에 있는 전체 부품 번호를 입력받아서 기록
for i in input().split():
    array[int(i)] = 1
    

# 손님이 찾는 부품 리스트
m = int(input())
wants = list(map(int, input().split()))

    
for i in wants:
    if array[i] == 1:
        print('yes', end = ' ')
    else:
        print('no', end = ' ')
```

    5
    8 3 7 9 2
    3
    5 7 9
    no yes yes 

### 1-4. 집합 자료형 (Set) 이용 풀이
- 단순히 특정한 수가 한 번이라도 등장했는지를 검사할 때 set을 효과적이게 사용할 수 있다
- 많은 양의 데이터를 in으로 탐색할 때 list보다 set이 성능이 더 좋다


```python
n = int(input())
parts = set(map(int, input().split()))
    
m = int(input())
wants = list(map(int, input().split()))

for i in wants:
    if i in parts:
        print('yes', end = ' ')
    else:
        print('no', end = ' ')
```

    5
    8 3 7 9 2
    3
    5 7 9
    no yes yes 

## 2. 떡볶이 떡 만들기
- 절단기 높이 H에 맞춰 적어도 손님이 요구한 떡의 총 길이 M만큼의 떡을 제공하기 위해 설정할 수 있는 H의 최댓값을 구하는 프로그램
- 전형적인 이진 탐색 문제이자 파라메트릭 서치 문제
- 적절한 높이를 찾을 때까지 절단기 높이 H를 반복해 조정
- H의 범위는 1 ~ 10억까지의 정수 중 하나 => 당연하다는 듯이 이진 탐색을 먼저 떠올려야 한다

### 파라메트릭 서치
- 최적화 문제를 결정 문제(yes or no)로 바꾸어 해결하는 기법
- 원하는 조건을 만족하는 가장 알맞은 값을 찾는 문제에 주로 사용
- 범위 내 조건을 만족하는 가장 큰 값을 찾으라는 최적화 문제인 경우, 이진 탐색으로 범위를 좁혀나갈 수 있음
- 이 유형의 경우, 이진 탐색을 재귀적으로 구현하지 않고 반복문을 이용해 구현해야 더 간결


```python
# 떡의 갯수 : n, 요청한 떡의 길이 m
n, m = map(int, input().split())

list_ = list(map(int, input().split()))

# 이진 탐색을 위한 시작점과 끝점 설정
# 당연히 절단기의 높이는 0 ~ 가장 긴 떡의 길이 안에 있어야 한다
start = 0
end = max(list_)

result = 0
while(start <= end):
    total = 0
    # 중간값 => 계속 해서 최적화 시켜줄 H에 해당
    mid = (start + end) // 2
    
    for x in list_:
        # 잘랐을 때 떡의 양 계산
        if x > mid:
            total += x - mid
    
    # 떡의 양이 부족한 경우 더 많이 자르기 (왼쪽 부분 탐색)
    if total < m:
        end = mid - 1
    # 떡의 양이 충분한 경우 덜 자르기(오른쪽 부분 탐색)
    else:
        result = mid # 최대한 덜 잘랐을 때가 정답
        start = mid + 1
        
print(result)
```

    4 6
    19 10 17 15
    15

