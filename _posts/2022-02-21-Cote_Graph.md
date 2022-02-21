---
layout: single
title: 코딩 테스트 - 08. 기타 그래프 알고리즘 (Python3)
categories: cote
tag: [python, algorithm, coding_test]
toc: true 
author_profile: false
sidebar:
    nav: "docs"
---



<br/>

# 기타 그래프 알고리즘

- 노드와 노드 사이에 연결된 간선의 정보를 가지고 있는 자료구조
- 알고리즘 문제를 접했을 때 **'서로 다른 객체(혹은 객체)가 연결되어 있다'**는 이야기를 들으면 그래프 알고리즘을 떠올려라

## 00-1. 인접 행렬 vs 인접 리스트
- 노드 개수 V, 간선 개수 E

||**인접 형렬**|**인접 리스트**|
|:----:|:----:|:----:
**방식**|2차원 배열|리스트
**공간 복잡도**|O(v**2)|O(E)
**시간 복잡도**|O(1)|O(V)
**알고리즘**|플로이드 워셜|다익스트라





## 00-2. 서로소 집합 자료구조
- 서로소 집합
    - 공통 원소가 없는 두 집합
- 서로소 부분 집합들로 나누어진 원소들의 데이터를 처리하기 위한 자료구조
- union(합집합)과 find(찾기) 연산으로 구성 (**union-find** 자료구조라고도 불림)
- 트리 자료구조를 이용하여 표현
    1. union 연산을 확인하여 서로 연결된 두 노드 A, B를 확인
        1. A와 B의 루트 노드 A', B'를 각각 찾음
        2. A'를 B'의 부모 노드로 설정(B'가 A'를 가리키도록 한다) (더 작은 원소가 부모 노드)
    2. 모든 union 연산을 처리할 때까지 1번 과정 반복
- union 연산들은 **그래프 형태로 표현 가능** (**실제로 원소의 집합 정보를 표현할 땐 트리 사용**)
    - 효과적은 수행을 위해 **부모 테이블**을 항상 가지고 있어야 함
    - 루트 노드를 즉시 계산할 수 없고, 부모 테이블을 계속 확인하며 거슬러 올라가야 함

### 기본적인 서로소 집합 알고리즘 소스 코드
- find 함수가 비효율적으로 동작 (V: 노드의 개수, M: union 연산의 개수일 때 => O(VM))
- 경로 압축을 하여 효율적이게 변경 (해당 노드의 루트 노드를 바로 부모 노드로 설정)
    ```python
    return find_parent(parent, parent[x]) => parent[x] = find_parent(parent, parent[x])
    ```


```python
# 노드의 개수와 간선의 개수 입력받기
v, e = map(int, input().split())

# 부모 테이블 초기화
parent = [0] * (v+1)

# 부모 테이블상에서, 부모를 자기 자신으로 초기화
for i in range(1, v+1):
    parent[i] = i

# 특정 원소가 속한 집합을 찾기 (루트 노드 찾기)
def find_parent(parent, x):
    
    # 루트 노드가 아니라면, 루트 노드를 찾을 때까지 재귀적으로 호출
    if parent[x] != x:
        
        # 경로 압축 부분
        # return find_parent(parent, parent[x])
        parent[x] = find_parent(parent, parent[x])
    return x

# 두 원소가 속한 집합을 합치기
def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    
    # 더 작은 수가 부모 노드가 됨
    if a < b:
        parent[b] = a
    else:
        parent[a] = b
    
# union 연산을 각각 수행
for i in range(e):
    a, b = map(int, input().split())
    union_parent(parent, a, b)
    
# 각 원소가 속한 집합 출력
print('각 원소가 속한 집합 : ', end='')
for i in range(1, v+1):
    print(find_parent(parent, i ), end=' ')
    
print()

# 부모 테이블 내용 출력
print('부모 테이블 : ', end='')
for i in range(1, v+1):
    print(parent[i], end=' ')
```

    6 4
    1 4
    2 3
    2 4
    5 6
    각 원소가 속한 집합 : 1 1 1 1 5 5 
    부모 테이블 : 1 1 2 1 5 5 

