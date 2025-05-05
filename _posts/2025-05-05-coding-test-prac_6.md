---
layout: post
title:  "[Programmers] 코딩테스트 연습문제 6 (Stack/Queue)"
author: me
categories: [ coding-test ]
date: 2025-05-05 08:00:00
image: https://programmers.co.kr/assets/img-meta-programmers-411e94bf29153dc31004168e6cd500279b1a531a23689303755e51971dee4526.png
---

#### 023. 같은 숫자는 싫어 (스택/큐 lv.1)
- **문제에서 원하는 것**: 배열에서 연속적으로 나타나는 숫자를 하나만 남기고 제거하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 숫자 0부터 9까지로 이루어진 배열 `arr`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 배열의 순서는 유지하기  

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 하나씩 큐에 넣으면서 같은 숫자가 있다면 넣지 않기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 큐 정의 `from collections import deque`  

    2. 배열을 돌면서 현재 숫자와 큐의 마지막 숫자 비교  
        큐가 비어있거나 마지막 숫자와 다르면 배열에 `append`, 같으면 넘어간다  
        
    3. 순회가 끝나면 배열 return

```python
from collections import deque

def solution(arr):
    ### 1. 큐 정의
    queue = deque()
    
    ### 2. 연속으로 나타나는 숫자 제거
    for num in arr:
        if not queue or queue[-1] != num:
            queue.append(num)
    return list(queue)
```

<br>

---
#### 024. 기능개발 (스택/큐 lv.2)
- **문제에서 원하는 것**: 각 배포마다 배포되는 기능의 수 return

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 작업의 진도가 적힌 정수 배열 `progress`
    - 각 작업의 개발 속도가 적힌 정수 배열 `speeds`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 배포는 하루에 한 번만 가능

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 각 작업이 완료되기까지 걸리는 일 수를 계산하고, 배포 가능한 시점을 기준으로 묶어주는 방식

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 작업별 완료까지 걸리는 날짜 계산하기  
        각 작업에 대해 `(100 - 현재 진행도) // 개발 속도`를 해서 며칠이 걸리는지 계산 (나누어 떨어지지 않으면 하루 더 걸리니까 `ceil`로 올림해야 함)  
        
    2. 앞 작업이 끝나야지 뒤에 것도 배포 가능  
        작업을 앞에서부터 보면서, 현재 배포 기준 날짜보다 작은 날짜가 나오면 같은 배포 그룹 (더 큰 배포 그룹이 나오면 새로운 배포 그룹으로 묶기)  
        
    3. 각 배포 그룹의 기능 개수를 리스트에 저장

```python
import math

def solution(progresses, speeds):
    result = []
    
    ### 1. 작업별 완료까지 걸리는 날짜 계산
    days_required = [math.ceil((100 - p) / s) for p, s in zip(progresses, speeds)]
    
    ### 2. 배포 그룹 묶기
    current_max_day = days_required[0]
    count = 1
    
    for day in days_required[1:]:
        if day <= current_max_day:
            count += 1             # 같은 배포 그룹
        else:
            result.append(count)   # 지금까지 묶인 개수 저장
            current_max_day = day  # 새로운 배포 기준일 갱신
            count = 1              # 초기화
    result.append(count)
    
    return result
```

<br>

