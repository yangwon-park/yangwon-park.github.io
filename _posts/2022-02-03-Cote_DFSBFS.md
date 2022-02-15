---
layout: single
title: 코딩 테스트 - 03. DFS & BFS (Python3)
categories: cote
tag: [python, algorithm, coding_test]
toc: true 
author_profile: false
sidebar:
    nav: "docs"
---

<br/>

# 인접 행렬 vs 인접 리스트

- 인접 행렬
    - 모든 관계를 저장하므로 노드 개수가 많을수록 메모리가 불필요하게 낭비됨
    - 특정 두 노드의 연결 상태를 빠르게 확인 가능
- 인접 리스트
    - 연결된 정보만을 저장하기 떄문에 메모리를 효율적으로 사용
    - s특정한 두 노드가 연결되어 있는지에 대한 정보를 얻는 속도가 느림 (하나 하나씩 확인해야 하기 떄문)
        - 특정 노드와 연결된 모든 노드를 순회하는 경우 더 유용




```python
# 인접 행렬 (Adjacency Matrix)
INF = 999999999

graph = [
    [0, 7, 5],
    [7, 0 , INF],
    5, INF, 0
]
```


```python
print(graph)
```

    [[0, 7, 5], [7, 0, 999999999], 5, 999999999, 0]



```python
# 인접 리스트 (Adjacency List)
# 행이 3개인 2차원 리스트로 인접 리스트 표현

graph = [[] for _ in range(3)]

# 노드 0에 연결된 노드 정보 저장 (노드, 거리)
graph[0].append((1, 7))
graph[0].append((2, 5))

# 노드 1에 연결된 노드 정보 저장 (노드, 거리)
graph[1].append((0, 7))

# 노드 2에 연결된 노드 정보 저장 (노드, 거리)
graph[2].append((0, 5))

print(graph)
```

    [[(1, 7), (2, 5)], [(0, 7)], [(0, 5)]]

<br/>

# DFS (깊이 우선 탐색)

- **스택** 활용
- 최대한 멀리 있는 노드를 우선 탐색
- 순서
    1. 탐색 시작 노드를 스택에 삽입하고 방문 처리를 한다
    2. 스택의 최상단 노드에 방문하지 않은 인접 노드가 있으면 그 인접 노드를 스택에 넣고 방문 처리  
       (방문하지 않은 인접 노드가 없으면 스택에서 최상단 노드를 꺼낸다)
    3. 2번 과정을 더 이상 수행할 수 없을 때까지 반복
- O(N)의 시간 복잡도
- 재귀 함수 사용 시, 매우 간결하게 구현 가능


```python
# DFS 메소드 정의
def dfs(graph, v, visited):
    # 현재 노드를 방문 처리
    visited[v] = True
    print(v, end=' ')
    #현재 노드와 연결된 다른 노드를 재귀적으로 방문
    for i in graph[v]:
        if not visited[i]:
            dfs(graph, i, visited)

# 각 노드가 연결된 정보를 리스트 자료형으로 표현
graph = [
    [],
    [2, 3, 8],
    [1, 7],
    [1, 4, 5],
    [3, 5],
    [3, 4],
    [7],
    [2, 6, 8],
    [1, ]
]

# 각 노드가 방문된 정보를 리스트 자료형으로 표현
visited = [False] * len(graph)

# 정의된 DFS 함수 호출
dfs(graph, 1, visited)
```

    1 2 7 6 8 3 4 5 


```python
print(visited)
```

    [False, True, True, True, True, True, True, True, True]

<br/>

# BFS (너비 우선 탐색)

- **큐** 활용
- 가까운 노드부터 탐색하는 알고리즘
- 순서
    1. 탐색 시작 노드를 큐에 삽입하고 방문 처리
    2. 큐에서 노드를 꺼내 해당 노드의 인접 노드 중에서 방문하지 않은 노드를 모두 큐에 삽입하고 방문 처리
    3. 2번의 과정을 더 이상 수행할 수 없을 때까지 반복
- O(N)의 시간 복잡도
- 일반적인 경우 수행 시간이 DFS보다 좋은 편


```python
from collections import deque

# BFS 메소드 정의
def bfs(graph, start, visited):
    # 큐(Queue) 구현을 위해 deque 라이브러리 사용
    queue = deque([start])
    # 현재 노드 방문 처리
    visited[start] = True
    # 큐가 빌 때까지 반복
    while queue:
        #큐에서 하나의 원소를 뽑아 출력
        v = queue.popleft()
        print(v, end=' ')
        # 해당 원소와 연결된, 아직 방문하지 않은 원소들을 큐에 삽입
        for i in graph[v]:
            if not visited[i]:
                queue.append(i)
                visited[i] = True
                
# 각 노드가 연결된 정보를 리스트 자료형으로 표현
graph = [
    [],
    [2, 3, 8],
    [1, 7],
    [1, 4, 5],
    [3, 5],
    [3, 4],
    [7],
    [2, 6, 8],
    [1, ]
]

# 각 노드가 방문된 정보를 리스트 자료형으로 표현
visited = [False] * len(graph)

# 정의된 DFS 함수 호출
bfs(graph, 1, visited)
```

    1 2 3 8 7 4 5 6 

<br/>

# 문제

## 1. 노드, 관선 연결 정보를 입력받아 DFS & BFS로 탐색