### 서로소 집합을 활용한 사이클 판별
- 간선을 하나씩 확인하면서 두 노드가 포함되어 있는 집합을 합치는 과정을 반복하여 사이클 판별
- 과정
    1. 각 간선을 확인하며 두 노드의 루트 노드를 확인
        1. 루트 노드가 서로 다르다면 두 노드에 대하여 union 연산 수행
        2. 루트 노드가 서로 같다면 사이클이 발생한 것
    2. 그래프에 포함되어 있는 모든 간선에 대하여 1번 과정을 반복
- 방향성이 없는 무향 그래프에서만 적용 가능


```python
# 특정 원소가 속한 집합 찾기
def find_parent(parent, x):
    
    # 루트 노드가 아니면, 루트 노드를 찾을 때까지 재귀적으로 호출
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]

# 두 원소가 속한 집합 합치기
def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    
    if a < b:
        parent[b] = a
    else:
        parent[a] = b
    
# 노드의 개수와 간선의 개수 입력받기
v, e = map(int, input().split())
           
# 부모 테이블 초기화
parent = [0] * (v+1)
           
# 부모 테이블상에서 부모를 자기 자신으로 초기화
for i in range(1, v+1):
    parent[i] = i
    
    
# 사이클 발생 여부
cycle = False

for i in range(e):
    a, b = map(int, input().split())
    
    # 사이클이 발생한 경우 종료
    if find_parent(parent, a) == find_parent(parent, b):
        cycle = True
        break
    # 사이클이 발생하지 않았다면 union 수행
    else:
        union_parent(parent, a, b)

if cycle:
    print("사이클이 발생했습니다")
else:
    print("사이클이 발생하지 않았습니다")
```

    3 3
    1 2
    1 3
    2 3
    사이클이 발생했습니다


## 01. 신장 트리
- 하나의 그래프가 있을 때 모든 노드를 포함하면서 사이클이 존재하지 않는 부분 그래프
- **모든 노드가 포함되어 서로 연결되면서 사이클이 존재하지 않는다** => 트리의 성립 조건
- **최소 신장 트리**
    - 신장 트리 중 최소 비용으로 만들 수 있는 신장 트리
    - 최종 간선의 개수 == 노드의 개수 - 1

### 01-1. 크루스칼 알고리즘
- 대표적인 최소 신장 트리 알고리즘
- 가장 적은 비용으로 모든 노드를 연결할 수 있음 (그리디 알고리즘으로 분류)
- 모든 간선에 대하여 정렬을 수행한 뒤, **가장 거리가 짧은 간선부터** 집합에 포함 (사이클 발생 간선은 제외)
    1. 간선 데이터를 비용에 따라 오름차순으로 정렬
    2. 간선을 하나씩 확인하며 현재의 간선이 사이클을 발생시키는지 확인
        - 사이클이 발생하지 않는 경우에만 최소 신장 트리에 포함
    3. 모든 간선에 대하여 2번 과정을 반복
- 시간 복잡도 : O(ElogE)


```python
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    
    if a < b:
        parent[b] = a
    else:
        parent[a] = b
        
v, e = map(int, input().split())
parent = [0] * (v+1)

# 모든 간선을 담을 리스트
edges = []

# 최종 비용을 담을 변수
result = 0

for i in range(1, v+1):
    parent[i] = i
    
for _ in range(e):
    a, b, cost = map(int, input().split())
    
    # 비용순으로 정렬하기 위해서 튜플의 첫 번째 원소를 비용으로 설정
    edges.append((cost, a, b))
    
# 간선을 비용순으로 정렬
edges.sort()

# 간선을 하나씩 확인
for edge in edges:
    cost, a, b = edge
    
    # 사이클이 발생하지 않는 경우에만 집합에 포함
    if find_parent(parent,a ) != find_parent(parent, b):
        union_parent(parent, a, b)
        result += cost
        
print(result)
```

    7 9
    1 2 29
    1 5 75
    2 3 35
    2 6 34
    3 4 7
    4 6 23
    4 7 13
    5 6 53
    6 7 25
    159


