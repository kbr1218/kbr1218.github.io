---
layout: post
title:  "[Kaggle Gen AI] Day 1 - 품질, 속도, 비용 사이에서 균형 잡기: LLM 추론 가속 기법들 🚀"
author: me
categories: [ Kaggle Gen-AI ]
date: 2025-04-11 09:00:00
image: assets/images/20250402/day1_1.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_8/)에서는 LLM이 생성한 결과를 **어떻게 평가할 수 있는지**에 대해 간단하게 살펴봤다.  
텍스트 생성처럼 **정답이 없는 문제**일수록 단순한 정확도나 BLEU 점수만으로는 부족하고, **다양한 기준과 다면적인 평가 프레임워크**가 필요하다는 걸 알게 되었다.  

이번에는 LLM을 **더 빠르고 효율적으로 동작시키는 방법**인 **Inference Acceleration(추론 가속)**에 대해 정리해보려고 함!  

LLM이 아무리 똑똑해도 **응답 속도가 느리거나 / 비용이 너무 많이 들면** 실제 서비스에서는 쓰기 어렵다.  

그래서 이번 글에서는  
✔️ LLM 추론을 빠르게 만드는 다양한 기법들,  
✔️ 품질<small style="color:gray">(Quality)</small> vs 속도<small style="color:gray">(Speed)</small> vs 비용<small style="color:gray">(Cost)</small> 사이의 균형,  
✔️ 그리고 지연 시간<small style="color:gray">(Latency)</small>, 처리량<small style="color:gray">(Throughput)</small>을 조절하는 기술들까지  
실제 시스템에서 효율적으로 LLM을 사용하는 법을 알아보겠다.  

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=Na3O4Pkbp-U&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=1)

<br>

---
### ⚙️ 왜 추론 가속이 중요한가?
LLM은 정말 똑똑하지만 **모델이 커질수록 느려지고, 응답을 받는 데 시간이 오래걸리고 실행 비용도 점점 올라간다.**  

그래서 **추론 과정<small style="color:gray">(Inference optimization; 응답을 생성하는 과정)</small> 자체를 최적화**하는 것이 중요하다. 
특히 **속도가 중요한 실시간 애플리케이션일수록 더!**  

<div style="margin-top: 3.3em;"></div>

##### 핵심은 트레이드오프 (Trade-off)
LLM 추론 최적화는 결국 **여러 요소들 사이의 균형**을 찾는 작업이다.  
**품질**<small style="color:gray">(Quality)</small> vs. **속도**<small style="color:gray">(Speed)</small> vs. **비용**<small style="color:gray">(Cost)</small>
사이에서 **균형을 잡는 것**이 관건이다.

> sometimes you might sacrifice little accuracy to gain a lot of speed.

또 하나 중요한 건, 단순히 한 번의 응답 속도<small style="color:gray">(latency)</small>뿐만 아니라, 
시스템 전체가 **얼마나 많은 요청을 처리할 수 있냐**<small style="color:gray">(throughput)</small>도 함께 고려해야 한다.

> the best approach depends on the application.  

<br>

---
### ⚙️ 추론을 가속하는 두 가지 접근 방식
LLM 추론을 빠르게 만드는 기술은 크게 두 가지로 나눌 수 있다.  
- **출력 결과를 조금 바꾸더라도 효율을 높이는 방법** <small style="color:gray">(Output Approximating Methods)</small>
- **출력은 그대로 두고 계산만 더 빠르게 만드는 방법** <small style="color:gray">(Output Preserving Methods)</small>   

하나씩 살펴봅시다요.

<div style="margin-top: 3.3em;"></div>

---
##### 1️⃣ Output Approximating Methods
출력 결과가 약간 달라질 수도 있지만, **추론 속도나 효율을 크게 끌어올릴 수 있는 기법**들이다.

