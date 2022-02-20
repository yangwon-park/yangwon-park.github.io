---
layout: single
title: 코딩 테스트 - 07. 최단 경로 (Python3)
categories: cote
tag: [python, algorithm, coding_test]
toc: true 
author_profile: false
sidebar:
    nav: "docs"
---

<br/>

# 최단 경로

- 말 그대로 가장 짧은 경로를 찾는 알고리즘 (**길 찾기** 문제)
- 다양한 사례에 맞는 알고리즘이 존재하므로 그것을 미리 파악하면 좀 더 쉽게 문제를 풀 수 있음
- 보통 그래프를 이용하여 표현하며, 최단 경로 모두를 출력하기보다 단순히 최단 거리를 출력하도록 요구하는 문제가 많이 출제됨
- **다익스트라**, **플로이드 워셜**, **벨만 포드** 알고리즘이 대표적
- 그리디 알고리즘과 다이나믹 프로그래밍의 한 유형이라고 볼 수 있음
- 문제를 풀기 전에 그래프를 그림으로 한 번 그려보고 접근하자

## 0. 힙 자료구조
- 우선순위 큐를 구현하기 위하여 사용하는 자료구조 중 하나
- 우선순위 큐 : 우선순위가 가장 높은 데이터를 가장 먼저 삭제
- Python에서 우선순위 큐가 필요할 때, PriorityQueue 혹은 **heapq**를 사용 (heapq가 더 빠름)
- 우선순위 큐 라이브러리에 데이터 묶음을 넣으면, 첫 번째 원소를 기준으로 우선순위를 부여
    - ex) (가치, 물건) => '가치' 값이 우선순위 값
- 우선순위 큐를 구현할 때는 내부적으로 최소 힙 혹은 최대 힙을 이용
    - 최소 힙 : 값이 작은 데이터 먼저 삭제 (Python, Java)
    - 최대 힙 : 값이 큰 데이터 먼저 삭제 (C++)
    - 우선순위에 해당하는 값에 음수를 붙여 반대로 사용할 수 있음

## 1. 다익스트라 최단경로 알고리즘
- 그래프에 여러 개의 노드가 있을 때, 특정한 노드에서 출발하여 다른 노드로 가는 각각의 최단 경로를 구해주는 알고리즘
- **음의 간선**이 없을 때 정상적으로 동작
- 실제 GPS 소프트웨어의 기본 알고리즘으로 채택
- 그리디 알고리즘으로 분류되며, **매번 가장 비용이 적은 노드를 선택**해서 임의의 과정을 반복함
    1. 출발 노드 설정
    2. 최단 거리 테이블 초기화
    3. 방문하지 않은 노드 중 최단 거리가 가장 짧은 노드 선택
    4. 해당 노드를 거쳐 다른 노드로 가는 비용을 계산한 후, 최단 거리 테이블 갱신
    5. 3과 4를 반복
- 최단 거리 테이블 :  '각 노드에 대한 현재까지의 최단 거리' 정보를 가지고 있는 1차원 리스트
- 최단 거리 테이블을 계속 갱신함
    - 이 때, 알고리즘이 진행되면서 한 단계당 하나의 노드의 대한 최단거리를 확실하게 찾는다
    - 즉, **한 번 선택된 노드의 최단 거리는 더 이상 감소하지 않는다**

### 1-1. 간단한 다익스트라 알고리즘
- 매번 선형 탁샘하고, 현재 노드와 연결된 노드를 매번 확인 => 시간 복잡도 차원에서 좋지 못한 성능
- 시간 복잡도 : O(V**2) (V : 노드의 개수)


```python
import sys
# input = sys.stdin.readline
INF = int(1e9) # 무한을 의미하는 값 (10억)

# n : 노드의 개수, m : 간선의 개수
n, m = map(int, input().split())

# 시작 노드 번호
start = int(input())

# 각 노드에 연결되어 있는 노드에 대한 정보를 담는 리스트 만들기
graph = [[] for i in range(n+1)]

# 방문한 적이 있는지 체크하는 목적의 리스트
visited = [False] * (n + 1)

# 최단 거리 테이블을 모두 무한으로 초기화 (거리가 나올 때 바로 갱신 가능한 크기의 값)
distance = [INF] * (n + 1)

# 모든 간선 정보 입력
for _ in range(m):
    
    # A에서 B로 가는 비용은 C
    a, b, c = map(int, input().split())
    graph[a].append((b, c))
    
# 방문하지 않는 노드 중에서 가장 최단 거리가 짧은 노드의 번호 반환
def get_smallest_node():
    min_value = INF
    
    # 가장 최단 거리가 짧은 노드(인덱스)
    index = 0
    
    for i in range(1, n + 1):
        if distance[i] < min_value and not visited[i]:
            min_value = distance[i]
            index = i
    return index

def dijkstra(start):
    # 시작 노드에 대해서 초기화
    distance[start] = 0
    visited[start] = True
    
    for j in graph[start]:
        distance[j[0]] = j[1]
        
    # 시작 노드를 제외한 전체 n - 1개의 노드에 배해 반복
    for i in range(n - 1):
        
        # 현재 최단 거리가 가장 짧은 노들르 꺼내서 방문 처리
        now = get_smallest_node()
        visited[now] = True
        
        # 현재 노드와 연결된 다른 노드 확인
        for j in graph[now]:
            cost = distance[now] + j[1]
            
            # 현재 노드를 거쳐서 다른 노드로 이동하는 거리가 더 짧으 ㄴ경우
            if cost < distance[j[0]]:
                distance[j[0]] = cost
                
# 다익스트라 알고리즘 수행
dijkstra(start)

# 모든 노드로 가기 위한 최단 거리 출력
for i in range(1, n+1):
    
    # 도달할 수 없는 경우, 무한이라고 출력
    if distance[i] == INF:
        print("INFINITY")
    else:
        print(distance[i])
```

    6 11
    1
    1 2 2
    1 3 5
    1 4 1
    2 3 3
    2 4 2
    3 2 3
    3 6 5
    4 3 3
    4 5 1
    5 3 1
    5 6 2
    0
    2
    3
    1
    2
    4


