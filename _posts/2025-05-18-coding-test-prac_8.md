---
layout: post
title:  "[Programmers] 코딩테스트 연습문제 8"
author: me
categories: [ coding-test ]
date: 2025-05-05 08:00:00
image: https://programmers.co.kr/assets/img-meta-programmers-411e94bf29153dc31004168e6cd500279b1a531a23689303755e51971dee4526.png
---

#### 038. 최댓값과 최솟값 (연습문제 lv.2)
- **문제에서 원하는 것**: 문자열 숫자 중 최솟값 최댓값 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - `s`에는 둘 이상의 정수가 공백으로 구분되어 있다

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 모든 차량이 단속용 카메라를 한 번은 만나도록 하기  

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 공백 기준 문자열 `split`후 최소-최댓값 비교

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 공백 기주능로 문자열 나누기

    2. 문자열 리스트를 정수형으로 변환

    3. `min/max` 함수로 최솟값 최댓값 구하기

```python
def solution(s):
    ### 1. 공백 기준 문자열 나누기
    numbers = s.split(" ")
    ### 2. 정수형으로 변환
    int_numbers = list(map(int, numbers))
    ### 3. 최솟값, 최댓값 구하기
    min_val = min(int_numbers)
    max_val = max(int_numbers)
    return f"{min_val} {max_val}"
```

<br>

---
#### 039. 최솟값 만들기 (연습문제 lv.2)
- **문제에서 원하는 것**: 두 개의 배열의 수를 곱한 후 더한 값을 최솟값 만들기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 자연수로 이루어진 배열 `A`, `B`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 배열 A, B는 길이가 같음
    - 각 배열에서 k번째 숫자를 뽑았다면 다음에는 k번째 숫자를 다시 뽑을 수 없음

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 한 배열의 최솟값과 다른 배열의 최댓값만을 곱해 최소 만들기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    - 최소-최대 사용 방법
        1. A의 길이만큼 반복  
        2. A에서 최솟값, B에서 최댓값 찾아서 곱한 후 그 값 answer++  
        3. 곱한 값은 배열에서 `remove`  

        ⇒ 이렇게 하면 시간복잡도 **O(n^2)**
        
    - 정렬을 사용한 greedy  
        1. A는 오름차순, B는 내림차순 정렬
        2. 같은 인덱스끼리 곱해서 누적합
        
        ⇒ 이렇게 하면 시간복잡도 **O(n log n)**

```python
### 첫 번째 방법
def solution(A,B):
    answer = 0
    
    ### 1. A의 길이만큼 반복
    for i in range(len(A)):
        ### 2. 최솟값 최댓값 찾아 곱하기
        answer += (min(A)) * (max(B))
        ### 3. 사용한 숫자는 제거
        A.remove(min(A))
        B.remove(max(B))

### 두 번째 방법
def solution(A,B):
    A.sort()
    B.sort(reverse=True)
    return sum(a * b for a, b in zip(A, B))
```

<br>

---
#### 040. JadenCase 문자열 만들기 (연습문제 lv.2)
- **문제에서 원하는 것**: 주어진 문자열을 JadenCase로 만들기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 문자열 `s`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 문자열 `s`는 알파벳과 숫자, 공백문자로 이루어짐
    - 숫자는 단어의 첫 문자로만 나옴
    - 공백문자가 연속해서 나올 수 있음

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 문자열 전체를 소문자로 만들고 문자열을 순회하면서 공백 뒤/맨 앞이라면 대문자로 변환

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 문자열 전체를 소문자로 변환

    2. 문자열 길이만큼 순회

    3. 맨 앞 또는 공백 뒤라면 대문자로 변환

```python
def solution(s):
    answer = ''
    cap_next = True
    
    ### 1. 문자열을 소문자로 변환
    s = s.lower()
    
    ### 2. 문자열 길이만큼 순회
    for char in s:
        if cap_next and char.isalpha():
            answer += char.upper()
        else:
            answer += char
        # 공백 뒤라면 대문자로
        cap_next = (char == ' ')
        
    return answer
```

<br>

---
#### 041. 숫자의 표현 (연습문제 lv.2)
- **문제에서 원하는 것**: 연속된 자연수들로 숫자 표현하는 방법의 수 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 자연수 `n`

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: `x`는 자연수여야 하므로 `(n - k(k-1)/2)`가 k로 나누어 떨어져야 함  

    ```math
    n = x + (x+1) + (x+2) + ... + (x+k-1) = kx + (k(k-1))/2
    ```

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. k를 1부터 `k(k-1)/2 < n` 인 동안 반복

    2. `(n - k(k-1)/2) / k` 이 값이 정수면 카운트

```python
def solution(n):
    answer = 0
    k = 1
    
    while (k * (k - 1) // 2) < n:
        num = n - k * (k - 1) // 2
        if num % k == 0:
            answer += 1
        k += 1
        
    return answer
```

<br>

---
#### 042. 다음 큰 숫자 (연습문제 lv.2)
- **문제에서 원하는 것**: 자연수 `n` 의 다음 큰 숫자 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 자연수 `n`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - `n`의 다음 큰 숫자는 `n`보다 큰 자연수
    - `n`의 다음 큰 숫자와 `n`은 2진수로 변환했을 때 1의 갯수가 같음
    - `n`의 다음 큰 숫자는 조건 1, 2를 만족하는 수 중 가장 작은 수

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: `n`보다 큰 수 중 2진수로 변환했을 때 1의 갯수 비교

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. `n`의 2진수에서 1의 개수 먼저 계산

    2. `n`보다 큰 수를 하나씩 증가시키면서 2진수로 변환했을 때 1의 개수 비교

    3. `n`과 같은 1의 개수를 가지면 그 수 반환

```python
def solution(n):
    count = bin(n).count('1')
    
    while True:
        n += 1
        if bin(n).count('1') == count:
            return n
```

<br>

---
#### 043. 피보나치 수 (연습문제 lv.2)
- **문제에서 원하는 것**: `n`번째 피보나치 수를 1234567로 나눈 나머지를 리턴

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 2 이상의 자연수 `n`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - F(n) = F(n-1) + F(n-2)

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 반복문으로 동적 프로그래밍

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. 변수 2개로 이전 계산값 저장

    2. F(0) = 0, F(1) = 1로 초기화

    3. F(2)부터 F(n)까지 반복적으로 계산

    4. 계산할 때마다 `%1234567` 연산

```python
def solution(n):
    if n == 0:
        return 0
    # F(0), F(1)
    a, b = 0, 1
    for _ in range(2, n + 1):
        a, b = b, (a + b) % 1234567
    return b
```

<br>

---
#### 044. 귤 고르기 (연습문제 lv.2)
- **문제에서 원하는 것**: 크기가 서로 다른 종류의 수의 최솟값 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 한 상자에 담으려는 귤의 개수 `k`
    - 귤의 크기를 담은 배열 `tangerine`

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 많이 있는 크기부터 선택하기 (greedy)

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. 귤 크기별 개수 확인

    2. 빈도수 내림차순

    3. 개수 누적하면서 k개 채우기

```python
from collections import Counter

def solution(k, tangerine):
    ### 귤 크기별 개수 세기
    size_count = Counter(tangerine)
    ### 내림차순 정렬
    sorted_sizes = sorted(size_count.values(), reverse=True)
    
    # 고른 귤 개수
    total = 0
    # 종류 수
    answer = 0
    
    for count in sorted_sizes:
        total += count
        answer += 1
        if total >= k:
            break
    return answer
```

<br><br>