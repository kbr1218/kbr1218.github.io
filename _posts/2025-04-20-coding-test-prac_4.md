---
layout: post
title:  "[Programmers] 코딩테스트 연습문제 4 (완전탐색)"
author: me
categories: [ coding-test ]
date: 2025-04-16 08:00:00
image: https://programmers.co.kr/assets/img-meta-programmers-411e94bf29153dc31004168e6cd500279b1a531a23689303755e51971dee4526.png
---

#### 014. 모의고사 (완전탐색 lv.1)
- **문제에서 원하는 것**: 최고 득점자를 오름차순 리스트로 반환

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 정답이 순서대로 들어있는 배열 `answer`


<p style="margin-top: 2.3em;"></p>

- **조건**
    - 시험은 최대 10,000문제
    - 문제의 정답은 1, 2, 3, 4, 5 중 하나
    - 가장 높은 점수를 받은 사람이 여럿일 경우, return 값을 오름차순 정렬

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 각 수포자의 패턴을 정답 리스트 `answer`와 인덱스 기준으로 비교

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 각 수포자의 패턴 준비

    2. `answer`와 인덱스 기준으로 비교  
        반복되는 패턴이므로 `%` 연산을 써서 반복 처리  
        `pattern[i % len(pattern)]`

    3. 각 수포자의 맞힌 개수 구하기

    4. 가장 높은 점수를 가진 사람을 오름차순 정렬로 반환



```python
def solution(answers):
    # 수포자 패턴
    p1 = [1, 2, 3, 4, 5]
    p2 = [2, 1, 2, 3, 2, 4, 2, 5]
    p3 = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5]
    
    # 각 수포자가 맞힌 개수를 세는 배열 정의
    scores = [0, 0, 0]
    
    for i in range(len(answers)):
        if answers[i] == p1[i % len(p1)]:
            scores[0] += 1
        if answers[i] == p2[i % len(p2)]:
            scores[1] += 1
        if answers[i] == p3[i % len(p3)]:
            scores[2] += 1
    
    # 가장 높은 점수 찾기
    max_score = max(scores)
    
    # 가장 높은 점수를 받은 사람들 반환
    result = []
    for i in range(3):
        if scores[i] == max_score:
            result.append(i + 1)
            
    return result
```

<br>

---
#### 015. 최소직사각형 (완전탐색 lv.1)
- **문제에서 원하는 것**: 모든 명함을 수납할 수 있는 가장 작은 지갑 사이즈 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 모든 명함의 가로 길이와 세로 길이를 나타내는 2차원 배열 `sizes`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - `sizes`의 길이는 1 이상 10,000 이하

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 더 긴 쪽을 가로(`w`) 짧은 쪽을 세로(`h`)라고 생각하기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략 1**:  
    1. `sizes`에서 각 명함을 [긴 쪽, 짧은 쪽]으로 정렬

    2. 각각의 `w`와 `h`에서 최댓값 찾기

    3. 지갑의 크기: `w * h`

```python
def solution(sizes):
    # 최댓값 변수 선언
    max_w = 0
    max_h = 0
    
    for w, h in sizes:
        long_one = max(w, h)
        short_one = min(w, h)
        # 최댓값 찾기
        max_w = max(max_w, long_one)
        max_h = max(max_h, short_one)
        
    return max_w * max_h
```

<br>

---
####  016. 소수 찾기 (완전탐색 lv.2)
- **문제에서 원하는 것**: 숫자에서 소수 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 문자열 숫자 저장된 변수 `numbers`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - `numbers`는 0~9까지 숫자만으로 이루어져 있음
    - `numbers`는 길이 1 이상 7 이하인 문자열
    - 중복된 소수는 한 번만 카운트
    - 숫자는 순서를 바꾸거나 일부만 사용할 수 있음

<p style="margin-top: 2.3em;"></p>

- **접근 방법**:  모든 가능한 숫자 조합을 만들고 소수인 것만 세기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 모든 숫자 조합 생성 (`itertools.permutations()`함수 사용)

    2. 각 숫자를 정수로 바꾸고 소수 판별 (`is_prime` 함수를 정의해서 사용)

    3. 중복된 수를 제거하기 위해 `set` 사용

    4. 소수인 숫자의 수 반환


