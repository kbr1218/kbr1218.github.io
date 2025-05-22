---
layout: post
title:  "[Programmers] 코딩테스트 연습문제 7 (Greedy/DFS/BFS)"
author: me
categories: [ coding-test ]
date: 2025-05-12 08:00:00
image: https://programmers.co.kr/assets/img-meta-programmers-411e94bf29153dc31004168e6cd500279b1a531a23689303755e51971dee4526.png
---

#### 029. 단속카메라 (Greedy lv.3)
- **문제에서 원하는 것**: 설치해야 하는 단속카메라의 최소 개수 구하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 고속도로를 이동하는 차량의 경로 `routes`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 모든 차량이 단속용 카메라를 한 번은 만나도록 하기  

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 최대한 많은 차가 공유하는 지점에 카메라를 설치하면서, 겹치지 않은 새로운 구간이 생기면 카메라 추가로 설치하기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 차량 경로를 진출 지점으로 오름차순 정렬  

    2. 가장 먼저 진출하는 차량의 진출 지점에 카메라 설치하기  

    3. 다음 차량의 진입 지점이 현재 카메라 위치보다 위에 있다면 새 카메라 설치 (해당 차량의 진출 지점에)  

    4. 모든 차량에 대해 반복

```python
def solution(routes):
    ### 1. 진출 지점 기준으로 오츰차순 정렬
    routes.sort(key=lambda x:x[1])
    
    ### 2. 첫 번째 차량의 진출 지점에 카메라 설치
    camera_position = routes[0][1]
    answer = 1
    
    ### 3. 반복문으로 나머지 차량 검사
    for route in routes[1:]:
        # 현재 차량의 진입 지점이 카메라 위치보다 뒤에 있으면 새 카메라 설치
        if route[0] > camera_position:
            answer += 1
            # 진출 지점에 설치
            camera_position = route[1]
            
    return answer
```

<br>

---
#### 030. 섬 연결하기 (Greedy lv.3)
- **문제에서 원하는 것**: 최소 비용으로 모든 섬이 통행 가능하도록 만들기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 섬의 개수 `n`
    - 섬 사이에 다리를 건설하는 비용 `costs`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 다리를 여러 번 건너도 도달할 수만 있다면 통행 가능함
    - `costs[i][0]`와 `costs[i][1]`는 다리가 연결되는 두 섬의 번호, `costs[i][2]`는 두 섬을 연결하는 다리를 건설할 때 드는 비용

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 모든 섬을 최소 비용으로 연결하는 최소 신장 트리 만들기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 모든 `cost`를 비용을 기준으로 오름차순 정렬  

    2. 각 섬을 독립된 집합으로 초기화  
    
    3. 비용이 작은 간선부터 하나씩 확인하면서  
        ⇒ 두 섬이 서로 다른 집합에 속해있으면 연결하고  
        ⇒ 두 섬을 하나의 집합으로 합치기          
        ⇒ 연결한 간선을 총 비용에 더하기  
        
    4. 이 과정을 `n-1`개의 간선을 선택할 때까지 반복

```python
def solution(n, costs):
    ### 1. 비용 기준으로 간선 정렬
    costs.sort(key=lambda x: x[2])
    
    ### 2. 부모 배열 초기화
    parent = [i for i in range(n)]
    
    # 대표 노드를 찾는 find 함수 정의
    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]
    
    # 두 노드를 연결하는 union 함수 정의
    def union(a, b):
        root_a = find(a)
        root_b = find(b)
        if root_a != root_b:
            parent[root_b] = root_a
            return True
        return False
    
    ### 3. 최소 비용 계산
    total_cost = 0
    edge_count = 0
    
    for a, b, cost in costs:
        # 다른 집합이면
        if union(a, b):
            total_cost += cost
            edge_count += 1
            # n - 1개의 간선이 될 때까지 반복
            if edge_count == n - 1:
                break
    
    return total_cost
```

<br>

---
#### 031. 네트워크 (DFS/BFS lv.3)
- **문제에서 원하는 것**: 네트워크의 개수 return하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 컴퓨터의 개수 `n`
    - 연결에 대한 정보가 담긴 2차원 배열 `computers`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 각 컴퓨터는 0부터 `n-1`인 정수로 표현
    - `i`번 컴퓨터와 `j`번 컴퓨터가 연결되어 있으면 `computer[i][j]`를 1로 표현

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 각 컴퓨터를 하나의 노드로 생각하고 무방향 그래프의 연결 요소 개수를 찾기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 방문 여부를 기록할 리스트 `visited` 만들기

    2. 0번부터 n-1번까지 컴퓨터를 순회하면서 방문하지 않은 컴퓨터가 있다면  
        ⇒ 그 컴퓨터를 시작으로 DFS나 BFS 수행  
        ⇒ 네트워크 개수 증가  
        
    3. 순회가 끝날 때까지 반복

