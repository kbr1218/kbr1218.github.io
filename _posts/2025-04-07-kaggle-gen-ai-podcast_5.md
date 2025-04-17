---
layout: post
title:  "[Kaggle Gen AI] Day 1 - LLM 파인튜닝 A to Z 🚀"
author: me
categories: [ Kaggle Gen-AI ]
date: 2025-04-07 09:00:00
image: assets/images/20250402/day1.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_4/)에서는 오픈소스 LLM 생태계를 정리하면서, 최근 공개된 다양한 모델이 어떤 특징을 갖고 있는지 살펴봤다.  

하지만 강력한 **파운데이션 모델**<small style="color:gray">(foundation models)</small>도 실제로 어떤 작업에 쓰려면 **그에 맞게 조정**해줄 필요가 있다.
> These foundation models are powerful, but they need to be tailored for specific tasks— and that’s where fine-tuning comes in exactly.

그래서 이번 게시물에서는, **LLM을 특정 태스크에 맞케 커스터마이징하는 법**, 바로 **파인튜닝**<small style="color:gray">(fine-tuning)</small>의 전체 흐름을 정리해보겠다.  

Pretraining → SFT → RLHF → PEFT까지 이어지는 기술 흐름을 따라가면서, 요즘 많이 쓰인다는 **LoRA, QLoRA, Soft Prompting** 같은 방식들도 함께 정리해볼 예정!

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=Na3O4Pkbp-U&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=1)

<br>

---
### 📚 Pre-training: 언어를 처음 배우는 단계
파운데이션 모델의 첫 단계는 **pre-training**이다. 말 그대로 모델에게 **엄청나게 많은 텍스트 데이터를 주는 과정!** <small style="color:gray">(팟캐스트에서는 'feed'라는 표현을 썼다.)</small>  
레이블 없이 그냥 **raw 데이터**만 주면서 학습시키는 방식이다. 
> like learning the grammar and vocabulary of a language.

이 과정을 통해 모델은 **단어와 문장이 어떻게 연결되는지, 언어의 기본 패턴**을 익히게 된다.  
하지만 이 작업은 계산 자원을 **굉장히굉장히** 많이 요구한다. <small style="color:gray">(super resource intensive)</small>
> like giving the model a general education in language.

<br>

---
### 🔌 Fine-tuning: 모델을 "전문가"로 만드는 과정
Pre-training을 통해 모델이 **언어에 대한 일반적인 감각**을 갖게 되었다면, **fine-tuning**은 그 모델을 **특정 작업에 맞게 다듬는** 과정이다.  

번역, 질문응답, 창의적인 글쓰기처럼 **특정한 작업**<small style="color:gray">(task)</small>에 모델을 맞추고 싶을 때, 그에 맞는 **작은 규모의 데이터셋으로 추가 학습**을 시키는 것!  
이 과정을 통해 모델은 **일반적인 언어 감각을 가진 '학생'**에서 **특정 분야에 능숙한 '전문가'**로 바뀌게 된다.  
> so you’re specializing the model, making it an expert in a particular area.

<div style="margin-top: 3.3em;"></div>

그리고 이때 자주 쓰이는 방법 중 하나가 바로 **Supervised Fine-tuning(SFT)**이다.  
<br>

---
### 🧠 Supervised Fine-tuning (SFT): “무엇을” + “어떻게” 가르치기
SFT는 **아주 흔하게 쓰이는 파인튜닝 방식**이다.  
모델에게 **프롬프트와 정답이 함께 주어진 데이터셋**을 통해 학습시키는 방식.  

예를 들어,  
"질문을 하면 답을 하게 만들고 싶다"면 → **"질문 + 올바른 답변" 쌍이 여러 개 있는 데이터셋으로 모델을 학습**시키는 것이다.  

