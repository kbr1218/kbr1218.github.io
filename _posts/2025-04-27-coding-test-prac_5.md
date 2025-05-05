---
layout: post
title:  "[Programmers] 코딩테스트 연습문제 5 (Heap)"
author: me
categories: [ coding-test ]
date: 2025-04-27 08:00:00
image: https://programmers.co.kr/assets/img-meta-programmers-411e94bf29153dc31004168e6cd500279b1a531a23689303755e51971dee4526.png
---

#### 021. 더 맵게 (Heap lv.2)
- **문제에서 원하는 것**: 모든 음식의 스코빌 지수를 `K` 이상으로 만들기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 음식의 스코빌 지수를 담은 배열 `scoville`
    - 원하는 스코빌 지수 `K`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - `섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)`

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 스코빌 지수가 가장 낮은 두 개를 뽑아서 새 음식을 만들어 다시 넣기 ⇒ 반복 ⇒ 우선순위 큐(Heap)이 가장 빠름 ⇒ heapq 모듈 사용

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. `heapq.heapify(scoville)` ⇒ 리스트를 바로 최소 힙으로 변환하는 함수

    2. 반복  
        힙에서 가장 작은 값이랑 두 번째로 작은 값 꺼내기  
        새로운 음식 스코빌 지수를 계산해서 힙에 다시 넣기  
        섞은 횟수 `answer + 1`  

    3. 힙에 있는 모든 값이 `K` 이상이면 `answer` 반환, 합칠 음식이 없는데 `K`를 못 넘기면 `-1` 반환

```python
import heapq

def solution(scoville, K):
    ### 1. 리스트를 최소 힙으로 변환
    heapq.heapify(scoville)
    # 섞은 횟수
    answer = 0
    
    while scoville:
        # 가장 작은 값 꺼내기
        least = heapq.heappop(scoville)
        if least >= K:
            # 이미 모두 K 이상이면 종료
            return answer
        
        # 더 이상 섞을 음식이 없으면 실패
        if not scoville:
            return -1
        
        # 두 번째 작은 값 꺼내기
        second_least = heapq.heappop(scoville)
        # 새로운 음식의 스코빌 계산
        new_one = least + (second_least * 2)
        # 힙에 넣기
        heapq.heappush(scoville, new_one)
        answer += 1
    
    # while문이 종료해도 종료 못 했으면 실패
    return -1
```

<br>

---
#### 022. 이중우선순위큐 (Heap lv.3)
- **문제에서 원하는 것**: 모든 연산을 처리한 후 큐의 상태 return하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 이중우선순위큐가 할 연산 배열 `operations`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - `operations` 배열의 원소는 “명령어 데이터” 형식으로 주어짐
    - `I 숫자`: 큐에 주어진 숫자 삽입
    - `D 1`: 큐에서 최댓값 삭제
    - `D -1`: 큐에서 최솟값 삭제

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: heap을 사용해서 최댓값과 최솟값 삭제하기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. `min_heap`: `(값, 고유ID)` 로 저장 (기본 min-heap)
    
    2. `max_heap`: `(-값, 고유ID)` 로 저장 (음수값으로 최대 힙처럼 사용)
    
    3. `visited[id] = True`: 아직 유효한 값인지 추적

    4. 각 연산 처리  방법
        `I x`: 두 힙에 동시에 삽입  
        `D 1`: max_heap에서 아직 유효한 값이 나올 때까지 `pop`  
        `D -1`: min_heap에서 아직 유효한 값이 나올 때까지 `pop`  
    
    5. 두 힙에서 동기화된 유효한 값만 남기고 최댓값, 최솟값 추출

```python
import heapq

def solution(operations):
    min_heap = []
    max_heap = []
    # 인덱스로 유효 여부를 추적
    visited = [False] * len(operations)
    
    for i, op in enumerate(operations):
        # 경우에 따른 각 연산 처리
        if op.startswith("I"):
            # 숫자 부분만 각 힙에 삽입
            num = int(op.split()[1])
            heapq.heappush(min_heap, (num, i))
            heapq.heappush(max_heap, (-num, i))
            visited[i] = True
            
        elif op == "D 1":
            # max_heap에서 유효한 값이 나올 때까지 pop
            while max_heap and not visited[max_heap[0][1]]:
                heapq.heappop(max_heap) # 이미 삭제된 값은 skip
            if max_heap:
                _, idx = heapq.heappop(max_heap)
                # 제거한 값이니 False로 변경
                visited[idx] = False
                
        elif op == "D -1":
            while min_heap and not visited[min_heap[0][1]]:
                heapq.heappop(min_heap)
            if min_heap:
                _, idx = heapq.heappop(min_heap)
                visited[idx] = False
    
    
    # 남은 유효한 값만 추출
    while min_heap and not visited[min_heap[0][1]]:
        heapq.heappop(min_heap)
    while max_heap and not visited[max_heap[0][1]]:
        heapq.heappop(max_heap)
    
    # 큐가 비어있다면
    if not min_heap or not max_heap:
        return [0, 0]
    
    # [최댓값, 최솟값] return
    return [-max_heap[0][0], min_heap[0][0]]
```

<br><br>