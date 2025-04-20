---
layout: post
title:  "[Kaggle Gen AI] Day 1 - Prompt Engineering 시작하기: 출력 제어의 기술 🚀"
author: me
categories: [ Kaggle Gen-AI ]
date: 2025-04-13 09:00:00
image: assets/images/20250402/day1_2.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_9/)까지는 LLM에 대한 전반적인 개념을 훑어봤고,  

이번 글부터는 **프롬프트 엔지니어링**<small style="color:gray">(Prompt Engineering)</small>으로 Unit 1 시작!  

팟캐스트에서는 프롬프트 엔지니어링을 **LLM을 내가 원하는 대로 정확히 움직이게 만드는 기술**이라고 표현했다. 
> “Prompt engineering — that art of getting large language models to do exactly what you want them to do.”  


[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=CFtX0ZyLSAY&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=2)

<br>

---
### 🧭 Unit 1의 미션: 프롬프트 엔지니어링 기술 익히기
이번 유닛의 목표는 **LLM을 원하는 대로 움직이게 만드는 프롬프트 엔지니어링의 핵심 기법들**을 익히는 것이다.  

기본 개념부터, Chain of Thought, ReAct 등 **고급 프롬프트 전략들**까지 깊이 있게 탐구할 예정.  

오늘은 프롬프트 엔지니어링으로 **모델이 어떤 출력을 하게 할지 컨트롤하는 방법**인 **출력 제어**<small style="color:gray">(Output Configuration)</small>에 대해 정리해봅시다.


<br>

---
### ⚙️ 출력 제어의 기본: 단순한 입력 그 이상
프롬프트를 잘 짜는 것도 중요하지만, 그보다 먼저 알아야 할 건 **LLM의 출력이 어떤 방식으로 결정되는지**다.
> configuring the LLM's output is not just what you put in - it's how you shape what comes out.  

우리가 생각하는 것보다 훨씬 많은 부분을 직접 제어할 수 있다.  
이번에는 그중에서도 대표적인 출력 제어 방식인  
✔️ **출력 길이 설정**,  
✔️ **샘플링 조절**  
에 대해 알아본다.  

<div style="margin-top: 3.3em;"></div>

##### 1️⃣ 출력 길이 설정 (Output Length)
가장 단순해 보이지만, **모델이 생성하는 토큰의 수**는 **비용, 처리 시간, 시스템 한계**에 영향을 주는 아주 현실적인 요소다.  

특히 kaggle notebook처럼 **출력 제한이 있는 상황**이라면 더 중요해진다.  
단순히 토큰 제한을 낮게 설정하는 것만으로는 부족할 수 있고, 프롬프트 자체를 **더 타이트하게 설계**해야 한다.  

예를 들어, **ReAct** 방식처럼 **빠른 반복이 필요한 구조**에서는 응답을 짧고 명확하게 유도하는 것이 핵심이 된다.  

<div style="margin-top: 3.3em;"></div>

---
##### 2️⃣ 샘플링 제어 (Sampling Controls)
LLM이 **다음 단어를 어떻게 선택하는지**, 그 과정을 바꿔가면서 **출력 스타일, 다양성, 정확도**를 조절할 수 있다.

<div style="margin-top: 3.3em;"></div>

##### 🔸 Temperature
> think of temperature like a dial that controls Randomness.

🔽 낮은 temperature
  - 가장 확률이 높은 단어들만 골라서 **예측 가능한 결과**<small style="color:gray">(predictable deterministic output)</small>를 만든다.  

    > it's almost like getting the model's best guess.
  - e.g. `import pandas as pd`처럼 정확한 문법으로 특정한 코드를 생성하고 싶을 때

<div style="margin-top: 2.3em;"></div>

🔼 높은 temperature
  - **다양하고 창의적인 결과를 유도**한다. 
  - 아이디어 브레인스토밍이나 새로운 접근을 탐색할 때 매우 효과적이다. 

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 정확도를 위해서는 낮은 temp, 창의성을 위해서는 높은 temp
</div>
<br>

---
##### 🔸 Top-K & Top-P — 후보군 필터링
- **Top-K**
  - 가장 **가능성 높은 K개 단어**만 후보로 고려한다.
  - K=1이면 사실상 **가장 확률 높은 단어**만 선택
  - K가 높아질수록 더 많은 다양성을 고려한다. 

<div style="margin-top: 2.3em;"></div>

- **Top-P** <small style="color:gray">(Nucleus Sampling)</small>
  - 전체 **확률의 누적합이 P 이상**인 최소 단어 집합만 고려한다.
  - P=0이면 극도로 결정적인 결과 반환
  - P=1이면 거의 모든 단어가 포함되어 **랜덤성이 증가**한다. 

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 top-K = 고정된 개수, top-P = 누적 확률 기준
</div>
<br>

---
##### 🔧 그럼 이 설정들은 어떻게 함께 작동할까?
셋 다 설정되어 있는 경우, 아래와 같은 순서로 동작한다:
1. 먼저 **top-K와 top-P로 후보 단어를 필터링**하고  
2. 그 중에서 **temperature에 따라 무작위로 최종 선택**한다. 

<div style="margin-top: 3.3em;"></div>