## 02. 위상 정렬
- 순서가 정해져 있는 일련의 작업을 차례대로 수행해야 할 때 사용할 수 있는 알고리즘
- 방향 그래프의 모든 노드를 **방향성에 거스리지 않도록 순서대로 나열**하는 정렬
- 그래프상에서 전후 관계가 있다면, 위상 정렬을 수행하여 모든 선후 관계를 지키는 전체 순서를 계산할 수 있음
- 답안이 여러 가지로 나올 수 있음
- 진입차수
    - 특정한 노드로 **들어오는** 간선의 개수
- 과정
    1. 진입차수가 0인 노드를 큐에 넣음
    2. 큐가 빌 때까지 다음의 과정을 반복
        1. 큐에서 원소를 꺼내 노드에서 출발하는 간선을 그래프에서 제거
        2. 새롭게 진입차수가 0이 된 노드를 큐에 넣음
        3. 이때, 모든 원소를 방문하기 전에 큐가 빈다 => 사이클이 발생
- 시간 복잡도 : O(V+E)


```python
from collections import deque

v, e = map(int, input().split())

# 모든 노드에 대한 진입차수 0으로 초기화
indegree = [0] * (v+1)

# 각 노드에 연결된 간선 정보를 담기 위한 연결 리스트(그래프) 초기화
graph = [[] for i in range(v+1)]

# 방향 그래프의 모든 간선 정보를 입력받기
for _ in range(e):
    a, b = map(int, input().split())
    
    # 정점 A에서 B로 이동 가능
    graph[a].append(b)
    
    # 진입차수를 1 증가
    indegree[b] += 1
    
# 위상 정렬 함수
def topology_sort():
    
    # 알고리즘 수행 결과를 담을 리스트
    result = []
    
    q = deque()
    
    # 처음 시작할 때는 진입차수가 0인 노드를 큐에 삽입
    for i in range(1, v+1):
        if indegree[i] == 0:
            q.append(i)
            
    while q:
        now = q.popleft()
        
        # 알고리즘 수행 결과 리스트에 노드 추가
        result.append(now)
        
        # 해당 원소와 연결된 노드들의 진입차수에서 1 빼기
        for i in graph[now]:
            indegree[i] -= 1
            
            # 새롭게 진입차수가 0이 되는 노드를 큐에 삽입
            if indegree[i] == 0:
                q.append(i)
                
    for i in result:
        print(i, end = ' ')
        
topology_sort()
```

    7 8
    1 2
    1 5
    2 3
    2 6
    3 4
    4 7
    5 6
    6 4
    1 2 5 3 6 4 7 

<br/>

# 문제

## 01. 팀 결성
- 학생들에게 0번 ~ N번까지의 번호 부여
- 처음에는 모두 다 다른 팀
- 팀 합치기 연산과 팁 여부 확인 연산
    - 팀 합치기 : 두 팀을 합침 (0, a, b)
    - 팀 여부 확인 : 같은 팀에 속하는지 확인 (1, a, b)
- 전형적인 서로소 집합 알고리즘 문제


```python
n, m = map(int, input().split())
parent = [0] * (n + 1)

def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    
    if a < b:
        parent[b] = a
    else:
        parent[a] = b
        
for i in range(0, n+1):
    parent[i] = i
    
for i in range(m):
    oper, a, b = map(int, input().split())
    
    if oper==0:
        union_parent(parent, a, b)
    elif oper==1:
        if find_parent(parent, a) == find_parent(parent, b):
            print('YES')
        else:
            print('NO')
```

    7 8
    0 1 3
    1 1 7
    NO
    0 7 6
    1 7 1
    NO
    0 3 7
    0 4 2
    0 1 1
    1 1 1
    YES


