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

<br><br>