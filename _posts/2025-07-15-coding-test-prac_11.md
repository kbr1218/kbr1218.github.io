---
layout: post
title:  "[Programmers] 코딩테스트 연습문제 11"
author: me
categories: [ coding-test ]
date: 2025-07-15 08:00:00
image: https://programmers.co.kr/assets/img-meta-programmers-411e94bf29153dc31004168e6cd500279b1a531a23689303755e51971dee4526.png
---

#### 052. 땅따먹기 (연습문제 lv.2)
- **문제에서 원하는 것**: 마지막 행까지 내려오면서 얻을 수 있는 점수의 최댓값 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - N행 4열로 이루어진 땅따먹기 게임의 땅 `land`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 한 행씩 내려올 때, 같은 열을 연속해서 밟을 수 없음  

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 동적 프로그래밍으로 풀기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 각 행의 각 열에 대해 최대 점수를 누적해서 계산한다

    2. 현재 행의 각 열(`j`)에 대해, 이전 행의 같은 열(`j`)을 제외한 3개의 열에서 최댓값을 찾아 더하기

    3. `dp[i][j]` 는 `i`행 `j`열까지 밟았을 때의 최대 점수

```python
def solution(land):
    answer = 0
    
    for i in range(1, len(land)):
        for j in range(4):
            # 같은 열을 제외한 이전 행의 최댓값
            land[i][j] += max(land[i-1][k] for k in range(4) if k != j)

    return max(land[-1])
```

<br>

---
#### 059. 호텔 대실 (연습문제 lv.2)
- **문제에서 원하는 것**: 최소한의 객실 수를 사용해 모든 손님을 받을 때 필요한 객실의 수

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 예약 시작/종료 시간이 문자열로 주어진 2차원 배열 `book_time`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 객실은 퇴실 시간 + 10분 후에 다음 손님이 사용할 수 있음
    - 모든 시간은 "HH:MM" 형태의 24시간 기준 문자열
    - 예약은 자정을 넘지 않음
    - 시작 시간 < 종료 시간

<p style="margin-top: 2.3em;"></p>

- **접근 방법**  
    1. 시간을 전부 분 단위로 변환
    2. 예약을 시작 시간 기준으로 정렬
    3. 퇴실 + 10분 후 시간을 관리하면서 사용할 수 있는 방이 있으면 재사용 (없으면 새로운 방 추가)
    4. 필요한 방 개수 = 동시에 사용 중인 방의 최대 수

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 문자열 시간을 분 단위로 변환 ("HH:MM" >> int)

    2. 예약 리스트를 시작 시간 기준으로 정렬

    3. 최소 힙으로 방들의 퇴실 시간 + 10분을 관리

    4. 새로운 예약이 들어오면, 가장 빠른 퇴실 시간과 비교하여 방 재사용 여부 결정

    5. 힙 크기가 곧 필요한 최소 객실 수가 될듯!

```python
import heapq

def solution(book_time):
    # 문자열 HH:MM을 분 단위로 변환
    def time_to_minutes(time_str):
        h, m = map(int, time_str.split(":"))
        return h * 60 + m

    # 모든 예약 시간 분으로 변환
    times = []
    for start, end in book_time:
        start_min = time_to_minutes(start)
        end_min = time_to_minutes(end) + 10  # 퇴실 후 청소 시간 포함
        times.append((start_min, end_min))

    # 시작 시간 기준으로 정렬
    times.sort()

    # 최소 힙 (퇴실+청소 시간 저장)
    room_heap = []

    for start, end in times:
        # 기존 방 중 현재 예약과 겹치지 않는 방 제거
        if room_heap and room_heap[0] <= start:
            heapq.heappop(room_heap)
        # 현재 예약 추가
        heapq.heappush(room_heap, end)

    # 남은 방의 개수가 최소 필요한 객실 수
    return len(room_heap)
```

<br>

---
#### 060. 숫자 카드 나누기 (연습문제 lv.2)
- **문제에서 원하는 것**: **조건을 만족하는 가장 큰 양의 정수 a**를 구하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 철수의 카드 숫자 배열 `arrayA`
    - 영희의 카드 숫자 배열 `arrayB`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 두 조건 중 하나만 만족하면 됨  
        1. a는 arrayA의 모든 수를 나누고, arrayB의 어떤 수도 나누면 안 됨  
        2. 반대: a는 arrayB의 모든 수를 나누고, arrayA의 어떤 수도 나누면 안 됨   

<p style="margin-top: 2.3em;"></p>

- **접근 방법**  
     1. 각 배열의 **최대공약수(GCD)**를 구하기
     2. 그 GCD가 상대 배열을 하나라도 나누면 안 됨 → 조건 만족하는지 확인
     3. 두 조건을 각각 체크해서 가능한 값 중 큰 값 리턴

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. `gcdA = GCD(arrayA)`

    2. `gcdB = GCD(arrayB)`
        
    3. gcdA가 arrayB의 모든 값 중 하나라도 나누면 → 조건 실패  

    4. gcdB가 arrayA의 모든 값 중 하나라도 나누면 → 조건 실패  

    5. 조건을 만족하는 gcd 중 큰 값 리턴 (없으면 0)  

