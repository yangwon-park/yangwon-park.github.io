---
layout: single
title: 백준 1939 - 중량 제한 (Python3)
categories: baekjoon
tag: [python, algorithm, coding_test]
toc: true 
author_profile: false
sidebar:
    nav: "docs"
---

## 코드

```python
from collections import deque
n, m = map(int, input().split())

graph = [[] for _ in range(n+1)]

for _ in range(m):
    a, b, c = map(int, input().split())
    graph[a].append((b, c))
    graph[b].append((a, c))
    
for i in range(1, n+1):
    graph[i].sort(reverse=True)

# 섬 2개 주어짐
# 편의상 start, end로 명명
start, end = map(int, input().split())


# bfs 구현
# bfs는 항상 start에서 시작한다
# 어차피 출발은 start에서만 하므로
# 굳이 따로 처리해줄 필요가 없다
# ==> 즉, 우리가 찾고자 하는 바는 중량 제한을 통과하는 경우
# ==> c에 대하여 이진 탐색을 수행할 뿐
# ==> bfs로는 목표한 중량으로 start => end까지 갈 수 있는지만 확인
def bfs(target):
    
    
    # 해당 섬에 방문하였는지 체크할 리스트
    visited = [False for _ in range(n+1)]
    
    # start점 방문
    visited[start] = True
    q = deque()
    q.append(start)
    
    while q:
        now = q.popleft()
        
        # 시작점이 끝점과 동일하면 True 반환
        # 즉, start -> end까지 배송에 성공함
        if now == end:
            return True
        
        # 현재 섬과 연결돼있는 섬에 대한 정보를 탐색
        for nx, nc in graph[now]:
            
            # 방문한 적 없는 섬이고
            # 목표한 하중(target == mid)보다 크거나 같다
            # => 방문할 수 있다는 의미
            if visited[nx] == False and target <= nc:
                
                # 큐에 넣어줌
                q.append(nx)
                # 방문할 것이므로 True로 변경
                visited[nx] = True
    # now == end가 못 됐으니 False 반환           
    return False

# 이진 탐색 구현
def binary_search():
    result = 0 

    # 문제에 중량 제한(c)의 최솟값, 최댓값이 명시돼있음
    # _min = left,
    # _max = right
    _min, _max = 1, 1000000000
    
    # 중량 제한을 이진 탐색으로 찾음
    while _min <= _max:

        # mid (target) 설정
        mid = (_min + _max) // 2

        # mid에 대한 bfs의 결과가 참이면
        if bfs(mid):
            result = mid
            _min = mid + 1
        else:
            _max = mid - 1
            
    return result
print(binary_search())
```



## 문제 해설

이진 탐색과 BFS을 함께 사용하여 푸는 문제.

문제를 풀 수 있는 알고리즘에 대한 힌트가 많이 주어진 것을 봤을 때, 다양한 풀이 방법이 존재하는 것 같은 문제이다. 난 그 중, BFS와 이진 탐색을 활용한 방법을 선택하였다. (이 방법이 가장 먼저 떠오르기도 했고, 구글링 과정 중 이 방법이 다익스트라 알고리즘에 비하여 속도가 더 빠른 것을 확인했음)

두 알고리즘을 활용하여 풀어야 하는 문제이다보니, 주어진 값들에 대하여 어떤 식으로 알고리즘을 적용해야 할지 파악하는 것이 꽤나 어려웠다.

이진 탐색으로 중량 c를 target으로 설정하고, 그 값에 대하여 BFS를 수행, 결과가 주어진 end까지 도달할 수 있다면 True를 반환하여 최종 정답 변수인 result에 target인 mid의 값을 넣어준다. 이 때, _min과 _max가 우리가 찾는 c 즉, 중량의 최댓값의 범위이므로 c의 값을 이진 탐색(파라메틱 서치)하여 찾는 것이 문제에서 원하는 정답을 찾을 수 있는 로직이다. 위의 과정을 수행하여 _min과 _max의 값을 변경하면서 수행하여 최종 정답을 구할 수 있다.

두가지 알고리즘을 한 번에 접하게 되는 문제는 처음인 것 같다. 이러한 문제를 많이 풀면 사고력 증진에 매우 도움이 될 것 같다. 끊임없이 연습하여 내 것으로 만들어야겠다.
