---
layout: single
title: 코딩 테스트 - 02. Implementation (Python3)
categories: cote
tag: [python, algorithm, coding_test]
toc: true 
author_profile: false
sidebar:
    nav: "docs"
---



<br/>

# Implementation (구현)

- 머릿속에 있는 알고리즘을 소스코드로 바꾸는 과정

- 모든 범위의 코딩 테스트 문제 유형을 포함하는 개념

  <br/>


# 문제

## 1. 상하좌우
- N x N 정사각형 공간에 있는 A가 (1,1)에서 출발하여 최종 도착 지점의 좌표를 출력하는 프로그램을 작성하시오
    - L, R, U, D의 문자가 반복적으로 적혀있다 (A의 이동 계획서)
        - L : 왼쪽 한 칸
        - R : 오른쪽 한 칸
        - U : 위로 한 칸
        - D : 아래로 한 칸
    - N x N 공간을 벗어나는 움직임은 무시된다
    - 연산 횟수는 이동 횟수에 비례 => O(N)


```python
# 공간 크기
n = int(input())
```

    5



```python
# 시작 좌표
x, y = 1, 1
```


```python
move_list = input().split()
```

    R R R U D D



```python
move_types = ['L', 'R', 'U', 'D']

# x좌표는 좌우로 움직일 때, y좌표는 상하로 움직일 때 값이 변함
dx = [0, 0, -1, 1]
dy = [-1, 1, 0, 0]
```


```python
# 이동 계획서 확인
for m in move_list:
    for i in range(len(move_types)):
        if m == move_types[i]:
            nx = x + dx[i]
            ny = x + dy[i]
            
    if nx < 1 or ny < 1 or nx > n or ny > n:
        continue
            
    x, y = nx, ny

print(nx, ny)
```

    3 2


## 2.시각
- 00시 00분 00초부터 N시 59분 59초까지 모든 시각 중에서 3이 하나라도 포함되는 모든 경우의 수를 구하는 프로그램 작성
- 완전 탐색 : 비효율적인 시간 복잡도로 인하여 데이터가 큰 경우에 정상적으로 동작하지 않을 수 있음 => 100만개 이하일 때 사용
    - 하루는 86400초 => 경우의 수가 작음 => 완젙 탐색 가능


```python
n = int(input())
```

    5



```python
cnt = 0

for i in range(n + 1):
    for j in range(60):
        for k in range(60):
            if '3' in str(i) + str(j) + str(k):
                cnt += 1
                
print(cnt)
```

    11475


## 3. 왕실의 나이트
- 8 x 8 좌표 평면의 정원에서 나이트 이동 가능 경우의 수 구하기
    - 수평 2 + 수평 1
    - 수직 2 + 수평 1
- 행 위치 : 1 ~ 8
- 열 위치 : a ~ h
- 정원 밖으로는 나갈 수 없다


```python
input_data = input()
```

    a1



```python
# col값을 숫자로 변환 (a : 1, b : 2, ...)
col = ord(input_data[0]) - ord('a') + 1# chr <-> ord
```


```python
row = int(input_data[1])
```


```python
col
```




    1




```python
steps = [(-2, -1), (-2, 1), (-1, -2), (-1, 2), (1, -2), (1, 2), (2, -1), (2, 1)]
```


```python
type(steps[0])
```




    tuple




```python
result = 0

for i in steps:
        nr = row + i[0]
        nc = col + i[1]
        
        # 이동 가능한 위치면 카운트 증가
        # 좌표는 (1, 1) ~ (8, 8)까지 밖에 없음
        # 따라서 이동하고싶은 좌표가 이 사이에 있는 경우에만 result 추가
        if nr >= 1 and nr <= 8 and nc >= 1 and nc <= 8:
            result += 1
            
print(result)
```

    2


## 4. 게임 개발
- 첫째 줄 : 맵 생성 (n, m)
- 둘째 줄 : 캐릭터 현재 좌표 (A, B) (항상 육지)
- 둘째 줄 : 바라보는 방향 (d)
    - 0 : 북, 1 : 동, 2 : 남, 3 : 서
- 셋째 줄 : 육지(0), 바다(1) 인지 북 -> 남, 서 -> 동 순서로 주어짐
    - 맵의 외곽은 항상 바다

