---
layout: post
title:  "[Programmers] 코딩테스트 연습문제 3 (DFS/BFS, 정렬)"
author: me
categories: [ coding-test ]
date: 2025-04-16 08:00:00
image: https://programmers.co.kr/assets/img-meta-programmers-411e94bf29153dc31004168e6cd500279b1a531a23689303755e51971dee4526.png
---

#### 009. 게임 맵 최단거리 (DFS/BFS lv.2)
- **문제에서 원하는 것**: `(1, 1)` 위치에서 출발해서 `(n, m)` 위치에 최단 거리로 도달하는 칸 수 구하기 (도달하지 못 하면 -1 반환)

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - n x m 크기의 2차원 배열: `maps`
    - 이동 가능한 길: `1`
    - 이동 불가능한 길: `0`
    - 시작 위치: `(0, 0)`
    - 도착 위치: `( n-1, m-1 )`


<p style="margin-top: 2.3em;"></p>

- **조건**
    - 한 번에 상하좌우 1칸 이동 가능
    - 게임 맵을 벗어난 길은 갈 수 없다
    - `n, m <= 100`
    - 도달하지 못하면 -1 반환

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 최단거리 문제에서는 BFS가 항상 유리함! ⇒ 너비우선탐색(BFS)

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. BFS 준비  
        큐를 사용하기  
        방문 확인용 배열과 방향(상하좌우) 배열을 정의  
            `dx = [-1, 1, 0, 0]`, `dy = [0, 0, -1, 1]`  
            상(-1, 0), 하(1, 0), 좌(0, -1), 우(0, 1)

    2. BFS 탐색 시작    
        `(0, 0)`을 시작으로 큐에 삽입  
        방문할 수 있는 1을 기준으로만 이동  
        방문하면 현재 거리 +1로 maps 값 갱신  

    3. 결과 반환  
        `(n-1, m-1)`에 도착하면 `maps[n-1][m-1]` 값 반환  
        끝까지 탐색했는데 상대 진영에 도착하지 못한다면 `-1` 반환  


```python
# 필요한 모듈 import
from collections import deque

def solution(maps):
    ### 1. BFS 준비
    # 맵의 크기 확인
    n = len(maps)        # 행
    m = len(maps[0])     # 열
    
    # 방향 배열 설정 (상, 하, 좌, 우)
    dx = [-1, 1, 0, 0]
    dy = [0, 0, -1, 1]
    
    # 큐 초기화 (시작 위치 넣기)
    queue = deque()
    queue.append((0, 0))
    
    ### 2. BFS 탐색
    # 큐가 빌 때까지 while문 반복
    while queue:
        # 현재 위치를 x, y 변수에 저장
        x, y = queue.popleft()
        
        # 상하좌우 방향 탐색
        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]
            
            # 맵을 벗어났다면
            if nx<0 or ny<0 or nx>=n or ny>=m:
                continue
            # 벽(0)인지 / 이미 방문했다면
            if maps[nx][ny] == 0:
                continue
            # 방문 가능하다면
            if maps[nx][ny] == 1:
                # 거리 기록하고 큐에 추가
                maps[nx][ny] = maps[x][y] + 1
                queue.append((nx, ny))
                
    ### 3. 결과 반환
    # maps[n-1][m-1]의 값이 1이라면 도착하지 못한 것
    if maps[n-1][m-1] == 1:
        answer = -1
    # 도착 가능하다면 누적된 거리 반환
    else:
        answer = maps[n-1][m-1]
    
    return answer
```

<br>

---
#### 010. 타겟 넘버 (DFS/BFS lv.2)
- **문제에서 원하는 것**: 숫자를 더하거나 빼서 `target`을 만드는 방법의 수 구하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 사용할 수 있는 숫자 배열 `numbers`
    - 만들고자 하는 목표 숫자 `target`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 순서는 바꾸지 않기
    - 각 숫자 앞에 `+` 또는 `-`로 계산
    - 모든 경우의 수를 고려해 `target`을 만드는 경우만 카운트

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 모든 경우의 수를 탐색해야 하는 DFS(깊이 우선 탐색)

<p style="margin-top: 2.3em;"></p>

- **풀이 전략 1**:  
    1. `numbers` 배열을 하나씩 보면서

    2. 현재 숫자에 `+` 또는 `-`를 붙여 두 개로 재귀 호출

    3. 끝까지 가서 누적 합이 `target`이 되면 `count` 변수 ++

```python
def solution(numbers, target):
    answer = 0
    
    # 깊이 우선 탐색 함수 정의
    def dfs(index, total):
        nonlocal answer
        
        # 종료 조건: 모든 숫자를 다 사용했을 때
        if index == len(numbers):
            # 계산 결과가 target과 일치한다면
            if total == target:
                answer += 1
            return
        # 현재 숫자를 더하거나 빼는 재귀함수
        dfs(index + 1, total + numbers[index])
        dfs(index + 1, total - numbers[index])
    
    dfs(0, 0)
    return answer
```

<br>