<div style="margin-top: 3.3em;"></div>

 1-1. **Quantization (양자화)**
  - 가장 널리 쓰이는 기법 중 하나.
  - 모델의 가중치<small style="color:gray">(weights)</small>와 연산<small style="color:gray">(activations)</small>을 **32비트 대신 8비트, 심지어 4비트 정수로 줄여서 계산**하는 방식이다. 

    > it's all about reducing the numerical precision.  

  - 메모리도 절약되고, 계산 속도도 빨라진다. 
  - 그리고 대부분의 경우 **정확도 손실도 아주 미미**하다.  
    
    <div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💡 <strong>QAT (Quantization Aware Training)</strong>을 사용하면,<br>
    정확도 손실을 더 줄이면서 양자화를 적용할 수 있고, 양자화<small style="color:gray">(Quantization)</small> 전략 자체를 <strong>파인튜닝<small style="color:gray">(fine-tuning)</small>하는 것도 가능</strong>하다.
    </div>

<div style="margin-top: 3.3em;"></div>

1-2. **Distillation (지식 증류)**
- **더 크고 정확한 모델(teacher model)**의 행동을 **더 작고 빠른 모델(student model)**이 배우도록 학습시키는 방식이다. 

    > you have a large accurate teacher model and you train a smaller student model to copy its behavior.

- student model의 속도는 훨씬 빠르고, 적절히 학습하면 좋은 정확도를 유지할 수 있다.  

- 대표적인 방식들:
  1. data distillation
  2. knowledge distillation
  3. on policy distillation
  
  <div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 이런 방식들은 <strong>결과가 약간 달라질 수 있지만</strong> 효율을 높이는 데 효과적이다. 
  </div>
  <div style="margin-top: 3.3em;"></div>

> there’s a whole toolbox of techniques for making these models run faster and more efficiently

<br>

---
##### 2️⃣ Output Preserving Methods
이번엔 **출력 결과를 바꾸지 않으면서, 계산 결과를 더 빠르게 만드는 방법**들이다.  
→ 성능을 깎지 않으면서 효율을 올릴 수 있다는 것이 장점!  

<div style="margin-top: 3.3em;"></div>

2-1. **FlashAttention**
  - Transformer에서 병목 구간인 **Self-Attention 계산을 최적화**하는 기술이다.
  - 계산 방식만 바꾸는 거라서 **출력은 기존과 동일**하면서도
  - 속도는 훨씬 빨라진다.

  <div style="margin-top: 3.3em;"></div>

2-2. **Prefix Caching**
  > seems like a good trick for conversational applications.

  - 이전 입력들과 반복되는 부분이 많을 때,
  - **이미 계산한 attention 값을 캐시해두고 다시 쓰는** 방식이다. 
  - 그래서 매 턴마다 계산을 다시 할 필요가 없다.
  - Google의 AI studio나 Vertex AI 등에서도 이 방식이 적용되어 있따.

  > it’s like remembering what you’ve already calculated so you don’t have to do it again.

<div style="margin-top: 3.3em;"></div>

2-3. **Speculative Decoding**
  - 꽤 똑똑한 방식이다. 
  - **작고 빠른 draft 모델**이 먼저 다음 토큰들을 예측하고
  - **메인 모델이 그 결과를 (병렬로) 확인**하는 구조이다. 
  - draft가 예측한 결과가 맞다면, 그 토큰을 그대로 사용하여 계산 과정을 생략할 수 있다.
  - 잘 훈련된 draft model이 있으면 **속도는 크게 오르면서 품질도 그대로 유지**할 수 있다. 

<div style="margin-top: 3.3em;"></div>

2-4. **Batching & Parallelization**
  - **batching**: 여러 요청을 한 번에 처리해서 효율을 높이는 방식이다.  
  - **parallelization**: 계산을 여러 프로세서/기기에서 동시에 처리하는 방법이다.

      <div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
        💬 병렬화에도 여러 방식이 있고, 각각 트레이드오프가 존재한다. 
      </div>
<br>


---
### 💭오늘 챙겨간 것들
추론 가속<small style="color:gray">(Inference Acceleration)</small>에는  
✔️ **조금의 정확도를 포기하고 속도를 올리는 방식**과  
✔️ **정확도는 그대로 두고 계산만 빠르게 하는 방식**  
두 가지가 있다는 걸 알게 되었다!  

서비스 목적 또는 사용하는 모델에 따라 **적절한 기술 조합을 고르는 것**이 중요할 듯 하다.

<div style="margin-top: 3.3em;"></div>

다음 글에서는 LLM이 **얼마나 다양한 분야에서 활약하고 있는지**에 대해 정리해보겠다. 
인트로 유닛의 마지막 글이 될 예정!  

<br><br>

