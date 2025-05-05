---
layout: post
title:  "[Kaggle Gen AI] Day 3 - 생성형 AI 에이전트, 모델을 넘어 목표를 향해 움직이는 시스템 🚀"
author: me
categories: [ Kaggle Gen-AI]
date: 2025-04-26 09:00:00
image: assets/images/20250402/day3_1.png
---
Day3에서는, **생성형 AI 에이전트** <small style="color:gray">(generative AI agent)</small>에 대해 본격적으로 들어가본다.  

그냥 텍스트만 생성하는 모델이 아니라, **도구를 사용하고, 외부 세계를 인식하고, 목표를 달성하기 위해 스스로 계획을 세우는 '행동하는 시스템'**에 가까운 개념이다.  

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 마치 우리가 책이나 검색엔진 같은 도구를 활용해 더 똑똑하게 움직이듯, 에이전트는 AI 모델이 디지털 세계에서 도구를 활용하여 '생각하고 움직이는 존재'가 되는 방식이다.
</div>

그래서 이번 팟캐스트에서는:  
✔️ 에이전트가 **무엇**인지,  
✔️ 어떤 **구성요소**로 이루어졌는지,  
✔️ **어떻게 작동**하는지,  
✔️ 그리고 이걸 **어떻게 쓸 수 있는지**를 다룬다.  

<div style="margin-top: 3.3em;"></div>

이번 글에서는 그중에서도
- 에이전트의 기본 개념
- '모델, 도구, 오케스트레이션 레이어'로 이루어진 AI Agent의 핵심 구조
- 전통적인 LLM과의 차이점  

을 중심으로 살펴보려고 한다. 

<div style="margin-top: 3.3em;"></div>

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=D3Kaqz7VW28&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=5)

<div style="margin-top: 1.3em;"></div>

---
### 🧩 생성형 AI 에이전트란?
**에이전트** <small style="color:gray">(Agent)</small>는 일반적인 생성형 AI 모델과는 조금 다른 개념이다.  

에이전트는 단순히 텍스트나 이미지를 만드는 것을 넘어서, **스스로 목표를 설정하고, 외부 정보를 활용하며, 필요한 도구를 써서 문제를 해결**하려고 한다.  
> so it's not just creating text or images, it's actually trying to achieve a special goal.

이 말은, 에이전트는  
- 외부 세계를 **관찰**하고 <small style="color:gray">(observing the world around it)</small>
- 그걸 바탕으로 **행동**하고 <small style="color:gray">(taking actions based on what it sees)</small>
- 필요한 경우 **툴을 호출하거나 정보를 검색**하면서 <small style="color:gray">(using tools it has available to it)</small>
- **스스로 목표 달성에 필요한 경로를 결정**할 수 있다는 뜻이다.  <small style="color:gray">(be given a goal and then operate independently)</small>

미리 정해진 명령어만 따르는 게 아니라, **어떤 문제를 해결해야 하는지만 주어지면** 그걸 위해 **필요한 단계를 스스로 계획하고 실행**하는 구조이다.  
> and it can even figure out the steps it needs to take to reach that goal, even if you haven't told it exactly what to do at every step.

이게 바로 '에이전트'가 단순한 생성형 모델과 구분되는 지점이다.  

<br>

---
### 🧭 다루는 범위 – 언어 모델 기반의 에이전트
이번 팟캐스트에서 말하는 '에이전트'는 굉장히 넓은 개념처럼 보이지만, 초점은 명확하다. 
바로 **생성형 언어 모델 (LLM)**을 **의사결정의 중심에 둔 에이전트**를 다룬다.  
> so the language model is the brain of the operation.

그러니까 여기서 말하는 에이전트란, 
- 핵심 '지능' 역할을 **LLM이 수행하고**,
- 그 LLM이 외부 정보와 도구를 활용해 **의사결정과 행동을 주도**하는 시스템이다.   

<div style="background: #f0f8ff; border-left: 4px solid #0277bd; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  💡 여기서 중요한 건, 단순한 API 호출 파이프라인이 아니라, <strong>“LLM이 중심이 되어 판단하고 행동하는 구조”</strong>라는 점이다.
</div><br>

---
### 🤖 에이전트의 작동 – Cognitive Architecture 살펴보기
에이전트를 제대로 이해하려면, 그 **내부 구조**인 **Cognitive Architecture**를 살펴볼 필요가 있다.  

일종의 **내부 운영체제** 같은 개념으로, 에이전트가 **어떻게 사고하고** <small style="color:gray">(makes decisions)</small>, **결정하고** <small style="color:gray">(behaves)</small>, **행동하는지** <small style="color:gray">(takes action)</small>를 구성하는 요소들로 이루어져 있다.
> it's the set of components that determine how the agent behaves, how it makes decisions and how it actually takes action.

<div style="flex: 3; min-width: 120px;">
    <img src="/assets/images/20250427/aiagent1.png" 
        alt="general agent architecture and compoents" 
        style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
    <small style="color:gray; display: block; margin-top: 8px;">
      General agent architecture and components
    </small>