### 1-2. 개선된 다익스트라 알고리즘
- 힙 자료구조를 사용하여 특정 노드까지의 최단 거리에 대한 정보를 힙에 담아서 처리 (최소 힙)
- 최단 거리 테이블은 1차원 리스트로 그대로 사용 + 현재 가장 가까운 노드를 저장하기 위한 목적으로 **우선순위 큐** 사용 (튜플 형식)
- 시간 복잡도 : O(ElogV) (V : 노드의 개수, E : 간선의 개수)
    - 이렇게 빠른 이유
        1. 반복문(While)은 노드의 개수 V 이상의 횟수로는 반복되지 않는다
        2. V번 반복될 때 마다 각각 자신과 연결된 간선들을 모두 확인  
            => '현재 우선순위 큐에서 꺼낸 노드와 연결된 다른 노드들을 확인'하는 총 횟수는 E만큼 연산이 수행  
            => E개의 원소를 우선순위 큐에 넣었다가 모두 빼내는 연산과 매우 유사




```python
import heapq
import sys
# input = sys.stdin.readline
INF = int(1e9) # 무한을 의미하는 값 (10억)

# n : 노드의 개수, m : 간선의 개수
n, m = map(int, input().split())

# 시작 노드 번호
start = int(input())

# 각 노드에 연결되어 있는 노드에 대한 정보를 담는 리스트 만들기
graph = [[] for i in range(n+1)]

# 방문한 적이 있는지 체크하는 목적의 리스트
visited = [False] * (n + 1)

# 최단 거리 테이블을 모두 무한으로 초기화 (거리가 나올 때 바로 갱신 가능한 크기의 값)
distance = [INF] * (n + 1)

# 모든 간선 정보 입력
for _ in range(m):
    
    # A에서 B로 가는 비용 c
    a, b, c = map(int, input().split())
    graph[a].append((b, c))
    
def dijkstra(start):
    q = []
    
    # 시작 노드로 가기 위한 최단 경로 0, 큐에 삽입
    heapq.heappush(q, (0, start))
    distance[start] = 0
    
    while q:
        # 가장 최단 거리가 짧은 노드에 대한 정보 꺼내기
        dist, now = heapq.heappop(q)
        
        # 현재 노드가 이미 처리된 적 있는 노드라면 무시
        # 불러온 노드 정보의 거리값보다 벌써 갱신된 노드 정보 거리값이 더 작다면 무시
        if distance[now] < dist:
            continue
        
        # 현재 노드와 연결된 다른 인접한 노드들 확인
        for j in graph[now]:
            cost = dist + j[1]
            
            # 현재 노드를 거쳐서, 다른 노드로 이동하는 거리가 더 짧은 경우
            if cost < distance[j[0]]:
                distance[j[0]] = cost
                heapq.heappush(q, (cost, j[0]))
            
                
# 다익스트라 알고리즘 수행
dijkstra(start)

# 모든 노드로 가기 위한 최단 거리 출력
for i in range(1, n+1):
    
    # 도달할 수 없는 경우, 무한이라고 출력
    if distance[i] == INF:
        print("INFINITY")
    else:
        print(distance[i])    
```

    6 11
    1
    1 2 2
    1 3 5
    1 4 1
    2 3 3
    2 4 2
    3 2 3
    3 6 5
    4 3 3
    4 5 1
    5 3 1
    5 6 2
    0
    2
    3
    1
    2
    4


## 2. 플로이드 워셜 알고리즘
- 모든 지점에서 다른 모든 지점까지의 최단 경로를 모두 구해야하는 경우에 사용
- 다익스트라 알고리즘과 달리 매번 방문하지 않는 노드 중에서 최단 거리를 갖는 노드를 찾을 필요가 없다
- 2차원 리스트에 최단 거리 정보를 저장 (모든 노드에 대하여 다른 모든 노드로 가는 최단 거리 정보를 담아야 하므로)
- (N번의 단계에서 O(N**2)의 시간 소요
- 다이나믹 프로그래밍으로 분류
- 각 단계에서는 해당 노드를 거쳐 가는 경우를 고려함
    - ex) 1번 노드에 대한 확인 : A => 1번 => B 비용 확인 후 최단 거리 갱신
    - 즉, A => B or A => n번 노드 => B 중 더 작은 값으로 갱신
