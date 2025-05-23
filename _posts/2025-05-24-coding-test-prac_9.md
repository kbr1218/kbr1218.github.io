---
layout: post
title:  "[Programmers] 코딩테스트 연습문제 9"
author: me
categories: [ coding-test ]
date: 2025-05-24 08:00:00
image: https://programmers.co.kr/assets/img-meta-programmers-411e94bf29153dc31004168e6cd500279b1a531a23689303755e51971dee4526.png
---

#### 045. 멀리뛰기 (연습문제 lv.2)
- **문제에서 원하는 것**: 효진이가 n칸까지 가는 모든 방법의 수를 구해 1234567을 나눈 나머지 리턴 

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 멀리뛰기에 사용될 칸의 수 `n`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 효진이는 한 번에 1칸, 또는 2칸을 뛸 수 있음  

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: `n`칸에 도착하는 방법은 `n-1`번째 칸에서 1칸 점프하거나 `n-2`번째 칸에서 2칸 점프하는 경우밖에 없음 ⇒ 피보나치 수열 이용하면 됨

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 조건 설정  
        `n=1`일 때 ⇒ 방법 1가지  
        `n=2`일 때 ⇒ 방법 2가지 (1+1, 2)
        
    2. 점화식 세우기  
        `dp[i] = dp[i-1] + dp[i-2]`  
        효진이가 i번째 칸에 도달하는 방법의 수는 i-1번째 칸에서 1칸 뛴 경우, i-2번째 칸에서 2칸 뛴 경우임  
        
    3. 나머지 계산  
        각 단계마다 dp[i] %= 1234567 하기 (수가 커지지 않게)  
        
    4. DP 배열을 이용한 반복문 구현  
        3부터 n까지 반복하면서 `dp[i]`를 점화식으로 채움  

```python
def solution(n):
    dp = [0] * (n + 1)
    dp[1] = 1
    
    if n >= 2:
        dp[2] = 2
    
    for i in range(3, n + 1):
        dp[i] = (dp[i - 1] + dp[i - 2]) % 1234567
    return dp[n]
```

<br>

---
#### 046. N개의 최소공배수 (연습문제 lv.2)
- **문제에서 원하는 것**: n개의 수의 최소공배수 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - n개의 숫자를 담은 배열 `arr`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - `arr`는 길이 1 이상, 15 이하인 배열

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 두 개의 숫자씩 최소공배수를 구해 모든 수의 최소공배수 찾기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 최대공약수 `math.gcd`를 사용해 최소공배수 찾기  

    2. 배열의 모든 원소에 대해 반복  
        ⇒ 처음 두 수의 최소공배수 구하고  
        ⇒ 그 결과랑 다음 수의 최소공배수 구하고  
        ⇒ 이런식으로 배열 전체에 대해 실행  

```python
from math import gcd

# 최대공약수를 사용해 최소공배수를 찾는 함수 정의
def lcm(a, b):
    return a * b // gcd(a, b)

def solution(arr):
    answer = arr[0]
    for num in arr[1:]:
        answer = lcm(answer, num)
    return answer
```

<br>

---
#### 047. 연속 부분 수열 합의 개수 (연습문제 lv.2)
- **문제에서 원하는 것**: 원형 수열의 연속 부분 수열 합으로 만들 수 있는 수의 개수 리턴

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 원형 수열의 모든 원소가 담긴 배열 `element`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 3 ≤ `elements`의 길이 ≤ 1,000
    - 1 ≤ `elements`의 원소 ≤ 1,000

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 접근 방법: 가능한 모든 길이의 연속 부분 수열을 만들고, 합을 set에 저장해서 중복 제거하기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 결과 저장할 `set` 만들기

    2. 원형 수열을 다루기 위해 배열을 두 배로 늘리기

    3. 부분 수열의 길이를 1부터 `len(element)`까지 순회

    4. 각 길이마다 가능한 시작점을 순회하며 부분 수열 자르기  
        ⇒ 길이가 3이면 [7, 9, 1], [9, 1, 1], [1, 1, 4]
        
    5. 부분 수열의 합을 구해서 `set`에 저장

    6. (중복 제거된) `set`의 크기 리턴

```python
def solution(elements):
    n = len(elements)
    # 원형 수열을 다루기 위한 배열 * 2
    elements = elements * 2
    answer = set()
    # 부분 수열의 길이: 1부터 n까지
    for length in range(1, n + 1):
        # 시작 인덱스: 0부터 n-1까지
        for start in range(n):
            # 부분수열의 함
            part_sum = sum(elements[start:start + length])
            answer.add(part_sum)
    return len(answer)
```

<br>

---
#### 048. 할인 행사 (연습문제 lv.2)
- **문제에서 원하는 것**: 원하는 제품을 모두 할인받을 수 있는 회원등록 날짜의 총 일수 리턴

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 원하는 제품을 나타내는 문자열 배열 `want`
    - 원하는 제품의 수량을 나타내는 정수 배열 `number`
    - 마트에서 할인하는 제품을 나타내는 문자열 배열 `discount`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 회원 자격은 10일간 유효함
    - 가능한 날이 없으면 0 return

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 할인 품목 리스트에서 10일 연속 구간을 확인하면서 원하는 제품과 수량이 정확히 일치하는지 검사

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. 구매 목록을 딕셔너리로 변환  

    2. 할인 목록(`discount`)에서 길이 10의 윈도우 순회  
        ⇒ `for i in range(len(discount) - 9)`
        
    3. i부터 i+10까지의 품목을 슬라이스하고, 그 안의 품목 개수 카운트

    4. 카운트한 값과 정현이의 원하는 품목이 같은지 확인      
        ⇒ 두 값이 같다면 회원가입이 가능한 날이므로 ++
        
    5. 모든 구간을 검사한 후 가능한 날짜 수 리턴