```python
from itertools import permutations

# 소수 판별 함수 정의
def is_prime(n):
    if n < 2:
        return False
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    return True

def solution(numbers):
    ### 1. 모든 숫자 조합 만들기
    # 입력 문자를 리스트로 변경하기
    digits = list(numbers)
    
    # 모든 자리수의 순열 만들기
    all_combi = set()
    
    for i in range(1, len(digits) + 1):
        for p in permutations(digits, i):
            num = int(''.join(p))   # 문자열로 붙인 뒤 정수 변환
            all_combi.add(num)
    
    ### 2-3. all_combi에 대해 소수 판별
    prime_set  = {num for num in all_combi if is_prime(num)}
    
    ### 4. 소수인 숫자의 개수 반환
    return len(prime_set)
```

<br>

---
#### 017. 카펫 (완전탐색 lv.2)
- **문제에서 원하는 것**: 카펫의 전체 가로, 세로 크기를 [가로, 세로] 형태로 반환

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 테두리에 해당하는 갈색 격자의 수 `brown`
    - 가운데에 위치한 노란색 격자의 수 `yellow`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 카펫의 w는 h와 같거나 h보다 길다

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 전체 넓이에서 가능한 가로-세로 조합 확인하기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략 1**:  
    1. 전체 격자의 수는 `brown + yellow`  
        `brown+yellow`를 `w * h`로 만들 수 있는 모든 조합 찾기
        
    2. 각 `(w, h)` 조합에 대해 가운데 노란색 영역의 크기는 `(w - 2) * (h - 2)` 
        ⇒ 이 값이 `yellow`와 같다면 정답 후보

```python
def solution(brown, yellow):
    answer = []
    ### 1. 전체 격자의 수
    num = brown + yellow
    
    ### 2. 조합 찾기
    # 테두리는 최소 높이 3 이상
    for h in range(3, num + 1):
        # w * h가 되기 위해 나눠 떨어져야 함
        if num % h == 0:
            w = num // h
            # 가로는 세로보다 같거나 길어야 함
            if w >= h:
                if (w - 2) * (h - 2) == yellow:
                    return [w, h]
```

<br>

---
#### 018. 피로도 (완전탐색 lv.2)
- **문제에서 원하는 것**: 유저의 피로도 `k`가 있을 때, 유저가 탐험할 수 있는 최대 던전 수

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 유저의 현재 피로도 `k`
    - 각 던전별 “최소 필요 피로도”와 “소모 필요도”가 담긴 2차원 배열 `dungeons`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 던전은 하루에 한 번씩만 탐험 가능
    - 던전의 개수는 1 이상 8 이하
    - 각 던전은 `["최소 필요 피로도", "소모 피로도"]`로 표현됨
    - `k` 이상이어야 탐험을 시작할 수 있고, 탐험 후에는 소모 피로도만큼 `k`가 줄어듦

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 던전의 모든 순열을 시도해보고, 현재 피로도로 얼마나 많은 탐험이 가능한지 계산

<p style="margin-top: 2.3em;"></p>

- **풀이 전략 1**:  
    1. `itertools.permutations`를 이용해 `dungeons`의 모든 순열 생성  

    2. 각 순서에 대해  
        현재 피로도 `k`로 순서대로 던전 시도  
        탐험이 가능하면 피로도 차감하고 ++  
        탐험이 불가하면 멈추기  
        
    3. 각 순서의 최대 탐험 수 중 최댓값을 반환

```python
from itertools import permutations

answer = []

def solution(k, dungeons):
    max_count = 0
    
    ### 1. dungeons의 모든 순열 생성
    all_orders = list(permutations(dungeons, len(dungeons)))
    
    ### 2. 각 순서에 대해 k로 던전 시도
    for order in all_orders:
        cur_k = k
        count = 0
        
        for minimum, consume in order:
            # 탐험을 하기 위해서는 최소 필요도보다 커야 함
            if cur_k >= minimum:
                # 현재 k에서 소모 필요도 --
                cur_k -= consume
                count += 1
            # 최소 필요도보다 작은 k가 남았다면 break
            else:
                break
        max_count = max(max_count, count)
            
    return max_count
```

<br><br>