```python
def solution(n, computers):
    ### 1. 방문 여부를 기록할 리스트 정의
    visited = [False] * n
    answer = 0
    
    ### DFS 함수 정의
    def dfs(current):
        visited[current] = True
        for neighbor in range(n):
            if computers[current][neighbor] == 1 and not visited[neighbor]:
                dfs(neighbor)
                
    # 방문하지 않은 컴퓨터면 새로운 네트워크 시작
    for i in range(n):
        if not visited[i]:
            dfs(i)
            answer += 1
    
    return answer
```

<br>

---
#### 032. 단어 변환 (DFS/BFS lv.3)
- **문제에서 원하는 것**: begin에서 target 단어로 변환하는 가장 짧은 과정 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 두 개의 단어 `begin`, `target`
    - 단어의 집합 `words`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - `begin`과 `target`은 같지 않음
    - 변환할 수 없는 경우는 0을 return

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 최단 거리를 구하는 BFS

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. 한 글자만 다른 단어를 찾는 함수 만들기  

    2. BFS 탐색  
        ⇒ 시작 단어 `begin`을 큐에 넣고 방문 여부 기록하기  
        ⇒ 큐에서 단어를 하나씩 꺼내면서 한 글자 차이나는 단어들 중 아직 방문하지 않은 단어를 큐에 추가  
        ⇒ 각 단어에 도달했을 때 변환 횟수 저장  
        
    3. `target` 단어에 도달하면 변환 횟수 `answer` 반환

```python
from collections import deque

def solution(begin, target, words):
    # target이 words에 없으면 0 리턴
    if target not in words:
        return 0
    
    def is_one_char_diff(word1, word2):
        diff_count = sum(1 for a, b in zip(word1, word2) if a != b)
        return diff_count == 1
    
    visited = set()
    # (현재 단어, 변환 횟수)
    queue = deque([(begin, 0)])
    
    while queue:
        current_word, steps = queue.popleft()
        
        if current_word == target:
            return steps
        
        for word in words:
            if word not in visited and is_one_char_diff(current_word, word):
                visited.add(word)
                queue.append((word, steps + 1))
    
    return 0
```

<br>

---
#### 033. 여행경로 (DFS/BFS lv.3)
- **문제에서 원하는 것**: 주어진 항공권을 모두 써서 여행 경로 짜기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 항공권 정보가 담긴 2차원 배열 `tickets`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 주어진 항공권 모두 사용하기
    - 모든 도시를 방문할 수 없는 경우는 주어지지 않음
    - 가능한 경로가 두 개일 경우, 알파벳 순서가 앞서는 경로 return

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 모든 티켓을 사용한 경로 중 사전순으로 가장 빠른 경로 찾기 (DFS)

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. 그래프를 인접 리스트 형식으로 만들기  
        ⇒ 도착 공항 리스트는 알파벳 순으로 정렬
        
    2. DFS로 경로 탐색  
        ⇒ 경로에 공항을 하나씩 추가하고 항공권을 사용한 티켓 리스트에서 제거하면서 이동  
        ⇒ 모든 티켓을 사용한 시점에서 경로 리턴  
        
    3. 백트래킹으로 다른 가능한 경로 찾기 시도 (사전순으로 가장 빠른걸 반환해야 하니까 첫 번째로 성공한 경로를 바로 반환해도 ㄱㅊ)

```python
from collections import defaultdict

def solution(tickets):
    graph = defaultdict(list)
    
    ### 1. 그래프 구성
    for src, dst in tickets:
        graph[src].append(dst)
    for src in graph:
        graph[src].sort()
        
    # 최종 경로
    answer = []
    ticket_count = len(tickets)
    
    def dfs(current, path):
        if len(path) == ticket_count + 1:
            # 모든 티켓을 사용했을 때
            answer.extend(path)
            return True
        
        if current not in graph:
            return False
        
        # 백트래킹을 위한 배열 저장
        temp_list = list(graph[current])
        
        for i, next_dest in enumerate(temp_list):
            #티켓 사용
            graph[current].pop(i)
            if dfs(next_dest, path + [next_dest]):
                return True
            graph[current].insert(i, next_dest)
        
        return False
    
    dfs("ICN", ["ICN"])
    return answer
```

<br>

