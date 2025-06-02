---
layout: post
title:  "[Programmers] 코딩테스트 연습문제 10"
author: me
categories: [ coding-test ]
date: 2025-06-02 08:00:00
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
#### 053. 택배상자 (연습문제 lv.2)
- **문제에서 원하는 것**: 트럭에 실을 수 있는 최대 상자의 개수 구하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 택배 기사가 원하는 상자 순서를 나타내는 정수 배열 `order`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 트럭에는 `order` 배열의 순서대로 상자를 실어야 한다
    - 보조 벨트에서 가장 마지막 상자(스택의 top)만 꺼낼 수 있따
    - 원하는 순서대로 실을 수 없다면 멈춰야 함

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 컨베이어 벨트: 1부터 n까지 자연수 순으로 진행, 보조벨트는 스택, 순서대로 상자를 비교하며 시뮬레이션 수행

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. `cur` 변수를 만들어서, 현재 컨베이어 벨트에서 오는 상자를 1부터 n까지 처리

    2. `order[idx]`가 현재 트럭에 실어야 할 상자라고 가정하면

    3. 매번 `cur`과 `order[idx]`를 비교하면서  
        ⇒ 같으면 바로 실음 idx++  
        ⇒ 작으면 보조벨트 (stack)에 push (cur++)  
        ⇒ 크면 보조벨트에서 pop이 가능한지 확인  
        
    4. 보조벨트의 top이 `order[idx]` 와 같으면 pop해서 트럭에 싣고 idx++

    5. 할 수 있는 게 없으면 루프 종료

```python
def solution(order):
    stack = []
    cur = 1
    answer = 0
    n = len(order)
    
    while cur <= n:
        if order[answer] == cur:
            cur += 1
            answer += 1
        else:
            if stack and stack[-1] == order[answer]:
                stack.pop()
                answer += 1
            else:
                stack.append(cur)
                cur += 1
    # 컨베이어 벨트가 끝났으면 남은 거 스택에서 처리
    while stack and stack[-1] == order[answer]:
        stack.pop()
        answer += 1
    
    return answer
```

<br>

---
#### 054. 숫자 변환하기 (연습문제 lv.2)
- **문제에서 원하는 것**: 자연수 `x`를 `y`로 변환하기 위해 필요한 최소 연산 횟수 구하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 자연수 `x`, `y`, `n`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 사용 가능한 연산  
        1. `x`에 `n` 더하기  
        2. `x*2`  
        3. `x*3`  

    - `x`를 `y`로 만들 수 없다면 -1 return

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 최소 연산 횟수를 구하는 것 ⇒ 최단 경로 문제로 풀기 ⇒ BFS

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. `deque`를 사용한 BFS 큐를 초기화하고 `(x, 0)` 부터 시작

    2. 각 연산을 통해 아래 상태를 만들기  
        ⇒ x + 1  
        ⇒ x * 2  
        ⇒ x * 3
        
    3. 위 연산으로 만든 값이 `y`보다 작거나 같고, 아직 방문하지 않았다면 큐에 넣기

    4. 큐에서 꺼낸 값이 `y`라면 해당 시점의 연산 횟수 반환

    5. 끝까지 돌았는데 `y`에 도달하지 않았다면 `-1` 반환

```python
from collections import deque

def solution(x, y, n):
    visited = set()
    queue = deque([(x, 0)])
    
    while queue:
        current, cnt = queue.popleft()
        
        if current == y:
            return cnt
        
        for next_num in (current + n, current * 2, current * 3):
            if next_num <= y and next_num not in visited:
                visited.add(next_num)
                queue.append((next_num, cnt + 1))
    return -1
```

<br>

---
#### 055. 연속된 부분 수열의 합 (연습문제 lv.2)
- **문제에서 원하는 것**: 비내림차순으로 정렬된 수열에서, 조건을 만족하는 부분 수열 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 수열을 타나내는 정수 배열 `sequence`
    - 부분 수열의 합을 나타내는 정수 `k`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 기존 수열에서 임의의 두 인덱스의 원소와 그 사이의 원소를 모두 포함하는 부분 수열이어야 함
    - 부분 수열의 합은 `k`
    - 합이 `k`인 부분 수열이 여러 개인 경우 길이가 짧은 수열을 찾음
    - 길이가 짧은 수열이 여러 개인 경우 앞쪽(시작 인덱스가 작은)에 나오는 수열을 찾음

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 포인터를 이용해 구간을 확장/축소하면서 합 조절하기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. `start`, `end` 정의하고 초깃값은 0으로 설정, `sum=0`

    2. sum이 k보다 작으면 `end`를 늘려 구간 확장

    3. sum이 k보다 크면 `start`를 늘려 구간 축소

    4. `sum == k`일 때:  
        ⇒ 지금까지 찾은 구간보다 길이가 짧거나 길이가 같다면 `start` 인덱스가 더 작으면 `best_start`, `best_end` 갱신
        
    5. 포인터가 배열 끝에 도달할 때까지 반복

