---
layout: post
title:  "[Kaggle Gen AI] Day 1 - 샘플링으로 바꾸는 LLM의 말하기 스타일 🚀"
author: me
categories: [ Kaggle Gen-AI ]
date: 2025-04-09 09:00:00
image: assets/images/20250402/day1.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_6/)에서는 모델의 출력을 좌우하는 핵심 스킬, **프롬프트 엔지니어링**의 기본을 정리해봤다.  

이번에는 같은 프롬프트를 줬는데도 모델의 출력이 달라지는 이유인 **샘플링 기법**<small style="color:gray">(Sampling Techniques)</small>에 대해 알아보기로!  
즉, LLM이 **어떤 단어를 고를지**, 그 선택 과정을 어떻게 조절하느냐에 따라 답변이 **논리적이고 정확할 수도**, 혹은 **창의적이고 독창적일 수도** 있는 것이다.

> The model generates text— the sampling techniques can have a big impact on the **quality**, **creativity**, and **diversity** of the output.

이번 게시물에서는  
✔️ 가장 기본적인 **Greedy Search**부터  
✔️ 창의성을 높이는 **Random Sampling**,  
✔️ 그리고 요즘 많이 쓰이는 **Top-k, Top-p, Temperature** 같은 기법들까지  
대표적인 샘플링 방식들을 가볍게 정리해보겠다.

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=Na3O4Pkbp-U&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=1)

<br>

---
### 🔄 샘플링이란? 모델의 다음 말을 고르는 방식
LLM은 텍스트를 생성할 때, 매 순간 **“다음에 어떤 단어를 말할지”** 선택해야 한다.  
이때 어떤 기준으로 그 단어를 고를지 정하는 게 바로 **샘플링 기법**<small style="color:gray">(Sampling Techniques)</small>이다.

이 방식에 따라 모델이  
✔️ 더 **사실 기반의 정돈된 응답**을 할 수도 있고,  
✔️ 더 **창의적이고 자유로운 스타일**로 대답할 수도 있다.

<br>

---
### 🧪 주요 샘플링 기법들
프롬프트를 구성하는 방법은 다양하지만, 이번 유닛에서는 가장 **기본적이고 자주 쓰이는 세 가지 방식**만 간단히 소개하고 있다.  

##### 1️⃣ Greedy Search  
가장 단순한 방식.  
항상 **가장 확률이 높은 단어**를 고르는 방식이다.  

빠르고 예측 가능하지만, **반복적인 문장이나 지루한 표현**이 나올 확률이 높다.  
> this is fast, but can lead to repetitive output

<div style="margin-top: 3.3em;"></div>

##### 2️⃣ Random Sampling  
이름처럼 **무작위성**<small style="color:gray">(randomness)</small>을 더 많이 도입하는 방식이다.  
더 **창의적이고 유연한 출력**이 가능하지만, 때로는 **비논리적이거나 말이 안 되는 결과**가 나올 수도 있다.  
> more creative outputs, but also a higher chance of getting nonsensical text

<div style="margin-top: 3.3em;"></div>

##### 3️⃣ Temperature  
랜덤 샘플링의 **무작위성 정도를 조절**하는 매개변수.  

- **높은 temperature** → 더 다양한 단어 선택 (창의적, 예측불가)  
- **낮은 temperature** → 더 보수적인 단어 선택 (안정적, 사실 기반)

> higher temperature = more randomness

<div style="margin-top: 3.3em;"></div>

##### 4️⃣ Top-k Sampling  
모델이 다음 단어로 **가장 가능성 높은 K개만** 후보로 두고 그 중에서 선택하게 하는 방식.  
불필요한 단어를 제거해 출력의 **통제력**을 높인다.

<div style="margin-top: 3.3em;"></div>