- 전체적으로 3중 반복문을 이용
- 시간 복잡도 : O(N***3) 


```python
INF = int(1e9) # 무한을 의미하는 값 (10억)

# n : 노드의 개수, m : 간선의 개수
n, m = map(int, input().split())

# 2차원 리스트(그래프 표현)를 만들고, 모든 값을 무한으로 초기화
graph = [[INF] * (n + 1) for _ in range(n + 1)]

# 자기 자신에서 자기 자신으로 가는 비용 0으로 초기화
for a in range(1, n + 1):
    for b in range(1, n + 1):
        if a == b:
            graph[a][b] = 0
            
# 각 간선에 대한 정보를 입력받은 후, 그 값으로 초기화
for _ in range(m):
    
    # A에서 B로 가는 비용은 C
    a, b, c = map(int, input().split())
    graph[a][b] = c
    
# 점화식에 따라 플로이드 워셜 알고리즘을 수행
# K 노드를 기준으로 거쳐가는 형식으로 구현
for k in range(1, n + 1):
    for a in range(1, n + 1):
        for b in range(1, n + 1):
            graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])
            
# 수행된 결과를 출력
for a in range(1, n + 1):
    for b in range(1, n + 1):
        
        #도달할 수 없는 경우, 무한이라고 출력
        if graph[a][b] == INF:
            print("INFINITY", end = " ")
        else:
            print(graph[a][b], end = " ")
    print()
```

    4 7
    1 2 4
    1 4 6
    2 1 3
    2 3 7
    3 1 5
    3 4 4
    4 3 2
    0 4 8 6 
    3 0 7 9 
    5 9 0 4 
    7 11 2 0 

<br/>

# 문제

## 1. 미래 도시
- A의 위치(1) => K => X로 이동하는 최소 시간 구하기 **(플로이드 워셜)**
- X번 회사에 도달할 수 없다면 -1 출력
- 주어지는 순서
    - 첫째 줄 : 회사의 개수 N, 경로의 개수 M
    - 둘째 줄 ~ M + 1번째 줄 : 연결된 두 회사의 번호
    - M + 2번째 줄 : X, K


```python
INF = int(1e9)

n, m = map(int, input().split())

graph = [[INF] * (n + 1) for _ in range(n + 1)]

for a in range(1, n + 1):
    for b in range(1, n + 1):
        if a == b:           
            graph[a][b] = 0
            
for _ in range(m):
    a, b = map(int, input().split())
    
    # 문제에서 서로에게 가는 비용은 1이라고 설정
    graph[a][b] = 1
    graph[b][a] = 1
    
# K : 거쳐갈 노드, X : 최종 목적지
x, k = map(int, input().split())

for k in range(1, n + 1):
    for a in range(1, n + 1):
        for b in range(1, n + 1):
            graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])

            
# 수행 결과 (K를 거쳐야하니까 거리가 2개의 합)
distance = graph[1][k] + graph[k][x]

if distance >= INF:
    print('-1')
else:
    print(distance)
```

    4 2
    1 3
    2 4
    3 4
    -1


## 2. 전보
- 메세지를 최대한 많은 도시에 보내자
- 받은 도시의 개수, 총 걸리는 시간 계산 (다익스트라 알고리즘)
- 첫째 줄 : 도시의 개수 N, 통로의 개수 M, 보내고자 하는 도시 C
    - N, M의 범위 충분히 큼 => 우선순위 큐 사용
- 둘째 줄 ~ M + 1번째 줄 : 통로에 대한 정보 (X, Y, Z)


```python
import heapq
import sys

# input = sys.stdin.readline()
INF = int(1e9)

n, m, start = map(int, input().split())

graph = [[] for _ in range(n+1)]

distance = [INF] * (n + 1)

# 모든 간선의 정보 입력 받기
for _ in range(m):
    
    # x -> y, 비용 z
    x, y, z = map(int, input().split())
    graph[x].append((y, z))
    
def dijkstra(start):
    q = []
    
    heapq.heappush(q, (0, start))
    distance[start] = 0
    
    while q:
        dist, now = heapq.heappop(q)
        
        if distance[now] < dist:
            continue
            
        for i in graph[now]:
            cost = dist + i[1]
            
            if cost < distance[i[0]]:
                distance[i[0]] = cost
                heapq.heappush(q, (cost, i[0]))
            
dijkstra(start)

# 도달할 수 있는 노드의 개수
count = 0

# 도달할 수 있는 노드 중, 가장 멀리 있는 노드와의 최단 거리
max_distance = 0

for d in distance:
    if d != INF:
        count += 1
        max_distance = max(max_distance, d)
        
# 시작 노드는 제외해야함
print(count - 1, max_distance)
```

    3 2 1
    1 2 4
    1 3 2
    2 4

