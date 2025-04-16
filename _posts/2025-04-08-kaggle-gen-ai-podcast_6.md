---
layout: post
title:  "[Kaggle Gen AI] Day 1 - 프롬프트 엔지니어링, LLM을 잘 쓰는 기술 🚀"
author: me
categories: [ Kaggle Gen-AI ]
date: 2025-04-08 09:00:00
image: assets/images/20250402/day1.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_5/)에서는 LLM을 특정 작업에 맞게 조정하는 **파인튜닝**<small style="color:gray">(fine-tuning)</small>의 과정을 살펴봤다.  

하지만 이렇게 잘 훈련된 모델을 **실제로 어떻게 써먹을 수 있을까?**  
여기서 필요한 게 **프롬프트 엔지니어링**<small style="color:gray">(prompt engineering)</small>이다.  
> prompt engineering seems to be key skill here. absolutely essential.

이번 게시물에서는 프롬프트 엔지니어링의 기본 개념부터  
✔️ **어떻게 하면 원하는 출력을 얻을 수 있는지**,  
✔️ **프롬프트만 잘 바꿔도 얼마나 성능이 달라지는지**,  
✔️ 그리고 실제로 자주 쓰이는 대표적인 프롬프트 기법들까지 가볍게 정리해보려 한다.  

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  <small style="color:gray">💬 이 인트로 유닛 다음인 유닛 1의 주제가 프롬프트 엔지니어링이라 여기서는 프롬프트 엔지니어링을 간단하게 훑어만 보고 있음! 
  자세한 내용은 다음 유닛에서 본격적으로 다룰 예정.</small>
</div>

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=Na3O4Pkbp-U&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=1)

<br>

---
### 🧾 프롬프트란? 원하는 출력을 이끌어내는 입력 설계
프롬프트 엔지니어링은 결국 모델에게 주는 **입력**<small style="color:gray">(The Prompt)</small>을 **내가 원하는 출력이 나오도록 설계하는 기술**이다.  

프롬프트만 잘 다듬어도, 모델이 출력하는 답변의 **품질과 적절성**이 눈에 띄게 달라질 수 있다.  
> it can make a huge difference in the quality and relevance of the model’s response.

<br>

---
### 📝 대표적인 프롬프트 기법들
프롬프트를 구성하는 방법은 다양하지만, 이번 유닛에서는 가장 **기본적이고 자주 쓰이는 세 가지 방식**만 간단히 소개하고 있다.  

##### 1️⃣ Zero-shot Prompting
**예시 없이**, 질문이나 지시만 바로 주는 방식.  
모델의 **기존 지식**에 전적으로 의존해서 답을 생성하게 된다.

<div style="margin-top: 2.3em;"></div>

##### 2️⃣ Few-shot Prompting
여기서는 **예시 몇 개를 함께 제시**해서, 모델이 원하는 **형식이나 문체**를 파악하도록 돕는다.  
자연스럽게 기대하는 응답의 스타일을 유도할 수 있다.

<div style="margin-top: 2.3em;"></div>

##### 3️⃣ Chain of Thought Prompting
**복잡한 문제 해결**에 특히 효과적인 방식이다.  
모델에게 단순한 정답만 요구하는 게 아니라, **문제를 단계별로 풀어나가는 사고 과정을 보여주는 것!**

> like teaching it how to break down a complex problem into smaller, more manageable steps.

<br>

---
### 💭오늘 챙겨간 것들
오늘은 **프롬프트 엔지니어링**의 기초 개념과 자주 쓰이는 **세 가지 대표 기법**(Zero-shot, Few-shot, Chain of Thought)을 간단히 살펴봤다.  

다음 게시물에서는, 모델이 텍스트를 생성할 때 **어떤 단어를 선택할지 결정하는 방식**인 **샘플링 기법**<small style="color:gray">(Sampling Techniques)</small>에 대해 알아보겠다.  

<br><br>