</div>
<div style="margin-top: 3.3em;"></div>

위 그림을 보면 알 수 있듯이 에이전트의 구조는 크게 세 가지로 이루어져있다:
- **🧠 모델**
- **🛠️ 도구**
- **🕹️ 오케스트레이션 레이어**

<br>

---
##### 1️⃣ 모델 (Model) — 에이전트의 '두뇌'
가장 먼저, 모델은 에이전트의 중심이 되는 **언어 모델 (LLM)** 자체를 의미한다. **의사결정의 주체**이자, 에이전트가 사고하고 판단하는 부분이다. 
> this is the heart of the agent's intelligence

- 하나의 언어 모델이 사용될 수도 있고,
- **서로 다른 크기와 전문성을 가진 여러 모델이 협업**하는 형태일 수도 있다.

특히 중요한 점은, 이 모델들이 단순히 텍스트만 생성하는 것을 넘어 **ReAct**, **Chain of Thought (CoT)**, **Tree of Thoughts** 같은 **추론 프레임워크**를 통해 **단계적으로 사고하고, 전략적으로 문제를 해결**할 수 있어야 한다는 점이다.  

<div style="margin-top: 3.3em;"></div>

그리고 모델의 형태에 대해 세 가지 가능성도 함께 언급한다:
-  **범용 모델**<small style="color:gray">(general-purpose)</small>
- 텍스트·이미지 등 여러 모달리티를 다루는 **멀티모달**<small style="color:gray">(multimodal)</small> 모델
- 특정 API나 작업에 최적화된 **파인튜닝**<small style="color:gray">(fine-tuning)</small> 모델

