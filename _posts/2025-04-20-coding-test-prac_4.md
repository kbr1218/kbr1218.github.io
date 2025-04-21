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

(작성중)


<br><br>