만약 top-K 또는 top-P 둘 중 **하나만 설정된 경우**라면?
- 그 설정만 적용해서 후보를 좁힌다
- temperature는 항상 **후보가 결정된 후**에 작동한다. 


<div style="margin-top: 3.3em;"></div>

temperature를 지정하지 않았다면?
- **필터된 후보들 중에서 무작위**로 단어가 선택된다.

<br>

---
##### 🎯 완전히 결정적인 출력을 원할 땐?
- **temperature를 0**으로 설정하면
  - 무작위성이 완전히 사라지고, **항상 가장 확률 높은 단어**가 선택된다.  <small style="color:gray">(어차피 하나의 단어만 선택되기 때문!)</small>
  - 이 경우 top-K와 top-P는 **사실상 무시**된다.   

<div style="margin-top: 2.3em;"></div>

- 반대로, **top-K = 1** 또는 **top-P = 0**으로 설정해도 temperature에 상관없이 **가장 확률 높은 단어만 선택**된다.
  > setting top-K to 1 or top-P to 0 also overrides temperature — forcing a single best guess.

<div style="margin-top: 2.3em;"></div>

- top-K 값을 아주 크게 설정하거나, top-P를 1로 설정하면 사실상 **거의 모든 단어 후보가 출력 가능**해진다.

<br>

---
##### 🔬 백서에서 추천하는 조합들

<table style="border-collapse: collapse; width: 100%; text-align: center;">
  <thead>
    <tr style="background-color: #f9f9f9;">
      <th style="border: 1px solid #ccc; padding: 10px;">목적</th>
      <th style="border: 1px solid #ccc; padding: 10px;">Temperature</th>
      <th style="border: 1px solid #ccc; padding: 10px;">Top-P</th>
      <th style="border: 1px solid #ccc; padding: 10px;">Top-K</th>
      <th style="border: 1px solid #ccc; padding: 10px;">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ccc; padding: 10px;">💡 창의적인 아이디어 탐색</td>
      <td style="border: 1px solid #ccc; padding: 10px;">0.9</td>
      <td style="border: 1px solid #ccc; padding: 10px;">0.99</td>
      <td style="border: 1px solid #ccc; padding: 10px;">40</td>
      <td style="border: 1px solid #ccc; padding: 10px;">다양성과 무작위성을 극대화</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 10px;">🧠 일반적인 프롬프트 + 살짝 창의성</td>
      <td style="border: 1px solid #ccc; padding: 10px;">0.2</td>
      <td style="border: 1px solid #ccc; padding: 10px;">0.95</td>
      <td style="border: 1px solid #ccc; padding: 10px;">30</td>
      <td style="border: 1px solid #ccc; padding: 10px;">CoT, ReAct 등에서 적당한 조합</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 10px;">📄 일반적인 코드/텍스트 생성</td>
      <td style="border: 1px solid #ccc; padding: 10px;">0.1</td>
      <td style="border: 1px solid #ccc; padding: 10px;">0.9</td>
      <td style="border: 1px solid #ccc; padding: 10px;">20</td>
      <td style="border: 1px solid #ccc; padding: 10px;">안정적인 정형 출력</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 10px;">✅ 단 하나의 정답이 필요한 경우</td>
      <td style="border: 1px solid #ccc; padding: 10px;">0</td>
      <td style="border: 1px solid #ccc; padding: 10px;">-</td>
      <td style="border: 1px solid #ccc; padding: 10px;">-</td>
      <td style="border: 1px solid #ccc; padding: 10px;">완전한 결정론적 출력</td>
    </tr>
  </tbody>
</table>
<br>

---
##### ⚠ 반복 루프(Repetition Loop)에 주의
모델이 **특정 단어나 문장을 계속해서 반복**하는 상황을 말한다.  
> where the model gets stuck repeating the same words or phrases over and over again.

이런 반복은 **너무 낮거나 너무 높은 temp** 둘 다에서 발생할 수 있다. 
- 낮은 temp: **너무 예측 가능한** 출력 → 같은 표현 반복  
- 높은 temp: **우연히 같은 상태로 회귀** → 반복 루프

> it's like the model is either too predictable or too random - and either way it ends up repeating itself.

💡 해결법은?   
   **temperature, top-K, top-P를 섬세하게 조절** <small style="color:gray">(fine-tune)</small>해서 창의성과 안정성 사이의 **적절한 균형점(sweet spot)**을 찾는 것이다.

<br>

---
### 💭오늘 챙겨간 것들
프롬프트 엔지니어링의 시작은 LLM이 어떤 출력을 생성할지를 제어하는 방법을 아는 것이다.  

✔️ 출력 길이 설정은 비용과 속도, 시스템 제약에 직접적인 영향을 준다.  
✔️ **샘플링 조절(temperature, top-K, top-P)**을 통해 정확도와 창의성, 다양성 사이에서 밸런스를 맞출 수 있다.  
✔️ 세 가지 설정은 **top-K & top-P → temp** 선택의 순서로 조합되며, 세팅 조합이 반복 루프 같은 오류를 피하는 데도 중요하다.  

결국 중요한 건 상황에 따라 적절한 조합을 실험해보는 것이다.

<div style="margin-top: 2.3em;"></div>

다음 글에서는 프롬프트 엔지니어링의 핵심 전략들, **Chain of Thought, ReAct** 등 본격적인 기법을 정리할 예정! byebye

<br><br>

