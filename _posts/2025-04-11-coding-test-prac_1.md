---
layout: post
title:  "[Programmers] 코딩테스트 연습문제 1 (Greedy)"
author: me
categories: [ coding-test ]
date: 2025-04-11 06:00:00
image: https://programmers.co.kr/assets/img-meta-programmers-411e94bf29153dc31004168e6cd500279b1a531a23689303755e51971dee4526.png
---

#### 001. 체육복 (Greedy lv.1)
- **문제에서 원하는 것**: 최대한 많은 학생이 수업을 듣기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
   -  전체 학생 수 `n`
   - 체육복을 도난당한 학생 배열 `lost`
   - 여벌 체육복을 가진 학생 배열 `reserve`  

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 여벌이 있는 학생이 도난당했을 수도 있음
    - 도난당한 학생은 체육복이 없으므로 수업을 들을 수 없음
    - 단, 여벌이 있으면 자기가 입고 남은 게 없으므로 빌려줄 수는 없음
    - 앞번호나 뒷번호 학생한테만 빌려줄 수 있음

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: greedy. 지금 당장 체육복이 필요한 학생에게 빌려주는 방식

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 여벌이 있으면서 도난을 당했다면 `reserve`와 `lost` 배열 둘 다에 속함  
         ⇒ 얘는 여벌 하나를 자기한테 써야 하니 빌려줄 수 없음  
         ⇒ `reserve`와 `lost` 배열에서 중복된 번호를 제거하기!
        
    2. 정리된 `reserve` 배열을 순회하면서  
        ⇒ 바로 앞 번호(-1)에 체육복이 없는 사람이 있으면 빌려주기  
        ⇒ 아니면 뒷 번호(+1)에게 빌려주기
        
    3. 최종적으로 체육복이 없는 학생 수를 빼서 수업을 들을 수 있는 학생 수를 계산

```python
def solution(n, lost, reserve):
	### 1. 중복 제거 (여벌이 있으면서 도난 당한 학생)
	
	# 집합 연산자를 사용해서 중복 제거
	lost_set = set(lost) - set(reserve)
	reserve_set = set(reserve) - set(lost)
	
	### 2. reserve_set을 순회하면서 빌려주기
	# sorted로 정렬
	for i in sorted(reserve_set):
		# 앞 번호 학생이 도난당한 학생이라면
		if i - 1 in lost_set:
			# 앞 번호 학생에게 빌려주기
			lost_set.remove(i - 1)
		# 앞 번호 학생은 체육복이 있고, 뒷 번호 학생이 도난당했다면
		elif i + 1 in lost_set:
			# 뒷 번호 학생에게 빌려주기
			lost_set.remove(i + 1)
	
	### 3. 전체 학생에서 체육복을 빌리지 못한 학생 수 빼기
	return n - len(lost_set)
```

<br>

---
#### 002. 구명보트 (Greedy lv.2)
- **문제에서 원하는 것**: 구명보트를 최대한 적게 사용하여 모든 사람을 구출하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 사람들의 몸무게를 담은 배열 `people`
    - 구명보트의 무게 제한 `limit`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - 구명보트는 한 번에 최대 두 명만 탑승이 가능함.
    - 무인도에 갇힌 사람은 1명 이상 50,000명 이하.
    - 각 사람의 몸무게는 40kg 이상 240kg 이하.
    - 구명보트의 무게 제한(`limit`)은 사람들의 몸무게 중 최댓값보다 크게 주어짐.

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: greedy. “몸무게가 가장 무거운 사람 + 가장 가벼운 사람”을 최대한 같이 태우고, `limit`을 넘는다면 무거운 사람 단독으로 태우기

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:  
    1. `people` 배열 정렬하기 (내림차순)  

    2. 양쪽 포인터를 사용하여 인덱스 정의  
        `light`: 가벼운 사람 (끝 인덱스)   
        `heavy`: 무거운 사람 (시작 인덱스)  

    3. 가벼운 사람과 무거운 사람의 합이 `limit`보다 작으면 태우고, 아니면 무거운 사람 먼저 태우기  
         `people[light] + people[heavy] <= limit`   
               ⇒ 둘이 같이 탈 수 있으니깐 `light -= 1` & `heavy += 1`  
         `people[light] + people[heavy] > limit`  
               ⇒ 무거운 사람만 태우기 `heavy += 1`  

    4. 3번을 반복하면서 보트 개수 ++  
        양쪽에서 인덱스를 이동시키며 비교하는 방식이므로 `while` 반복문 사용  
        `heavy` 인덱스가 `light`를 넘어선다면 반복문 종료하기

```python
def solution(people, limit):
		# 보트 개수 answer
    answer = 0
    
    ### 1. people 배열 정렬
    sorted_people = sorted(people, reverse=True)
    # oder `people.sort(reverse=True)`
    
    ### 2. light & heavy 인덱스 정의
    light = len(sorted_people) - 1
    heavy = 0
    
    ### 3-1. 보트에 태우기
    while heavy <= light:
		    # 무거운 사람 + 가벼운 사람이 limit보다 작다면
		    if (sorted_people[heavy] + sorted_people[light]) <= limit:
				    # 인덱스 이동 & 둘이 같이 태우고 answer++
				    light -= 1
				    heavy += 1
				    answer += 1
				# limit을 넘는다면 무거운 사람만 먼저 태우기
				else:
						heavy += 1
						answer += 1
		
		### 3-2. 더 짧은 버전 (반복된 코드 줄이기)
		while heavy <= light:
				if (sorted_people[heavy] + sorted_people[light]) <= limit:
						light -= 1
				
				# if문의 결과에 상관없이 실행하는 코드의 중복 제거
				heavy += 1
				answer += 1
		
    return answer
```