이런식으로 SFT는 모델이  
✔️ **무엇을 해야 하는지**뿐만 아니라 <small style="color:gray">(not just teaching it what to do)</small>  
✔️ **어떻게 행동해야 하는지**까지 학습하도록 도와준다. <small style="color:gray">(also teaching it how to behave)</small>  

→ **도움이 되고<small style="color:gray">(helpful)</small>, 안전하고<small style="color:gray">(safe)</small>, 지시를 잘 따르는 모델<small style="color:gray">(good at following instructions)</small>**로 만드는 데 중요한 역할을 한다.

<br>

---
### 🎯 RLHF (Reinforcement Learning from Human Feedback)
SFT로는 부족한 부분을 더 보완해주는 게 RLHF이다.  
> how do teach these models to be more human-like in their responses?

RLHF는 모델의 출력을 **사람이 선호하는 방향으로 정렬**<small style="color:gray">(alignment)</small>시키는 방법이다.
- 사람이 직접 여러 응답을 보고, **더 나은 응답에 점수를 매기거나 순위를 매기고**, 
- 그 피드백으로 **보상 모델**<small style="color:gray">(Reward Model)</small>을 만든 뒤,
- 그 보상 모델을 기준으로 **LLM을 강화학습 방식으로 다시 학습**시키는 방식.

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  💬 결국, 모델이 <strong>사람이 더 좋아할 만한 답변을 생성하도록 학습</strong>하는 것이다.
  <small style="color:gray">(not just about giving the model correct answers, but about teaching it to generate responses that humans find helpful, truthful and safe.)</small>
</div>
<br>

---
### 🧪 RLAIF & DPO: 최신 Alignment 기법들
최근엔 사람 피드백 없이 더 효율적으로 정렬하는 방법도 연구되고 있다.

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  💡 <strong>여기서 말하는 alignment란?</strong><br>
  LLM이 만들어내는 응답<small style="color:gray">(output)</small>을 <strong>사람의 기대, 가치, 선호에 맞게 조정하는 것</strong>을 말한다.<br><br>
  모델이 그냥 아무 말이나 하지 않고,<br>
  🙅‍♂️ 위험하거나 유해한 말은 피하고,<br>
  🤝 정중하고 정확하며, 지시를 잘 따르도록 만드는 과정!<br><br>
  이 alignment를 위해 <strong>SFT, RLHF, RLAIF, DPO</strong> 같은 다양한 기술이 활용된다.
</div>


##### RLAIF (Reinforcement Learning from AI Feedback)
  : 사람 대신 다른 AI 모델이 피드백을 주는 방식

<div style="margin-top: 2.3em;"></div>

##### DPO (Direct Preference Optimization)
  : 보상 모델 없이도, 선호도를 바로 최적화하는 새로운 접근법

→ 이런 기술들은 정렬 과정<small style="color:gray">(alignment)</small>을 더 빠르고 안정적으로 만드는 시도들이다.

<br>

---
### 🧩 PEFT (Parameter-Efficient Fine-Tuning): 작은 변화로 큰 효과 내기
LLM을 전부 파인튜닝하는 건 사실 **비용과 자원이 엄청나게 많이 드는 작업**이다.  
그래서 최근에는, **모델 전체를 바꾸지 않고도 원하는 태스크에 적응시키는** 새로운 방식인 **PEFT**<small style="color:gray">(Parameter-Efficient Fine-Tuning)</small> 기법들이 등장하고 있다.  

즉, 모델 전체를 손대는 게 아니라, **작은 부분만 조정해서 전체 성능을 끌어올리는 전략**이라고 보면 된다.
> so it's like just making small adjustments instead of overhauling the entire system.

<div style="margin-top: 2.3em;"></div>

아래는 PEFT 기법들의 예시 몇 가지이다:

##### 1️⃣ Adapter-based Fine-tuning
모델 안에 **작은 모듈**<small style="color:gray">(Adapter)</small>을 추가하고, **그 안의 파라미터만 학습**시키는 방식이다. 
기존 모델은 유지하면서 **일종의 확장팩**을 붙여서 새로운 능력을 부여하는 느낌!