```python
from math import gcd
from functools import reduce

def solution(arrayA, arrayB):
    # 배열의 모든 수의 최대공약수 구하기
    def get_gcd(arr):
        return reduce(gcd, arr)

    # x가 상대 배열을 나누지 않는지 검사
    def is_valid(x, other_arr):
        for num in other_arr:
            if num % x == 0:
                return False
        return True

    gcdA = get_gcd(arrayA)
    gcdB = get_gcd(arrayB)

    result = 0
    if is_valid(gcdA, arrayB):
        result = max(result, gcdA)
    if is_valid(gcdB, arrayA):
        result = max(result, gcdB)

    return result
```

<br>

---
#### 061. 미로 탈출 (연습문제 lv.2)
- **문제에서 원하는 것**: 시작(S) → 레버(L) → 출구(E) 경로의 최소 이동 시간을 구하기 (갈 수 없다면 -1)

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 미로 정보가 담긴 문자열 배열 `maps`
    - 시작점 `S`
    - 출구 `E`
    - 레버 `L`
    - 통로 `O`
    - 벽 `X`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 한 칸 이동 = 1초  
    - 레버를 반드시 당겨야 출구를 열 수 있음  
    - 시작 → 레버 → 출구 순으로 이동  

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 최단 거리 문제이므로 BFS로 거리 탐색  

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. S, L, E의 좌표를 찾는다  

    2.BFS로 S → L 거리 계산  

    3. BFS로 L → E 거리 계산  

    4. 두 거리 모두 유효하면 더한 값을 리턴  
        
    5. 하나라도 -1이면 -1 리턴  

```python
from collections import deque

def solution(maps):
    n, m = len(maps), len(maps[0])

    # 시작, 레버, 출구 좌표 찾기
    for i in range(n):
        for j in range(m):
            if maps[i][j] == 'S':
                start = (i, j)
            elif maps[i][j] == 'L':
                lever = (i, j)
            elif maps[i][j] == 'E':
                exit_ = (i, j)

    # BFS 함수
    def bfs(start_pos, target_char):
        visited = [[False]*m for _ in range(n)]
        q = deque()
        q.append((start_pos[0], start_pos[1], 0))
        visited[start_pos[0]][start_pos[1]] = True

        while q:
            x, y, dist = q.popleft()
            if maps[x][y] == target_char:
                return dist

            for dx, dy in [(-1,0),(1,0),(0,-1),(0,1)]:
                nx, ny = x + dx, y + dy
                if 0 <= nx < n and 0 <= ny < m:
                    if not visited[nx][ny] and maps[nx][ny] != 'X':
                        visited[nx][ny] = True
                        q.append((nx, ny, dist+1))

        return -1

    # 거리 계산
    to_lever = bfs(start, 'L')
    to_exit = bfs(lever, 'E')

    if to_lever == -1 or to_exit == -1:
        return -1
    return to_lever + to_exit
```

<br>

---
#### 062. 124 나라의 숫자 (연습문제 lv.2)
- **문제에서 원하는 것**: **10진법 수 n**을 124 나라의 숫자로 변환하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 자연수 `n`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 자연수만 존재 (0은 없음)  
    - 사용 가능한 숫자: 1, 2, 4  

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 3진법과 유사한 방식으로 변환하되 나머지가 0일 땐 '4', 몫에서 1을 빼줘야 함  

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. `124 = ['4', '1', '2']`로 매핑 리스트를 만들기

    2. n이 0이 될 때까지:  
        ⇒ n을 3으로 나눈 나머지를 인덱스로 매핑 → 문자열 앞에 붙임  
        ⇒n이 3으로 나누어떨어질 경우 몫에서 1을 빼줌 (보정)  
        
    3. 결과 문자열 반환  

```python
def solution(n):
    result = ''
    nums = ['4', '1', '2']
    while n > 0:
        n, r = divmod(n, 3)
        if r == 0:
            n -= 1
        result = nums[r] + result

    return result
```

<br>

---
#### 063. 리코쳇 로봇 (연습문제 lv.2)
- **문제에서 원하는 것**: 로봇(R)이 목표(G) 에 정확히 도달하기 위한
최소 이동 횟수를 구하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 문자열 배열 (게임판) `board`
    - 시작 위치 `R`
    - 목표 위치 `G`
    - 장애물 `D`
    - 빈공간 `.`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 로봇은 상/하/좌/우 방향으로 이동
    - 한 번의 미끄러짐 = 한 번의 이동
    - 경로에 장애물이 있다면 멈출 수 없음

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: BFS를 사용해 최단 이동 횟수 탐색

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
1. R과 G의 좌표를 찾기

2. BFS를 위한 큐와 방문 배열 준비
    
3. 현재 위치에서 상/하/좌/우로 끝까지 이동 후 도착한 위치를 큐에 추가 (방문 체크)
    