<br>

---
#### 003. 조이스틱 (Greedy lv.2)
- **문제에서 원하는 것**: 조이스틱을 최소로 조작하여 이름 완성하기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 만들고자 하는 이름 `name`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - `name`의 길이는 1 이상 20 이하.
    - 모든 문자는 A로 설정되어 있음 (e.g. JAZ라면 “AAA”)

<p style="margin-top: 2.3em;"></p>

- **접근 방법**: greedy. 조이스틱을 최소한으로만 움직여서 원하는 문자열 만들기
    - 각 알파벳을 바꾸는 조작은 🔼 또는 🔽 중 더 적은 횟수로 선택
    - 이름 중간에 “A”가 있다면 커서도 최소한으로 이동하는 방식 선택

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 각 자리에서 알파벳 변경 횟수 계산  
        “A”에서 원하는 알파벳으로 바꾸기 위해 조이스틱 🔼 🔽를 몇 번 움직여야 하는지 계산  
        char를 유니코드 정수값으로 바꿔주는 파이썬 내장함수 `ord()` 사용  

        <p style="margin-top: 0.8em;"></p>

        (원하는 알파벳 - A)와 (Z - 원하는 알파벳 + 1) 중 최솟값  
            ⇒ Z에서 A로 이동하는 한 번을 더해줘야 함  
            `min( ord(char) - ord('A'), ord('Z') - ord(char) + 1)`

    2. 커서 이동의 최소 횟수 계산  
        이름에 연속된 A가 많을 경우 우회해서 돌아가는 게 더 빠를 수도 있음  
        기본값: 오른쪽(▶️)으로 끝까지 이동  
            - `len(name) - 1`
        연속된 A 구간이 있다면 왼쪽/오른쪽 중 최소 경로 선택  

    3. 최종 조작 횟수: 알파벳 변경 횟수 + 커서 이동 최소 횟수


```python
def solution(name):
    # 위/아래 이동 횟수 변수
    up_down = 0
    
    ### 1. 각 자리에서 알파벳 변경 횟수 계산
    for char in name:
        min_mv = min(ord(char) - ord('A'), ord('Z') - ord(char) + 1)
        up_down += min_mv
    
    ### 2. 커서 이동의 최소 횟수 계산
    # 기본값 설정 (그냥 오른쪽으로만 가는 경우)
    min_move = len(name)
        
    for i in range(len(name)):
        # 다음 글자의 인덱스 변수 선언
        next_idx = i + 1
        
        # 이름 길이보다 작으면서 다음 글자가 "A"일동안 반복
        while next_idx < len(name) and name[next_idx] == "A":
            next_idx += 1
        
        # 왼쪽 오른쪽 중 최솟값 계산
        # (i번째 자리까지 갔다가 다시 첫 알파벳으로 이동: i * 2)
        # (첫번째 자리에서 반대 방향으로 next_idx 자리로 이동: len(name) - next_idx)
        distance = min(i * 2 + (len(name) - next_idx),
                       # 뒷부분을 먼저 처리하는 경우
                       # (뒷부분을 처리하고 오는 경로: (len(name) - next_idx) * 2
                       # 처리하지 않은 앞부분까지 이동: + i
                       (len(name) - next_idx) * 2 + i)
        
        min_move = min(min_move, distance)
            
    answer = up_down + min_move
    return answer
```

<br>

---
#### 004. 큰 수 만들기 (Greedy lv.2)
- **문제에서 원하는 것**: 어떤 숫자에서 k개의 수를 제거하여 가장 큰 수 만들기

<p style="margin-top: 2.3em;"></p>

- **변수 확인**
    - 숫자 `number`
    - 제거할 개수 `k`

<p style="margin-top: 2.3em;"></p>

- **조건**
    - k는 1 이상 `number`의 자릿수 미만의 자연수

- **접근 방법**: greedy. 가장 큰 자릿수에 가장 큰 수 남기기
    - 앞에서부터 숫자를 확인하면서 현재 숫자가 이전 숫자보다 크다면 이전 숫자 제거  
        ⇒ 이렇게 k개의 수를 제거하면 가장 큰 수를 만들 수 있음

<p style="margin-top: 2.3em;"></p>

- **풀이 전략**:
    1. 스택 사용  
        앞에서부터 스택에 숫자를 하나씩 넣으면서  
        `top`보다 현재 숫자가 크면 `top`을 제거 (k번 반복)

    2. 제거 횟수가 k만큼 채워지면 남은 숫자는 순서대로 스택에 넣기

    3. 스택에 남은 숫자 중 마지막 `len(number) - k`개 만큼 잘라서 반환  
        스택의 모든 `top`이 앞의 숫자보다 커서 `pop`이 안되는 경우에는 k번을 제거하지 못한 상태로 순회가 끝날 수 있음  
        k개를 자르지 못하고 끝난다면, 인덱싱으로 `len(number) - k` 문자열만 return

<p style="margin-top: 3.3em;"></p>

```python
def solution(number, k):
    st = []
    pop_num = 0
    
    ### 1. 스택을 사용하여 가장 큰 수 완성하기
    for i in number:
        # stack에 값이 있고 top이 i보다 작다면 pop 반복
        while st and pop_num < k and st[-1] < i:
            st.pop()
            pop_num += 1
        st.append(i)
            
    # 반복문 이후에 pop의 횟수가 k보다 작다면
    if pop_num < k:
        # 스택의 앞에서부터 len - k만 남기기
        st = st[:(len(st) - (k - pop_num))]
    
    answer = ''.join(st)
    return answer
```

<br><br>