## 02. 도시 분할 계획
- 마을을 2개의 분리된 마을로 만들자
- 분리된 마을 안에 집들이 서로 연결되야 함
- 마을에는 집이 하나 이상있어야 함
- **전체 그래프에서 2개의 최소 신장 트리를 만들어야하는 문제**
    1. 크루스칼 알고리즘으로 최소 신장 트리를 찾음
    2. 구성 간선 중 가장 비용이 큰 간선을 제거  
        => 최소한의 비용으로 2개의 신장트리 생성 가능


```python
def find_parent(parent, x):
    if parent[x] != x:
        parent[x] = find_parent(parent, parent[x])
    return parent[x]

def union_parent(parent, a, b):
    a = find_parent(parent, a)
    b = find_parent(parent, b)
    
    if a < b:
        parent[b] = a
    else:
        parent[a] = b
        
v, e = map(int, input().split())
parent = [0] * (v+1)

edges = []
result = 0

for i in range(1, v+1):
    parent[i] = i
    
for _ in range(e):
    a, b, cost = map(int, input().split())
    
    edges.append((cost, a, b))
    
edges.sort()

# 최소 신장 트리에 포함되는 간선 중에 가장 비용이 큰 간선을 담을 변수
last = 0

for edge in edges:
    cost, a, b = edge
    
    # 사이클이 발생하지 않는 경우에만 집합에 포함
    if find_parent(parent, a) != find_parent(parent, b):
        union_parent(parent, a, b)
        result += cost
        
        # 정렬했으므로 맨 마지막에 들어오는 cost값이 가장 비용이 큰 값
        last = cost
        
print(result - last)
```

    7 12
    1 2 3
    1 3 2
    3 2 1
    2 5 2
    3 4 4
    7 3 6
    5 1 5
    1 6 2
    6 4 1
    6 5 3
    4 5 3
    6 7 4
    8


## 03. 커리큘럼
- 강의를 들을 때, 선수 강의를 수강해야만 들을 수 있는 강의가 존재
    - 선수 강의가 없는 강의의 경우 동시에 여러 개의 강의 수강 가능
- V개의 강의를 들을 때, 모든 강의를 수강하기까지 걸리는 최소 시간을 구하라
- 위상 정렬 알고리즘의 응용


```python
from collections import deque

# 리스트에 단순 대입 연산을 할 경우 값이 변경될 수 있음
# 따라서 값을 복제해서 사용하기 위해 deepcopy() 함수를 사용
import copy

# 노드의 개수 입력
v = int(input())

# 모든 노드에 대한 진입하수 0으로 초기화
indegree = [0] * (v+1)

# 각 노드에 연결된 간선 정보를 담기 위한 연결 리스트(그래프) 초기화
graph = [[] for _ in range(v+1)]

# 각 강의 시간을 0으로 초기화
time = [0] * (v+1)

# 방향 그래프의 모든 간선 정보 입력
for i in range(1, v+1):
    data = list(map(int, input().split()))
    
    # 첫 번째 요소 : 시간 정보
    time[i] = data[0]
    
    for x in data[1: -1]:
        indegree[i] += 1
        graph[x].append(i)
        
# 위상 정렬 함수
def topology_sort():
    
    # 알고리즘 수행 결과를 담을 리스트
    result = copy.deepcopy(time)
    
    q = deque()
    
    # 처음 시작할 때는 진입차수가 0인 노드를 큐에 삽입
    for i in range(1, v+1):
        if indegree[i] == 0:
            q.append(i)
            
    while q:
        now = q.popleft()
        
        # 해당 원소(now)와 연결된 노드들의 진입차수에서 1 빼기
        for i in graph[now]:
            result[i] = max(result[i], result[now] + time[i])
            indegree[i] -= 1
            
            if indegree[i] == 0:
                q.append(i)
                
    for i in range(1, v+1):
        print(result[i])
        
        
topology_sort()        

```

    5
    10 -1
    10 1 -1
    4 1 -1
    4 3 1 -1
    3 3 -1
    10
    20
    14
    18
    17