<div style="margin-top: 2.3em;"></div>

##### 2️⃣ LoRA (Low-Rank Adaptation)
전체 **weight**를 바꾸는 대신, 변화를 **저차원 행렬**<small style="color:gray">(Low-Rank Matrices)</small>로 근사해서 학습시키는 기법이다. 
아주 적은 수의 파라미터만 바꾸면서도, **전체 모델을 파인튜닝한 것처럼** 효과를 낼 수 있다.

<div style="margin-top: 2.3em;"></div>

##### 3️⃣ QLoRA (Quantized LoRA)
LoRA를 **더 가볍교 효율적으로 만든 버전**이다.  
모델의 weight를 **양자화**<small style="color:gray">(Quantization)</small> 해서 메모리 사용량을 대폭 줄이면서, **훨씬 적은 비용으로 LoRA를 적용**할 수 있게 해준다.

<div style="margin-top: 2.3em;"></div>

##### 4️⃣ Soft Prompting
모델 자체는 건드리지 않고, **입력 앞에 "학습된 벡터"<small style="color:gray">(Soft Prompt)</small>를 붙이는 방식**이다. 
이 벡터는 모델에게 주는 **가이드라인**처럼 작동해서, 원하는 태스크에 모델이 더 잘 반응하도록 도와준다.

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  💬 이렇게 다양한 PEFT 기법들은 <strong>성능과 비용, 효율성 사이에서 다양한 선택지를 제공</strong>해준다.
</div>

<br>

---
### 💭오늘 챙겨간 것들
이번 게시물에서는 **LLM을 태스크에 맞게 조정하는 전체 과정**을 살펴봤다. Pretraining부터 시작해서 SFT, RLHF, 그리고 최신 Alignment 기법인 DPO까지, 그 흐름 속에서 **LoRA, QLoRA, SoftPrompting**처럼 **비용은 낮추고 효율을 높이는 PEFT 기법**이 어떻게 등장했는지도 이해할 수 있었다!  

다음 게시물에서는 LLM을 잘 다루기 위한 핵심 기술 중 하나인 **프롬프트 엔지니어링**을 간단히 살펴볼 예정이다. <small style="color:gray"><em>(프롬프트 엔지니어링은 이번 팟캐스트의 첫 번째 유닛의 메인 주제이기 때문에 이번 인트로 유닛에서는 핵심만 간단하게 설명하고 넘어감!)</em></small>

<br>

---
<ul>
 Day-1
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-day1-assignments/">[Kaggle Gen AI] Day 1 과제 소개 - LLM & Prompt Engineering 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_1/">[Kaggle Gen AI] Day 1 - Transformer 구조 한 눈에 보기🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_2/">[Kaggle Gen AI] Day 1 - Decoder-Only 구조, 왜 LLM은 디코더만 쓸까? 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_3/">[Kaggle Gen AI] Day 1 - Transformer 이후, LLM 진화 타임라인 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_4/">[Kaggle Gen AI] Day 1 - 오픈소스 LLM 생태계, 한눈에 보기 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_6/">[Kaggle Gen AI] Day 1 - 프롬프트 엔지니어링, LLM을 잘 쓰는 기술 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_7/">[Kaggle Gen AI] Day 1 - 샘플링으로 바꾸는 LLM의 말하기 스타일 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_8/">[Kaggle Gen AI] Day 1 - 정답이 없어서 더 중요한 LLM 평가법 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_9/">[Kaggle Gen AI] Day 1 - 품질, 속도, 비용 사이에서 균형 잡기: LLM 추론 가속 기법들 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_10/">[Kaggle Gen AI] Day 1 - LLM은 지금 어디에 쓰이고 있을까? 실전 적용 사례 🚀</a></small></li>
</ul>
<br><br>