```python
from collections import Counter

def solution(want, number, discount):
    ### 1. 구매 목록을 딕셔너리로 변환
    want_dict = dict(zip(want, number))
    answer = 0
    
    ### 2. 할인 목록 순회
    for i in range(len(discount) - 9):
        ### 3. 품목 개수 카운트
        window = discount[i: i + 10]
        window_count = Counter(window)
        
        ### 4. 카운트한 값과 구매 목록 비교
        if window_count == want_dict:
            answer += 1

    return answer

```

<br>

---
#### 049. 행렬의 곱셈 (연습문제 lv.2)
- **문제에서 원하는 것**: 행렬의 곱

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 2차원 행렬 `arr1`, `arr2`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 행렬 arr1, arr2의 행과 열의 길이는 2 이상 100 이하입니다.
    - 행렬 arr1, arr2의 원소는 -10 이상 20 이하인 자연수입니다.
    - 곱할 수 있는 배열만 주어집니다

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: `arr1`의 열 개수 == `arr2`의 행 개수를 만족하는 행렬 곱셈

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. 행렬의 크기  
        ⇒ `arr1`의 행 수 × `arr2`의 열 수
        
    2. 3중 for문을 사용하여 계산  
        ⇒ `arr1`의 행 순회  
        ⇒ `arr2`의 열 순회  
        ⇒ 곱해서 더할 요소를 순회  

```python
def solution(arr1, arr2):
    answer = []
    for i in range(len(arr1)): # 행
        row = []
        for j in range(len(arr2[0])): # 열
            sum_val = 0
            for k in range(len(arr2)): # 곱셈 대상
                sum_val += arr1[i][k] * arr2[k][j]
            row.append(sum_val)
        answer.append(row)
    return answer
```

<br>

---
#### 050. 롤케이크 자르기 (연습문제 lv.2)
- **문제에서 원하는 것**: 롤케이크를 공평하게 자르는 방법의 수 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 롤케이크에 올려진 토핑들의 번호를 저장한 정수 배열 `topping`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 롤케이크의 조각 크기와 토핑 개수에 상관 없이 각 조각에 동일한 가짓수의 토핑이 올라가면 공평하게 롤케이크가 나누어진 것으로 생각

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 한 번만 순회하면서 토핑의 각 위치에서 왼쪽과 오른쪽의 토핑 종류 수를 확인 

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. 전체 토핑 개수를 `Counter`로 저장   
        ⇒ 이건 처음에 오른쪽의 전체 토핑을 간주
        
    2. 왼쪽에 토핑을 하나씩 추가해가고, 오른쪽 토핑은 하나씩 제거하면서 각 위치에서 왼쪽/오른쪽에 있는 토핑 종류 수 비교

    3. 같으면 ++

```python
from collections import Counter

def solution(topping):
    right_counter = Counter(topping)
    left_set = set()
    answer = 0
    
    for t in topping:
        # 왼쪽 조각에 추가
        left_set.add(t)
        
        # 오른쪽 조각에서 제거
        right_counter[t] -= 1
        if right_counter[t] == 0:
            del right_counter[t]
        
        # 종류 수가 같으면 공평하게 나누기
        if len(left_set) == len(right_counter):
            answer += 1
    return answer
```

<br>

---
#### 051. 뒤에 있는 큰 수 찾기 (연습문제 lv.2)
- **문제에서 원하는 것**: 배열의 모든 원소에 대한 뒷 큰수를 차례로 담은 배열 리턴

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 정수 배열 `numbers`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 자신보다 뒤에 있는 숫자 중 자신보다 크면서 가장 가까이 있는 수를 뒷 큰수라고 함

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 뒤에서부터 역순으로 순회하면서 스택을 사용하기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**: 
    1. 오른쪽 끝에서 왼쪽으로 배열 순회

    2. 스택에는 **자기보다 뒤에 있는 수들 중 유력한 뒷 큰수 후보들**이 들어감

    3. 현재 숫자보다 작거나 같은 수는 스택에서 제거 (뒷 큰수가 될 수 없기 때문)

    4. 스택의 top이 현재 원소의 뒷 큰수가 됨

    5. 스택에 현재 숫자를 넣고 다음 숫자로 넘어가기 반복…

```python
def solution(numbers):
    answer = [-1] * len(numbers)
    stack = []
    
    for i in range(len(numbers) - 1, -1, -1):
        # 스택에서 현재 숫자보다 작거나 같은 것 제거
        while stack and stack[-1] <= numbers[i]:
            stack.pop()
        
        # 스택이 비어있지 않다면 top이 뒷큰수
        if stack:
            answer[i] = stack[-1]
        
        # 현재 숫자를 스택에 push
        stack.append(numbers[i])
    return answer
```

<br><br>