이런 유연성 덕분에, 에이전트의 사용 목적에 따라 가장 적합한 모델을 선택할 수 있다. 

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 예를 들어, 어떤 API와 자주 상호작용해야 하는 에이전트라면, <strong>그 API 사용 패턴을 학습한 모델이 훨씬 적합</strong>하다. <small style="color:gray"><em>(the model would perform better if it's already encountered data that reflects how that API is typically used.)</em></small>
</div>

하지만 대부분의 언어 모델은 처음부터 에이전트 구조에 맞춰 학습된 것이 아니기 때문에, **어떤 도구를 쓸 수 있는지 / 어떻게 작업을 나눠야 하는지에 대한 사전 지식은 없다.**  
→ 그래서 **사용 예시나 학습 데이터를 기반으로 파인튜닝**하거나, **툴 사용법을 명시한 프롬프트를 통해 학습**시키는 것이 중요하다.

<br>

---
##### 2️⃣ 도구 (Tools) — 에이전트의 '손'
모델이 아무리 똑똑하더라도, 그 자체로는 세상에 영향을 줄 수 없다. **실제 행동을 가능**하게 하기 위해 **도구**가 필요하다.  

도구는 에이전트가  
- 외부 정보를 검색하거나
- 데이터를 생성/수정/삭제하거나
- 시스템과 직접 상호작용할 수 있게 만든다.

> so like if an agent needs to update a customer record or fetch some data from a website, it would use a tool for that.

도구의 형태는 정말 다양하지만, 기본적으로는 대부분 **웹 API와 비슷한 구조**로 작동한다.  

특히 **RAG** <small style="color:gray">(Retrieval-Augmented Generation)</small> 같은 고급 구조에서는 **도구를 통해 외부 지식을 검색**하는 것이 핵심이다.

<br>

---
##### 3️⃣ 오케스트레이션 레이어 (Orchestration Layer) — 에이전트의 '컨트롤 타워'
모델이 판단하고, 도구가 실행한다면, 그 사이를 연결하고 전체 프로세스를 **조율**하는 건 바로 **오케스트레이션 레이어**이다. 
> the orchestration layer, this is like the control center of the agent.

이 레이어는 다음과 같은 역할을 맡는다:
- 에이전트가 받은 정보를 **어떻게 처리할지 결정**하고, <small style="color:gray">(takes in information)</small>
- 모델의 추론을 통해 나온 결과를 **어떤 도구로 실행할지 선택**하고, <small style="color:gray">(reasons about that information)</small>
- 전체 작업이 완료될 때까지 **반복적으로 사이클을 돌면서 상태를 관리**한다. <small style="color:gray">(uses that reasoning to decide what to do next)</small>

> so it's making sure the model and the tools are working together smoothly and effectively.

<div style="margin-top: 3.3em;"></div>

Orchestration은 간단한 구조일 수도 있고, **복잡한 계산** <small style="color:gray">(a series of calculations)</small>, 조건 분기 <small style="color:gray">(involved logic)</small>, 그리고 **다른 ML 알고리즘과의 결합** <small style="color:gray">(using other machine learning algorithms)</small>까지 포함할 수 있다.  

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 결국, <strong>에이전트가 얼마나 유연하고 정교하게 작동하느냐는 이 레이어의 설계에 달려 있다.</strong>
</div>
<div style="margin-top: 3.3em;"></div>

이렇게 모델, 도구, 오케스트레이션 레이어 세 가지를 조합하면, **목표를 인식하고, 계획을 세우고, 외부와 상호작용하며 행동하는 시스템**인 **생성형 AI Agent**가 완성된다. 

<br>

---
### 🔍 모델 vs 에이전트 — 무엇이 다른가?
다음으로 팟캐스트에서는 **전통적인 LLM과 AI 에이전트 사이의 차이점**도 상세하게 짚고 넘어간다.  

처음엔 에이전트가 그냥 "도구가 붙은 모델" 처럼 보일 수도 있지만, 실제로는 훨씬 더 **구조적으로 깊은 차이**가 있다.   

<div style="margin-top: 3.3em;"></div>

##### 1️⃣ 지식의 한계 vs. 실시간 확장성
전통적인 LLM은  **훈련 당시의 데이터**만을 기반으로 작동하기 때문에, 최신 정보에 대해선 알지 못하거나 엉뚱한 답을 내놓기도 한다.  

하지만 에이전트는 **도구를 통해 실시간 정보를 검색하거나 수집**할 수 있다.  
⇒ **지식의 한계를 뛰어넘고 계속 '학습'하듯 확장**해나갈 수 있다는 점이 결정적이다. 
> so it's not limited by its initial training data. it can access up-to-date information from the outside world. 

<div style="margin-top: 3.3em;"></div>

##### 2️⃣ 단발성 vs. 기억 기반 상호작용
기존 모델은 **단일 입력 → 단일 출력** 구조이다. 과거 대화를 기억하거나 맥락을 유지하진 않는다.  

하지만 에이전트는 **전체 상호작용의 히스토리를 기억**하고, 그걸 기반으로 행동한다.  
⇒ **multi-turn conversations**, 즉 여러 차례의 대활르 이어가며 맥락을 유지하고 **대화가 이어질수록 더 나은 판단**을 하게 된다. 
> a model typically makes a single prediction based on the input you give it, but an agent can keep track of the whole history of the interaction

<div style="margin-top: 3.3em;"></div>

##### 3️⃣ 툴 지원의 유무
기본 LLM에는 **툴 사용에 대한 구조적 지원**이 없다.  하지만 에이전트는 **툴 사용이 아예 구조에 내장**되어 있다.  
⇒ 검색, 실행, 수정 등 다양한 액션을 자연스럽게 수행할 수 있다.  
> tools are an essential part of an agent's architecture.  

<div style="margin-top: 3.3em;"></div>

##### 4️⃣ 프롬프트 유도 vs. 내장된 추론 로직
전통적인 모델도 프롬프트를 잘 짜면 꽤 똑똑해질 수 있지만 **논리적 추론이나 단계적 사고는 여전히 제한적**이다.  

에이전트는 내부에 **ReAct, Chain of Thoughts 같은 reasoning framework**가 내장되어 있다.  
⇒ '툴을 잘 쓰는 것'이 아니라 **'전략적으로 쓸 수 있는 구조'**를 가지고 있다는 것이 큰 차이!
> so it's not just about having the tools. it's about having the built-in intelligence to use those tools strategically.

<br>

---
### 💭 오늘 챙겨간 것들
이번 글에서는 생성형 AI 에이전트에 대해서 알아봤다. 생성형 AI 에이전트는 단순히 텍스트를 출력하는 모델이 아니라,
✔️ 목표를 이해하고,  
✔️ 도구를 사용하며,  
✔️ 외부 세계와 상호작용하고,  
✔️ 계획을 세워 행동하는 **‘지능형 시스템’**이라는 점!  

그리고 에이전트의 Cognitive Architecture에서 세 가지 구성 요소를 간단히 말하자면:  
**모델(Model)**은 사고와 판단을 담당하고, **도구(Tools)**는 실제 행동을 가능하게 하며, **오케스트레이션 레이어(Orchestration Layer)**는 이 모든 흐름을 조율하는 에이전트의 운영체제 역할을 한다.  

또한, 생성형 AI 에이전트는 단순히 LLM 위에 도구를 덧댄 것이 아님!  
✔️ 실시간 정보 접근  
✔️ 기억을 기반으로 한 상호작용  
✔️ 도구 사용을 전제로 한 구조  
✔️ 그리고 내장된 추론 프레임워크  
가 일반적인 모델과의 큰 차이점이었고, 이것들이 결합되어 **훨씬 더 유연하고 지능적인 시스템**이 될 수 있다.

<div style="margin-top: 3.3em;"></div>

다음 글에서는 이 에이전트들이 실제로 어떻게 **정보를 받아들이고 판단하고 실행하는지**, 그리고 그 과정을 가능하게 하는 **ReAct / Chain of Thought / Tree of Thoughts 같은
추론 프레임워크**를 중심으로 정리해보겠다. 바이바이

<br><br>