##### 5️⃣ Top-p Sampling (Nucleus Sampling)  
Top-k와 비슷하지만, **누적 확률 기준으로 일정 임계치(p)**에 도달할 때까지 **동적으로 후보군을 구성**한다.  
더 **유연하고 상황에 맞는** 샘플링이 가능하다.  
> uses a dynamic threshold based on the probabilities of the tokens

<div style="margin-top: 3.3em;"></div>

##### 6️⃣ Best-of-n Sampling  
여러 개<small style="color:gray">(n개)</small>의 출력을 생성한 뒤, 그 중에서 **가장 적절한 하나를 선택**하는 방식이다.  
기준은 다양하게 설정할 수 있으며, **응답 품질을 한 번 더 필터링**하는 데 유용하다.

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  💬 예시로 <strong>Best-of-5</strong>라고 하면 → <strong>5개의 답변 생성</strong> → 평가 기준에 따라 <strong>가장 좋은 1개만 선택</strong>
</div>
<br>

---
### 💭오늘 챙겨간 것들
이번 게시물에서는 LLM이 **다음 단어를 어떻게 고르는지** 결정하는 다양한 **샘플링 기법들**을 정리해봤다.  

가장 기본적인 **Greedy Search**부터 창의성을 높이는 **Random Sampling**, 출력 스타일을 섬세하게 조절할 수 있는 **Top-k, Top-p, Temperature**,
그리고 품질을 한 번 더 걸러주는 **Best-of-n**까지 모두 출력의 **정확도, 다양성, 창의성**을 결정짓는 핵심 요소들이다!
> so fine-tuning these sampling parameters is key to getting the kind of output you want— whether it’s factual and accurate or more creative and imaginative.

<div style="margin-top: 3.3em;"></div>

다음 게시물에서는 LLM이 진짜로 **잘 작동하고 있는지** 어떻게 평가할 수 있는지를 살펴볼 예정이다.  

텍스트 생성처럼 **정답이 딱 정해져 있지 않은 문제**에서는 전통적인 ML 평가 방식<small style="color:gray">(F1-score, Accuracy, etc.)</small>만으로는 부족하다.  

그래서 다음 글에서는  
✔️ 어떤 평가 기준이 진짜 ‘좋은 LLM’을 잘 판별해주는지,  
✔️ 여전히 쓰이는 **BLEU, ROUGE 같은 정량적 지표**부터  
✔️ 직접 사람이 판단하는 **휴먼 평가**,  
✔️ 그리고 요즘 뜨고 있는 **LLM 기반 평가자(AI evaluator)**까지  
LLM을 실제 서비스에 도입하기 전 꼭 거쳐야 할 **평가 프레임워크 전반**을 공부해보겠다! 야호

<br>

---
<ul>
 Day-1
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-day1-assignments/">[Kaggle Gen AI] Day 1 과제 소개 - LLM & Prompt Engineering 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_1/">[Kaggle Gen AI] Day 1 - Transformer 구조 한 눈에 보기🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_2/">[Kaggle Gen AI] Day 1 - Decoder-Only 구조, 왜 LLM은 디코더만 쓸까? 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_3/">[Kaggle Gen AI] Day 1 - Transformer 이후, LLM 진화 타임라인 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_4/">[Kaggle Gen AI] Day 1 - 오픈소스 LLM 생태계, 한눈에 보기 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_5/">[Kaggle Gen AI] Day 1 - LLM 파인튜닝 A to Z 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_6/">[Kaggle Gen AI] Day 1 - 프롬프트 엔지니어링, LLM을 잘 쓰는 기술 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_8/">[Kaggle Gen AI] Day 1 - 정답이 없어서 더 중요한 LLM 평가법 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_9/">[Kaggle Gen AI] Day 1 - 품질, 속도, 비용 사이에서 균형 잡기: LLM 추론 가속 기법들 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_10/">[Kaggle Gen AI] Day 1 - LLM은 지금 어디에 쓰이고 있을까? 실전 적용 사례 🚀</a></small></li>
</ul>

<br><br>