---
#### 025. 올바른 괄호 (스택/큐 lv.2)
- **문제에서 원하는 것**: 올바른 괄호면 true, 아니면 false 리턴

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 괄호로 이루어진 문자열 `s`

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 문자열을 순서대로 스택에 넣으면서 확인

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 경우의 수 나누기
        1. 스택이 비어있는데 ‘)’가 들어오면 ⇒ False
        2. 스택에 ‘(’가 들어오면 ⇒ `append`
        3. 스택의 `top`에 ‘(’가 있는데 ‘)’가 들어오면 ⇒ `pop`
        4. 스택에서 ‘()’를 다 제외했는데 괄호가 남아있으면 ⇒ False

```python
def solution(s):
    stack = []
    for i in s:
        # 스택이 비어있는데 ')'가 들어오는 경우 >> False
        if len(stack) == 0 and i == ')':
            return False
        # 스택에 열린 괄호가 들어오는 경우 >> append
        if i == '(':
            stack.append('(')
        # 스택 맨 위에 '('가 있는데 닫힌 괄호가 들어오는 경우 >> pop
        if stack[-1] == '(' and i == ')':
            stack.pop()
    # 스택에서 '()'를 다 제외했는데 괄호가 남아있는 경우 >> False
    return False if len(stack) != 0 else True
```

<br>

---
#### 026. 프로세스 (스택/큐 lv.2)
- **문제에서 원하는 것**: 특정 프로세스가 몇 번째로 실행되는지 return

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 프로세스의 중요도가 순서대로 담긴 배열 `priorities`
    - 몇 번째로 실행되는지 알고싶은 프로세스의 위치 `location`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 한 번 실행한 프로세스는 다시 큐에 넣지 않는다

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 큐에서 하나 꺼낸 뒤 뒤에 더 높은 우선순위가 있는지 확인하고 있으면 다시 맨 뒤로, 없으면 실행

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. 큐 정의: `priorities`와 `index`를 묶어서 튜플로 저장 ⇒ 원래 위치 `location` 추적 가능  

    2. `pop`으로 큐에서 프로세스 하나를 꺼냄  

    3. 남아있는 큐에 꺼낸 것보다 우선순위가 높은 게 하나라도 있다면 꺼낸 걸 `append`로 다시 넣기  

    4. 없으면 실행되는 거니까 `count += 1`하고 `location`이라면 반환

```python
from collections import deque

def solution(priorities, location):
    ### 1. 큐 정의
    queue = deque([(i, p) for i, p in enumerate(priorities)])
    answer = 0
    
    while queue:
        idx, priority = queue.popleft()
        # 더 높은 우선순위가 있다면 큐에 다시 넣기
        if any(priority < other_priority for _, other_priority in queue):
            queue.append((idx, priority))
        else:
            # 종료한 프로세스의 수 ++
            answer += 1
            # location과 같다면 리턴
            if idx == location:
                return answer
```

<br>

---
#### 027. 다리를 지나는 트럭 (스택/큐 lv.2)
- **문제에서 원하는 것**: 모든 트럭이 순서대로 최단 시간 안에 다리 건너기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 다리에 올라갈 수 있는 트럭 수 `bridge_length`
    - 다리가 견딜 수 있는 무게 `weight`
    - 트럭별 무게가 담긴 배열 `truck_weights`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 트럭은 순서대로 다리를 건더야 함
    - 다리에 완전히 오르지 않은 트럭의 무게는 무시
    - `bridge_length`== 트럭이 다리를 건너는 데 걸리는 시간

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 큐에 트럭을 차례로 넣으면서 트럭이 `bridge_length`만큼 다리를 건넜다면 큐에서 제거하는 방식으로 큐를 움직이면서 초 계산

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. 길이가 `bridge_length`인 큐 정의  

    2. 시간을 1초씩 증가시키면서 반복문 수행
    
    3. 맨 앞 칸(`popleft`) 제거 → 트럭이 다리를 다 건넌 경우 → 해당 트럭의 무게를 `total_weight`에서 빼기
    
    4. 다음 대기 트럭이 있고, (total_weight + 다음 트럭의 무게 ≤ weight)라면 트럭을 큐에 `append`하고 `total_weight`에 해당 트럭 무게 더하기
    
    5. 다리에 올라갈 수 없는 경우 큐에 `0`을 `append`해서 빈 공간 채우기 (그냥 시간만++)
    
    6. `bridge` 큐가 빌 때까지 반복

```python
from collections import deque

def solution(bridge_length, weight, truck_weights):
    ### 1. 큐 정의
    bridge = deque([0] * bridge_length)
    # 트럭의 무게 배열도 큐로 변경 (시간 최적화)
    # 파이썬의 일반 리스트 `list`에서 `pop(0)`을 쓰면 O(n)의 시간이 걸리는데,
    # deque는 `popleft()`의 연산이 O(1)로 매우 빠름
    truck_weights = deque(truck_weights)
    answer = 0  # 시간
    total_weight = 0
    
    ### 2. bridge 큐가 비어있지 않을 때까지 시간을 증가시키면서 반복문 수행
    while bridge:
        answer += 1
        
        # 한 칸 전진 (맨 앞 트럭 빼기)
        total_weight -= bridge.popleft()
        
        # 건너야 할 트럭이 남아있다면
        if truck_weights:
            ### 3. 다음 트럭도 건널 수 있다면
            if total_weight + truck_weights[0] <= weight:
                # 다음 트럭을 다리로 이동
                truck = truck_weights.popleft()
                bridge.append(truck)
                total_weight += truck
            # 트럭 무게를 초과한다면 다리에 빈 공간 넣기
            else:
                bridge.append(0)
        
    return answer
```

<br>

---
#### 028. 주식가격 (스택/큐 lv.2)
- **문제에서 원하는 것**: 주식 가격이 떨어지지 않는 기간 계산하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 초 단위로 기록된 주식 가격이 담긴 배열 `prices`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - `prices`의 각 가격은 1 이상 10,000 이하의 자연수
    - `prices`의 길이는 2 이상 100,000 이하

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 스택에 가격이 아직 안 떨어진 index를 저장

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. 스택에 “가격이 아직 떨어지지 않은 시점의 인덱스”를 저장  
        ⇒ 스택에는 항상 앞에서 가격이 유지되던 인덱스만 남아있음

    2. 현재 시점에서 가격이 떨어졌는지 확인  
        ⇒ `prices[i] < prices[stack[-1]]`  
        ⇒ 가격이 이전보다 떨어졌다면 스택에서 꺼내고 `i - 꺼낸 인덱스`를 결과에 저장 (해당 시점에서 몇 초 동안 가격이 떨어지지 않았는가를 의미)  
    
    3. 가격이 떨어지지 않았으면 현재 시점의 인덱스를 스택에 `push`   
    
    4. 반복이 끝나도 스택에 남아있는 인덱스가 있다면 `len(prices) - 1 - top`를 저장

```python
def solution(prices):
    answer = [0] * len(prices)
    ### 1. 여기에는 "가격이 아직 떨어지지 않은 시점의 인덱스" 저장
    stack = []
    
    for i in range(len(prices)):
        ### 2. 스택에 값이 남아있고, 가격이 떨어진 경우
        while stack and (prices[i] < prices[stack[-1]]):
            top = stack.pop()
            answer[top] = i - top
        ### 3. 가격이 떨어지지 않았으면 현재 시점의 인덱스 append
        stack.append(i)
    
    ### 4. 끝까지 가격이 떨어지지 않은 경우 (스택에 값이 남아있는 경우)
    while stack:
        top = stack.pop()
        answer[top] = len(prices) - 1 - top
        
    return answer
```

<br><br>