```python
def solution(sequence, k):
    start = 0
    end = 0
    total = sequence[0]
    n = len(sequence)
    
    # 초깃값은 최대한 긴 구간으로 설정
    best = (0, n)
    
    while start <= end and end < n:
        if total < k:
            end += 1
            if end < n:
                total += sequence[end]
        
        elif total > k:
            total -= sequence[start]
            start += 1
        # total == k
        else:
            if end - start < best[1] - best[0]:
                best = (start, end)
            start += 1
            if start <= end:
                total -= sequence[start - 1]
            else:
                end = start
                if end < n:
                    total = sequence[end]
            
    return list(best)
```

<br>

---
#### 056. 마법의 엘리베이터 (연습문제 lv.2)
- **문제에서 원하는 것**: 최소한의 마법의 돌을 사용해서 0층으로 이동하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 현재 민수가 있는 층의 수 `storey`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - -1, +1, -10, +10, -100, +100 등과 같이 절댓값이 10c (c ≥ 0 인 정수)인 값을 더한 층으로 이동
    - 엘리베이터가 위치해 있는 층과 버튼의 값을 더한 결과가 0보다 작으면 엘리베이터는 움직이지 않음
    - 0층이 가장 아래층

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 각 자리수에서 내려가는 거랑 반대로 올렸다가 다음 자리에서 조정하는 것 중 하나 선택하기 (greedy)

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. 현재 수의 1의 자리부터 확인

    2. 내릴지 올릴지 결정  
        ⇒ 0~4는 그냥 내리고 (버튼 수 += 현재 수)  
        ⇒ 6~9는 올림 후 다음 자리로 넘기기  
        ⇒ 5: 다음 자리수가 5 이상이면 올림, 아니면 내림  
        
    3. 올림 발생 시 상위 자리수에 +1

    4. 모든 자리를 순회하고 남은 올림이 있다면 추가

```python
def solution(storey):
    answer = 0
    
    while storey > 0:
        digit = storey % 10
        next_digit = (storey // 10) % 10
        
        if digit < 5:
            answer += digit
            storey //= 10
        elif digit > 5:
            answer += (10 - digit)
            storey = storey // 10 + 1
        # digit == 5
        else: 
            if next_digit >= 5:
                answer += 5
                storey = storey // 10 + 1
            else:
                answer += 5
                storey //= 10
    return answer
```

<br>

---
#### 057. 2 x n 타일링 (연습문제 lv.2)
- **문제에서 원하는 것**: 직사각형 타일로 바닥을 채우는 방법의 수

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 직사각형 가로의 길이 `n`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 타일은 가로 2, 세로 1
    - 세로의 길이 2, 가로의 길이 `n`인 바닥 채우기
    - 경우의 수가 많아 질 수 있으므로, 경우의 수를 1,000,000,007으로 나눈 나머지를 return

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 점화식 기반 DP 문제로 풀기, 가능한 모든 타일 배치 수를 구하는 건 피보나치 수열이랑 비슷

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
1. `dp[i]`=길이 `i`일 때 타일을 채우는 방법 수

2. `dp[n] = dp[n-1] + dp[n-2]`      
    ⇒ 마지막에 세로 타일 하나 더 붙이면 `n-1` 길이 문제  
    ⇒ 마지막에 가로 타일 두개를 더 붙이면 `n-2` 길이 문제
    
3. 초기값 설정  
    ⇒ `dp[1] = 1`   
    ⇒ `dp[2] = 2`
    
4. `mod = 1000000007` 적용

```python
def solution(n):
    MOD = 1_000_000_007
    if n == 1:
        return 1
    # dp[1], dp[2]
    a, b = 1, 2
    
    for _ in range(3, n+1):
        a, b = b, (a + b) % MOD
    return b
```

<br>

---
#### 058. 시소 짝꿍 (연습문제 lv.2)
- **문제에서 원하는 것**: 시소 짝꿍이 될 수 있는 쌍의 개수 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 사람들의 몸무게 목록 `weights`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 2m, 3m, 4m 거리의 지점에 좌석이 하나씩 있음
    - 탑승한 사람의 무게와 시소 축과 좌석 간의 거리의 곱이 같다면 시소 짝꿍  
      ⇒ `A의 몸무게 × A의 거리 = B의 몸무게 × B의 거리`

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 어떤 사람 A에 대해 토크가 같은 B 찾기 ⇒ 해시맵(`Counter`)을 사용해서 중복 탐색 없이 계산

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
1. 가능한 거리 비율 조합 정의  
    ⇒ `[(1,1), (2,3), (2,4), (3,4)]`  
    ⇒ 이걸로 만들어지는 몸무게 비율 확인
    
2. 사람을 한 명씩 순회하면서 현재까지 나온 사람들의 몸무게로 만들 수 있는 짝꿍 수 누적 계산

3. `Counter`를 사용해서 누적 등장 수 기록하기

```python
from collections import Counter
from fractions import Fraction

def solution(weights):
    answer = 0
    count = Counter()
    
    for w in weights:
        for ratio in [1, Fraction(2,3), Fraction(3,2), Fraction(3,4), Fraction(4,3), Fraction(2,4), Fraction(4,2)]:
            pair_weight = w * ratio
            if pair_weight in count:
                answer += count[pair_weight]
        count[w] += 1
    return answer
```

<br><br>