```python
from collections import deque
# 슈퍼 기본 DFS & BFS
n, m, v = map(int, input().split())

# 리스트의 첫번째 요소는 빈 리스트 처리 => 인덱스 번호와 노드 번호를 통일시키기 위해서
# 따라서 n + 1 => 정점 개수 +1 만큼 반복해야함
graph = [[] for _ in range(n + 1)]

# 각 노드마다 방문 여부 체크할 리스트 선언
visited = [False] * (n + 1)

# 입력받은 노드와 간선간의 연결 정보를 노드별 인접리스로 표현
for _ in range(m):
    
    # 간선이 연결하는 두 정점의 번호 입력
    a, b = map(int, input().split())
    
    # a와 b 노드가 연결돼어있다 = b와 a 노드가 연결돼어있다 (swap)
    graph[a].append(b)
    graph[b].append(a)
    
    # 번확 작은 순서로 방문하기 때문에 sort()로 오름차순 정렬 시행
    graph[a].sort()
    graph[b].sort()

# DFS 
def dfs(graph, v, visited):
    # 시작 방문 처리 & 재귀적 호출 시 새로 받은 v번째 노드 방문처리
    visited[v] = True
            
    # 방문한 노드의 번호 출력
    print(v, end = ' ')
    
    # 처음 시작 노드의 번호 v부터 v에 연결되어있는 노드들을 반복하여 탐색 시작
    for i in graph[v]:
        
        # v번 노드의 방문 여부가 False이면
        if not visited[i]:
            
            # 재귀적으로 호출 => 스택 구현과 동일한 기능
            dfs(graph, i, visited)
# BFS      
def bfs(graph, v, visited):
    # 방문 여부 리스트 초기화
    visited = [False] * (n + 1)
    
    # 방문 노드를 담을 queue 선언
    queue = deque()
    
    # 시작 노드 방문
    queue.append(v)
    
    # 시작 방문 처리 & 재귀적 호출 시 새로 받은 v번째 노드 방문처리
    visited[v] = True
    
    # 방문을 완료한 원소들을 queue에 넣고 queue에서 하나씩 순서대로 뽑아내면
    # queue가 빌 것이고 그럴 경우 반복문 종료
    while queue:
        # 큐에서 LILO로 원소 출력
        p = queue.popleft()
        print(p, end=' ')
        
        # 해당 요소와 연결된 미방문 노드들 큐에 삽입 후 방문처리
        for i in graph[p]:
            if not visited[i]:
                queue.append(i)
                visited[i] = True
                
dfs(graph, v, visited)
print()
bfs(graph, v, visited)
```

    4 5 1
    1 2
    1 3
    1 4
    2 4
    3 4
    1 2 4 3 
    1 2 3 4 

## 2. 음료수 얼려 먹기


```python
n, m = map(int, input().split())

# 맵 정보 입력 받기
graph = []
for i in range(n):
    graph.append(list(map(int, input())))
    
# DFS로 특정한 노드를 방문한 뒤에 연결된 모든 노드들도 방문
def dfs(x, y):
    # 주어진 범위를 벗어나는 경우에는 즉시 종료
    if x <= -1 or x >= n or y <= -1 or y >= m:
        return False
    # 현재 노드를 아직 방문하지 않았다면
    if graph[x][y] == 0:
        # 해당 노드 방문 처리
        graph[x][y] = 1
        # 상, 하, 좌, 우 모두 재귀적으로 호출
        dfs(x - 1, y)
        dfs(x, y - 1)
        dfs(x + 1, y)
        dfs(x, y + 1)
        return True
    return False

# 모든 노드(위치)에 대하여 음료수 채우기
result = 0
for i in range(n):
    for j in range(m):
        # 현재 위치에서 DFS 수행
        if dfs(i, j) == True:
            result += 1
            
print(result)
```

    3 5
    00001
    11111
    00001
    2


## 3. 미로 탈출


```python
# BFS => 시작 지점에서 가까운 노드부터 차례대로 그래프의 모든 노드 탐색
from collections import deque

n, m = map(int, input().split())

# 맵 정보 입력
graph = []
for i in range(n):
    graph.append(list(map(int, input())))
    
    # 이동할 방향 정의 (상 하 좌 우)
    dx = [-1, 1, 0, 0]
    dy = [0, 0, -1, 1]
    
    # BFS 소스 코드
    def bfs(x, y):
        # 큐 구현
        queue = deque()
        queue.append((x, y))
        # 큐가 빌 때까지 반복
        while queue:
            x, y = queue.popleft()
            # 현재 위치에서 네 방향으로의 위치 확인
            for i in range(4):
                nx = x + dx[i]
                ny = y + dy[i]
                # 미로 찾기 공간을 벗어난 경우 무시
                if nx < 0 or ny < 0 or nx >= n or ny >= m:
                    continue
                # 벽인 경우 무시
                if graph[nx][ny] == 0:
                    continue
                # 해당 노드를 처음 방문하는 경우에만 최단 거리 기록
                if graph[nx][ny] == 1:
                    graph[nx][ny] = graph[x][y] + 1
                    queue.append((nx, ny))
                    
        #가장 오른쪽 아래까지의 최단 거리 반환
        return graph[n-1][m-1]
    
# BFS 수행 결과
print(bfs(0, 0))
```

    5 6
    101010
    111111
    111111
    000001
    000011
    10