---
#### 011. K번째 수 (정렬 lv.1)
- **문제에서 원하는 것**: 배열의 잘라 정렬한 후 특정 위치에 있는 수 구하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 배열 `array`
    - `i, j, k`를 원소로 가진 2차원 배열 `commands`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - `array`의 길이는 1 이상 100 이하
    - `array`의 각 원소는 1 이상 100 이하
    - `commands`의 길이는 1 이상 50 이하
    - `commands`의 각 원소는 길이가 3 (`i, j, k`)

<p style="margin-top: 2.3em;"></p>

- **접근 방법**:  슬라이싱으로 자르고 `sorted`로 배열 정렬

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. command의 원소 개수만큼 반복

    2. `i, j, k` 변수 정의

    3. `array` 슬라이싱하고 `sorted()`로 정렬

    4. `k`번째 수 return


```python
def solution(array, commands):
    answer = []
    
    ### 1. commands 원소 개수만큼 반복
    for cmd in commands:
        ### 2. i, j, k 변수 찾기
        i, j, k = cmd
        
        ### 3. array 슬라이싱하고 sorted로 배열 정렬
        sliced_arr = sorted(array[i -1 : j])
        answer.append(sliced_arr[k - 1])
    return answer
```

<br>

---
#### 012. 가장 큰 수 (정렬 lv.2)
- **문제에서 원하는 것**: 주어진 숫자들을 이어 붙여서 만들 수 있는 가장 큰 수 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 0 또는 양의 정수가 담긴 배열 `numbers`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - `numbers`의 길이는 1 이상 100,000 이하
    - `numbers`의 원소는 0 이상 1,000 이하
    - 문자열로 바꿔서 return

- **접근 방법**: 이어붙였을 때 큰 조합을 선택해야 함

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 숫자를 문자열로 바꾸고 비교하기 (`compare` 함수 새로 정의)

    2. 정렬 기준 정의  
        `a + b > b + a`면 a가 앞에 와야 함

    3. 정렬된 문자열을 이어붙이기  
        결과가 `'0000'`같은 수라면 `0`으로 리턴

<p style="margin-top: 3.3em;"></p>

```python
from functools import cmp_to_key

# 비교 함수
def compare(x, y):
    if x + y > y + x:
        return -1 # x 먼저
    elif x + y < y + x:
        return 1  # y 먼저
    else:
        return 0

def solution(numbers):
    # 문자열로 변경
    numbers = list(map(str, numbers))
    # 비교함수로 정렬
    # -1을 리턴하면 x가 앞에, 1을 리턴하면 y가 앞에
    numbers.sort(key = cmp_to_key(compare))
    
    # 정렬된 문자열 이어붙이기
    result = ''.join(numbers)
    
    return '0' if result[0] == '0' else result
```

<br>

---
#### 013. H-Index (정렬 lv.2)
- **문제에서 원하는 것**: 과학자의 H-Index 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 논문의 인용 횟수를 담은 배열 `citations`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 발표한 논문 `n`편 중 `h`번 이상 인용된 논문이 `h`편 이상이고 나머지 논문이 `h`번 이하 인용되었다면 `h`의 최댓값이 H-Index
    - 과학자가 발표한 논문의 수는 1편 이상 1,000편 이하
    - 논문별 인용 횟수는 0회 이상 10,000회 이하

- **접근 방법**: 반복문으로 배열을 순회하면서 `h`의 조건을 만족하는 최댓값 찾기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 논문의 수 `n`만큼의 범위(0~n)의 숫자를 `h`로 가정

    2. 매 `h`마다 `citations`에서 `h`이상 인용된 논문 수를 확인

    3. 그 수가 `h` 이상이면 정답 후보

    4. 후보 중 가장 큰 `h`를 정답으로 선택

<p style="margin-top: 3.3em;"></p>

```python
def solution(citations):
    ### 1. 논문의 수만큼 범위의 숫자를 h로 가정
    n = len(citations)
    
    # h 후보 배열
    h_arr = []
    
    ### 2. 매 h마다 cirations에서 h번 이상 인용된 논문 수 확인
    for h in range(n + 1):
        # h 이상 인용된 논문 개수 확인 변수
        count = 0
        for citation in citations:
            if citation >= h:
                count += 1
        # h번 인용된 i라면 후보에 추가
        if count >= h:
            h_arr.append(h)
                
    return max(h_arr)
```

<p style="margin-top: 2.3em;"></p>

- **풀이 전략 2 (sorted 함수)**:  
    1. `citations` 배열을 내림차순으로 정렬

    2. 앞에서부터 순회하면서 `citations[i] >= i + 1`이면 h-index 후보

    3. 조건을 만족하지 않는 값이 나오면 이전 값이 h-index의 최댓값

```python
def solution(citations):
    ### 1. 내림차순 정렬
    citations.sort(reverse=True)
    
    for i in range(len(citations)):
        # 해당 조건을 만족하지 않는 순간 이전 i가 H-index의 max값
        if citations[i] < i + 1:
            return i
    # 전부 만족하면 전체 개수가 h-index
    return len(citations)
```


<br>


<br><br>