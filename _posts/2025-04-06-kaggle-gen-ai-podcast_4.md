---
layout: post
title:  "[Kaggle Gen AI] Day 1 - 오픈소스 LLM 생태계, 한눈에 보기 🚀"
author: me
categories: [ Kaggle Gen-AI ]
date: 2025-04-06 09:00:00
image: assets/images/20250402/day1.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_3/)에서는 LLM이 어떻게 발전해 왔는지, GPT-1부터 GPT-4, Gemini까지 주요 모델들을 중심으로 진화 타임라인을 정리했다.  
각 모델의 구조적 전환점과 기술 트렌드, 그리고 성능 향상의 주요 포인트를 확인했는데,

이번 게시물에서는, **오픈소스 LLM** 생태계를 알아보겠다!  
최근 Google, Meta, Mistral, DeepSeek 등 다양한 기업들이 공개한 주요 오픈모델들이 있고, 각 모델이 어떤 특성을 갖고 있는지, 또 어떤 점에서 주목할 만한지를 간단하게 정리해보기로~  

참고로, 팟캐스트에서는 **"open-source side of things"**라고 말하면서 엄밀히 말해 오픈소스는 아니지만 접근 가능한 모델들도 함께 소개하고 있다.
<small style="color:gray">(open weights, semi-open licensing 포함)</small>

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=Na3O4Pkbp-U&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=1)

<br>

---
### 1️⃣ Google Gemma & Gemma 2 (2024) — 가볍고 빠르지만 강력한 모델
첫 번째로 언급된 건 Gemini 기반의 경량화 오픈모델 시리즈이다!

![Google Gemma](https://storage.googleapis.com/gweb-developer-goog-blog-assets/images/Gemma-social.2e16d0ba.fill-1200x600.png)

- 2B 파라미터 버전은 **단일 GPU에서도 실행이 가능**하여 접근성이 매우 높다.
- 특히 Gamma 2는 Meta의 LLaMA 3 70B와 비교해도 뒤지지 않는 성능을 보여주어 주목을 받는 중!

<br>

---
### 2️⃣ Meta LLaMA 시리즈 — 오픈소스 LLM의 대표 주자
**LLaMA 1 → 2 → 3**로 이어지는 시리즈.

![Meta LLaMA](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/69129b55-6798-43cd-92b5-0203f5d5a2f3/10.png?t=1730375002)

- LLaMA 2는 상업적 사용이 허용된 라이선스로 특히 영향력이 컸고,
- 최신 버전인 **LLaMA 3는 추론, 코딩, 상식, 안전성 등에서 계속 발전** 중이다.
- LLaMA 3.2에선 다국어 및 비전 모델까지 확장하고 있다.

<div style="margin-top: 1.3em;"></div>

<br>

---
### 3️⃣ Mistral AI & Mixtral — MoE 구조로 효율과 성능 모두
대표 모델인 Mixtral은 **Sparse MoE 구조**를 채택하였고 → 8개의 전문가 중 2개만 활성화하여 계산량을 절감했다.

![Mistral](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTsAiU2P4sfJhCPLSWWie0jJf65TSxyAb1zzQ&s)

- 수학, 코딩, 다국어 태스크에서 강력한 성능을 보이며 대부분 모델이 오픈소스로 공개되어 실제 적용에도 용이하다.

<br>

---
### 4️⃣ OpenAI o1 시리즈 — 고난도 추론에 특화
이름처럼 o1 모델은 복잡한 **과학·논리적 추론 태스크**에 강하다.

![Mistral](https://images.ctfassets.net/kftzwdyauwt9/4G2ZNJjHK6uhLQO2pXR7e3/9522fa4dd9f372deddfc710a02fad44c/o1-research.png?w=3840&q=90&fm=webp)

- 최근 공개된 과학적 추론 벤치마크에서 최상위권 성능 기록 중이다!
- 오픈소스는 아님

<br>

---
### 5️⃣ DeepSeek — RL 기반의 새로운 추론 방식 실험
**Group Relative Policy Optimization**이라는 새로운 **강화학습 방식**을 적용한 모델이다.

![DeepSeek](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT4OFbJw-hq6Bh8jgqK4q1Io8UFcq47DumjxA&s)

- DeepSeek R1 모델은 OpenAI의 o1 모델과 유사한 수준의 성능을 보이고 있고,
- 아직 코드는 공개되지 않았지만 **모델 가중치는 제공**되었다. 

<br>

---
### 6️⃣ 기타 주목할 만한 모델들
- CU 1.5 <small style="color:gray">from Alibaba</small>
- YE <small style="color:gray">from U1 AI</small>
- Grock 3 <small style="color:gray">from XII</small>  

→ 이들 역시 Transformer 기반으로 구축되어 있다.

<br>

---
### 💭오늘 챙겨간 것들
Gemma, LLaMA, Mixtral 등의 오픈소스 혹은 공개된 LLM들이 다양하게 등장하고 있고 서로 다른 강점을 가지고 있다!  
오픈 소스라고 소개된 모델 중 일부는 완전한 오픈소스가 아니며, 가중치만 공개되었거나, 상업적 제한이 있는 경우도 존재한다.

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  ⚠️ 오픈모델이라 해도, <strong>“오픈 = 자유 사용”은 아님!</strong> 용도에 따라 반드시 라이선스를 확인해야 한다.
</div>

다음 글에서는, LLM의 파인튜닝<small style="color:gray">(fine-tuning)</small>에 대해 간단하게 살펴보겠다. 🚀

<br><br>