---
#### 034. 베스트앨범 (Hash lv.3)
- **문제에서 원하는 것**: 베스트 앨범에 들어갈 노래를 순서대로 return

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 노래 장르를 나타내는 문자열 배열 `genres`
    - 노래별 재생 횟수를 나타내는 정수 배열 `plays`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 속한 노래가 많이 재생된 장르 먼저 수록
    - 장르 내에서 많이 재생된 노래 먼저 수록
    - 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래 먼저 수록

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 해시로 장르별 곡을 묶고 정렬한 후 상위 2개 선택

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. 입력 데이터를 장르별로 묶기

    2. 장르별 총 재생 횟수 계산하기

    3. 총 재생 수 기준으로 장르를 내림차순으로 정렬

    4. 각 장르에 대해  
        ⇒ 그 장르의 곡들을 재생 수 내림차순, 고유번호 오름차순으로 정렬  
        ⇒ 상위 1~2곡의 고유 번호를 결과 리스트에 추가  

```python
from collections import defaultdict

def solution(genres, plays):
    genre_to_songs = defaultdict(list)
    genre_play_count = defaultdict(int)
    
    ### 1-2. 장르별 곡 정보와 재생 수 저장
    for idx, (genre, play) in enumerate(zip(genres, plays)):
        genre_to_songs[genre].append((play, idx))
        genre_play_count[genre] += play
        
    ### 3. 장르를 총 재생 수 기준으로 내림차순 정렬
    sorted_genres = sorted(genre_play_count.items(), key=lambda x: x[1], reverse=True)
    answer = []
    
    ### 4. 각 장르 내에서 곡 정렬 후 상위 두 개 선택
    for genre, _ in sorted_genres:
        # 재생 수 내림차순 >> 고유 번호 오름차순 정렬
        songs = sorted(genre_to_songs[genre], key=lambda x: (-x[0], x[1]))
        top_songs = songs[:2]  # 최대 두 곡
        answer.extend([idx for _, idx in top_songs])
    return answer
```

<br>

---
#### 035. 자연수 뒤집어 배열로 만들기 (연습문제 lv.1)
- **문제에서 원하는 것**: 자연수를 뒤집어 배열로 만들기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 자연수 `n` 

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 접근 방법: 자연수 `n`을 문자열로 바꾸고 각 자리수를 뒤에서부터 숫자로 변환해 배열로 만들기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. 자연수 `n`을 문자열로 변환

    2. 문자열 뒤집기

    3. 각 문자를 정수로 변환하여 리스트에 저장

```python
def solution(n):
    return [int(ch) for ch in str(n)[::-1]]
```

<br>

---
#### 036. 나누어 떨어지는 숫자 배열 (연습문제 lv.1)
- **문제에서 원하는 것**: 배열 안의 숫자 중 `divisor`로 나누어 떨어지는 값 오름차순 정렬하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 자연수를 담은 배열 `arr`
    - 나누는 자연수 `divisor`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 나누어 떨어지는 element가 없다면 -1을 담은 배열 return

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: `divisor`로 하나씩 나누면서 나누어 떨어지는 값 배열에 담기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. 배열에서 `divisor`로 나누어 떨어지는 값만 필터링  
        ⇒ `element % divisor == 0`
        
    2. 필터링한 결과가 비어있지 않으면 오름차순 정렬하여 반환

    3. 비어있다면 `[-1]` 반환

```python
def solution(arr, divisor):
    answer = []
    for i in range(len(arr)):
        if arr[i] % divisor == 0:
            answer.append(arr[i])
    
    if len(answer) == 0:
        return [-1]
    else: 
        return sorted(answer)

# --- 더 간단한 버전
def solution(arr, divisor):
		answer = sorted([x for x in arr if x % divisor == 0])
		return answer if answer else [-1]
```

<br>

---
#### 037. n^2 배열 자르기 (월간 코드 챌린지 시즌 3 lv.2)
- **문제에서 원하는 것**: 주어진 정수로 1차원 배열 만들기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 정수 `n`, `left`, `right`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - `n`행 `n`열 크기의 비어있는 2차원 배열 만들기
    - `i = 1, 2, 3, ..., n`에 대해서, 1행 1열부터 `i`행 `i`열까지의 영역 내의 모든 빈 칸을 숫자 `i`로 채우기
    - 1행, 2행, …, n행을 잘라내어 이어붙인 1차원 배열 만들기
    - 새로운 1차원 배열을 `arr`이라고 할 때, `arr[left]`, `arr[left+1]`, ..., `arr[right]`만 남기고 나머지는 모두 지우기

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: `n * n` 배열이 일차원 배열로 펼쳐졌을 때의 인덱스 → 좌표 변환

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. `n*n` 배열을 일차원 배열로 펼쳤을 때, index `i`는 (i // n)행, (i % n)열에 해당  
        ⇒ `value(i) = max(i // n, i % n) + 1`
        
    2. i가 left부터 right 까지 범위에 해당하므로 위 수식을 적용한 값 배열로 반환

```python
def solution(n, left, right):
    answer = []
    
    for i in range(left, right + 1):
        row = i // n
        col = i % n
        answer.append(max(row, col) + 1)
    return answer
```

<br><br>