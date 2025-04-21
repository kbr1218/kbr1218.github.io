---
layout: post
title:  "[Programmers] 코딩테스트 연습문제 2 (Hash)"
author: me
categories: [ coding-test ]
date: 2025-04-11 08:00:00
image: https://programmers.co.kr/assets/img-meta-programmers-411e94bf29153dc31004168e6cd500279b1a531a23689303755e51971dee4526.png
---

#### 005. 폰켓몬 (Hash lv.1)
- **문제에서 원하는 것**: N/2마리의 폰켓몬 중 가장 많은 종류의 폰켓몬 선택하는 방법 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
   -  폰켓몬의 종류 번호가 담긴 1차원 배열 `nums`  


<p style="margin-top: 2.3em;"></p>

- **조건**
    - `nums`의 길이(N)는 1 이상 10,000 이하의 자연수이며, 항상 짝수로 주어짐
    - 폰켓몬의 종류 번호는 1 이상 200,000 이하의 자연수

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 같은 게 여러 개 있을 때, 최대한 다양한 종류를 고르는 문제 ⇒ 해시(Hash)

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. `set(nums)`으로 중복을 제거해서 종류 수 먼저 파악   

    2. `len(nums) // 2`는 고를 수 있는 최대 마릿수  

    3. 1번과 2번의 최솟값(`min(len(set(nums))), len(nums) // 2)`)으로 최대한 많은 종류의 폰켓몬 선택 가능  


```python
def solution(nums):
    return min(len(set(nums)), len(nums) // 2)
```

<br>

---
#### 006. 완주하지 못한 선수 (Hash lv.1)
- **문제에서 원하는 것**: 마라톤 선수들 중 완주하지 못한 단 한 명의 선수 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 마라톤에 참여한 선수들의 이름이 담긴 배열 `participant`
    - 완주한 선수들의 이름이 담긴 배열 `completion`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000 이하
    - `completion`의 길이는 `participant`보다 1 작다
    - 참가자의 이름은 1 이상 20 이하의 알파벳 소문자로 이루어짐
    - 참가자 중에는 동명이인이 있을 수 있음

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: 동명이인이 있기 때문에 이름의 횟수를 비교해야 함 ⇒ 해시맵

<p style="margin-top: 2.3em;"></p>

- **풀이 전략 1**:  
    1. `collections.Counter`를 사용하여 `participant`와 `completion` 에서 각각 이름별 등장 횟수 세기

    2. `participant`와 `competion`의 차이 구하기

    3. 차이의 key가 완주하지 못한 사람의 이름

```python
    from collections import Counter
    
    def solution(participant, completion):
        p_counter = Counter(participant)
        c_counter = Counter(completion)
        
        result = p_counter - c_counter
    
        return list(result.keys())[0]
```

<p style="margin-top: 2.3em;"></p>

- **풀이 전략 2**:  
    1. `defaultdict` 또는 `dict`로 이름 횟수 세기

    2. `completion`에 있는 이름을 하나씩 감소

    3. 값이 0보다 큰 사람이 완주하지 못한 것

```python
    def solution(participant, completion):
        runner = dict()
        
        ## 1. 참가자 명단 카운트
        for name in participant:
            if name in runner:
                runner[name] += 1
            else:
                runner[name] = 1
        # 2. 완주자 명단에서 하나씩 차감
        for name in completion:
            runner[name] -= 1
        
        # 3. 값이 0보다 큰 사람 찾기
        for name in runner:
            if runner[name] > 0:
                return name
```


<br>

---
#### 007. 전화번호 목록 (Hash lv.2)
- **문제에서 원하는 것**: 어떤 번호가 다른 번호의 접두어인 경우가 있는지 찾기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 전화번호가 담긴 배열 `phone_book`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - `phone_book`의 길이는 1 이상 1,000,000 이하
    - 각 전화번호의 길이는 1 이상 20 이하
    - 같은 전화번호가 중복해서 들어있진 않음

<p style="margin-top: 2.3em;"></p>

- **접근 방법**:  해시를 사용해서 문자열 탐색

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 모든 전화번호를 해시셋(set)에 넣기

    2. 전화번호의 접두어를 잘라서 set에 있는지 확인

    3. 접두사 관계가 있다면 `False`, 없다면 `True` 리턴


```python
def solution(phone_book):
    # 전화번호를 set에 저장
    phone_set = set(phone_book)

    # 전화번호 하나씩 반복하면서
    for num in phone_book:
        # 한 글자씩 잘라서 확인
        for i in range(1, len(num)):
            prefix = num[:i]
            if prefix in phone_set:
                return False
    return True
```

<p style="margin-top: 2.3em;"></p>

- **접근 방법 2**:  전화번호를 정렬하면 접두사 관계가 있는 번호는 인접하게 됨

<p style="margin-top: 2.3em;"></p>

- **풀이 전략 2: 정렬**:  
    1. `sorted`로 전화번호부 정렬

    2. 전화번호를 하나씩 보면서 앞 번호가 뒷 번호의 접두사인지 확인(`startwith`함수)

    3. 찾으면 `False` 리턴, 없으면 `True` 리턴

```python
def solution(phone_book):
    # 전화번호 정렬
    phone_book.sort()
    
    # 정렬 전화번호 하나씩 확인
    for i in range(len(phone_book) - 1):
        # 앞 번호가 뒷 번호의 접두사인지 확인
        if phone_book[i + 1].startswith(phone_book[i]):
            return False
    return True
```

<br>

---
#### 008. 의상 (Hash lv.2)
- **문제에서 원하는 것**: 의상의 이름과 종류가 주어졌을 때, 서로 다른 옷의 조합 수 구하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 의상의 종류가 담긴 2차원 배열 `clothes`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - `clothes`의 각 행은 [의상 이름, 의상 종류]로 이루어져 있음
    - 의상의 수는 1개 이상 30개 이하
    - 같은 이름을 가진 의상은 존재하지 않음
    - 모든 문자열의 길이는 1 이상 20 이하인 자연수, 알파베 소문자 또는 `_`로만 이루어져 있음

- **접근 방법**: 조합의 수를 곱셈으로 구하기 (해시맵 사용)

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 해시맵(`defaultdict`)으로 종류별 의상 개수 세기

    2. 각 종류마다 입을 수 있는 경우의 수 구하기 = 해당 종류의 아이템 수  + 1  
        ▶ 1은 안 입는 경우

    3. 모든 종류의 경우의 수 곱하기

    4. 모든 종류를 안 입는 경우 -1

<p style="margin-top: 3.3em;"></p>

```python
from collections import defaultdict

def solution(clothes):
    ### 1. 기본값이 0인 딕셔너리 정의
    closet = defaultdict(int)
    
    ### 2. 종류별로 개수 세기
    for name, category in clothes:
        closet[category] += 1
    
    answer = 1
    
    ### 3. 모든 종류의 경우의 수 곱하기
    for count in closet.values():
        # count + 1은 입는 경우 + 안 입는 경우
        answer *= (count + 1)
    ### 4. 모든 종류를 안 입는 경우 - 1
    return answer - 1
```

<br><br>