4. 이동 중 G에 도착하면 현재 이동 횟수 + 1 리턴

5. 모든 경우 탐색 후 도달 못하면 -1 반환

```python
from collections import deque

def solution(board):
    n, m = len(board), len(board[0])
    visited = [[False]*m for _ in range(n)]

    # 시작점(R)과 목표점(G) 찾기
    for i in range(n):
        for j in range(m):
            if board[i][j] == 'R':
                start = (i, j)
            if board[i][j] == 'G':
                goal = (i, j)

    # 상, 하, 좌, 우 방향
    directions = [(-1,0), (1,0), (0,-1), (0,1)]

    # BFS 초기화
    q = deque()
    q.append((start[0], start[1], 0))
    visited[start[0]][start[1]] = True

    while q:
        x, y, cnt = q.popleft()

        # 목표에 도달하면 종료
        if (x, y) == goal:
            return cnt

        for dx, dy in directions:
            nx, ny = x, y
            # 한 방향으로 끝까지 미끄러짐
            while 0 <= nx + dx < n and 0 <= ny + dy < m and board[nx + dx][ny + dy] != 'D':
                nx += dx
                ny += dy

            if not visited[nx][ny]:
                visited[nx][ny] = True
                q.append((nx, ny, cnt + 1))

    # 도달하지 못한 경우
    return -1
```

<br>

---
#### 064. 무인도 여행 (연습문제 lv.2)
- **문제에서 원하는 것**: 모든 무인도별 식량 합을 오름차순으로 정렬해 배열로 반환  

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 문자열 배열 (격자 지도) `maps`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 상, 하, 좌, 우로 연결된 숫자 칸들은 한 무인도  
    - 무인도가 없으면 결과는 [-1]  

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: DFS or BFS로 연결된 땅들을 탐색하여 하나의 무인도 단위로 합산  

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
1. 방문 여부를 체크하는 2차원 배열 준비  

2. 지도 전체 순회하면서 방문하지 않은 숫자 칸 발견 시  
    ⇒ DFS/BFS로 연결된 무인도 전부 탐색  
    ⇒ 해당 무인도의 식량 합 계산  
    
2. 모든 무인도 식량 합을 리스트에 추가  

3. 리스트가 비어있으면 [-1] 반환, 아니면 오름차순 정렬 후 반환  

```python
def solution(maps):
    n, m = len(maps), len(maps[0])
    visited = [[False]*m for _ in range(n)]
    islands = []

    # 상하좌우 이동 방향
    directions = [(-1,0),(1,0),(0,-1),(0,1)]

    def dfs(x, y):
        stack = [(x, y)]
        total = 0
        visited[x][y] = True

        while stack:
            cx, cy = stack.pop()
            total += int(maps[cx][cy])
            for dx, dy in directions:
                nx, ny = cx+dx, cy+dy
                if 0 <= nx < n and 0 <= ny < m:
                    if not visited[nx][ny] and maps[nx][ny] != 'X':
                        visited[nx][ny] = True
                        stack.append((nx, ny))
        return total

    for i in range(n):
        for j in range(m):
            if maps[i][j] != 'X' and not visited[i][j]:
                islands.append(dfs(i, j))

    if not islands:
        return [-1]

    return sorted(islands)
```

<br>

---
#### 065. 줄 서는 방법 (연습문제 lv.2)
- **문제에서 원하는 것**: 1번부터 n번까지 번호가 매겨진 n명의 사람을 나열하는 모든 방법을 사전 순으로 정렬했을 때 k번째 순열을 구해서 배열로 반환  

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 사람 수 (1부터 n까지 번호) `n`
    - 원하는 순열의 순서 (1부터 n!까지 자연수) `k` 

<p style="margin-top: 2.3em;"></p>

- **조건**
    - n <= 20
    - k !<= n! (모든 순열 개수)
    - 순열을 사전 순으로 정렬한 결과에서 k번째 반환

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 팩토리얼을 이용해 각 자리 숫자를 결정하는 방식 1번째 자리부터 가능한 숫자 중 k가 속한 구간을 찾아 차례대로 선택  

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
1. 1부터 n까지 숫자를 리스트로 준비  

2. n! 구해서 각 자리 선택 시 사용할 팩토리얼 배열 생성  
    
3. k를 1-based 인덱스로 사용하며  
    : 각 자리마다 (k-1) // factorial 값을 이용해 해당 자리 숫자를 결정  
    : 선택한 숫자는 리스트에서 제거  
    : k를 업데이트하여 다음 자리로 이동  
    
4. 모든 자리 숫자 결정 후 결과 리스트 반환  

```python
def solution(n, k):
    import math

    numbers = list(range(1, n+1))
    answer = []
    k -= 1  # 0-based index로 변환

    for i in range(n, 0, -1):
        fact = math.factorial(i-1)
        index = k // fact
        answer.append(numbers.pop(index))
        k %= fact

    return answer
```

<br><br>