###  Tip
1. 방향을 지정하는 문제 유형에선 dx, dy랑느 별도의 리스트를 만들어 방향을 정하는 것이 효과적
2. 예외처리는 고려하지 않고 빠르게 코드를 작성하자


```python
# 맵 생성
n, m = map(int, input().split())
```

    4 4



```python
# 방문 위치 저장을 위해 맵을 0 으로 초기화
# data = [[0 for i in range(n)] for i in range(m)]
```


```python
data = [[0] * m for _ in range(n)]
```


```python
A, B, direction = map(int, input().split())
```

    1 1 0



```python
# 현재 좌표 방문 처리
data[x][y] = 1
```


```python
arr = []

# 세로 크기 (n) 만큼 반복 => 전체 맵 정보 받기
for i in range(n):
    arr.append(list(map(int, input().split())))
```

    1 1 1 1
    1 0 0 1
    1 1 0 1
    1 1 1 1



```python
# 북 동 남 서 방향 정리
dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]
```


```python
# 왼쪽 회전 함수
def turn_left():
    global direction
    direction -= 1
    
    # 시작이 기준에서 왼쪽방향으로 90도 회전 => 북 기준 서쪽으로 회전한다고 보면 됨 => 북 서 남 동 순서 => direction -= 1하는 이유
    if direction == -1:
        direction = 3
```


```python
# 시뮬레이션 시작
count = 1
turn_time = 0

while True:
    # 1. 현재 방향 기준 왼쪽으로 회전
    turn_left()
    
    nx = A + dx[direction]
    ny = B + dy[direction]
    
    # 2. 회전한 이후 정면에 가보지 않은 칸이 존재하는 경우
    if data[nx][ny] == 0 and arr[nx][ny] == 0:
        data[nx][ny] = 1
        A = nx
        B = ny
        count += 1
        turn_time = 0
        continue
    
    # 2-2. 회전 후 가보지 않은 칸이 없는 경우
    else:
        turn_time +=1
    
    # 3. 네 방향 모두 갈 수 없는 경우
    if turn_time == 4:
        nx = A - dx[direction]
        ny = B - dy[direction]
        
        # 뒤로 갈 수 있따면 이동
        if arr[nx][ny] == 0:
            A = nx
            B = ny
        # 뒤가 바다
        else:
            break
        turn_time = 0
        
print(count)
    
        
```

    2
    3
    4
    4



```python
# N, M을 공백을 기준으로 구분하여 입력받기
n, m = map(int, input().split())

# 방문한 위치를 저장하기 위한 맵을 생성하여 0으로 초기화
d = [[0] * m for _ in range(n)]
# 현재 캐릭터의 X 좌표, Y 좌표, 방향을 입력받기
x, y, direction = map(int, input().split())
d[x][y] = 1 # 현재 좌표 방문 처리

# 전체 맵 정보를 입력받기
array = []
for i in range(n):
    array.append(list(map(int, input().split())))

# 북, 동, 남, 서 방향 정의
dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

# 왼쪽으로 회전
def turn_left():
    global direction
    direction -= 1
    if direction == -1:
        direction = 3

# 시뮬레이션 시작
count = 1
turn_time = 0
while True:
    # 왼쪽으로 회전
    turn_left()
    nx = x + dx[direction]
    ny = y + dy[direction]
    # 회전한 이후 정면에 가보지 않은 칸이 존재하는 경우 이동
    if d[nx][ny] == 0 and array[nx][ny] == 0:
        d[nx][ny] = 1
        x = nx
        y = ny
        count += 1
        turn_time = 0
        continue
    # 회전한 이후 정면에 가보지 않은 칸이 없거나 바다인 경우
    else:
        turn_time += 1
    # 네 방향 모두 갈 수 없는 경우
    if turn_time == 4:
        nx = x - dx[direction]
        ny = y - dy[direction]
        # 뒤로 갈 수 있다면 이동하기
        if array[nx][ny] == 0:
            x = nx
            y = ny
        # 뒤가 바다로 막혀있는 경우
        else:
            break
        turn_time = 0

# 정답 출력
print(count)
```

    4 4
    1 1 0
    1 1 1 1
    1 0 0 1
    1 1 0 1
    1 1 1 1
    3

