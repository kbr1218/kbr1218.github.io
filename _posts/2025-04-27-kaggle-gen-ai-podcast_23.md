---
layout: post
title:  "[Kaggle Gen AI] Day 3 - Cognitive Architecture: AI 에이전트의 작동 원리 들여다보기 🚀"
author: me
categories: [ Kaggle Gen-AI]
date: 2025-04-27 09:00:00
image: assets/images/20250402/day3_1.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_22/)에서는 **에이전트란 무엇인지** 그리고 모델, 도구, 오케스트레이션 레이어로 구성된 **에이전트의 기본 구조**를 정리해봤다.  

에이전트는 그냥 언어 모델에 기능을 덧붙이는 게 아니라 **목표 지향적으로 계획하고 행동하는 시스템**이다.  

이번 글에서는 에이전트가 **실제로 어떻게 '생각하고 움직이는지'**, 즉 내부에서 어떤 순서로 정보를 받아들이고, 판단하고, 도구를 활용해 목표에 도달하는지를 구체적으로 살펴보겠다!  

✔️ 이걸 가능하게 만드는 **Cognitive Architecture**,   
✔️ 그리고 ReAct, Chain of Thoughts, Tree of Thoughts 같은 **추론 프레임워크**들을 중심으로 정리해볼 예정  

<div style="margin-top: 3.3em;"></div>

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=D3Kaqz7VW28&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=5)

<div style="margin-top: 2.3em;"></div>

---
### 🍳 셰프처럼 생각하는 에이전트? — Cognitive Architecture의 핵심
이번 파트에서는 에이전트의 작동 원리인 **Cognitive Architecture**에 대해 좀 더 깊이 들어간다.  

팟캐스트에서는 이를 설명하기 위해 흥미로운 비유 하나를 사용하는데, 바로 **바쁜 주방의 셰프**<small style="color:gray">(a chef in a busy kitchen)</small>로 에이전트를 설명한 부분이다.   
> so think about it. a chef doesn't just throw ingredients together randomly - they follow a process: they get the order, they see what ingredients they have, then they think about what dishes they can make, they chop, mix, cook... and they constantly adjust based on what's happending.

<div style="margin-top: 3.3em;"></div>

셰프가
- 주문을 받고,
- 재료를 확인하고,
- 요리 순서를 계획해서
- 조리하고,
- 중간 상황을 보며 계속 조정하는 것처럼  

에이전트 역시 다음과 같은 사이클을 반복하며 작동한다: **계획 → 행동 → 관찰 → 조정**  

이런 흐름을 가능하게 하는 게 바로 **Cognitive Architecture**이며, 이 구조 덕분에 에이전트는  
✔️ **정보를 받아들이고** <small style="color:gray">( taking in information)</small>  
✔️ **추론하고** <small style="color:gray">(reasoning about it)</small>  
✔️ **결정을 내리고** <small style="color:gray">(making decisions)</small>  
✔️ **행동하며** <small style="color:gray">(taking actions)</small>   
✔️ 그 결과를 바탕으로 **다시 학습**하는 <small style="color:gray">(learning from the results)</small>  
지속적 순환 루프를 수행할 수 있다.

<br>

---
### 🕹️ 모든 흐름의 중심: Orchestration Layer
이 사이클의 중심에는 바로 **오케스트레이션 레이어**가 있다.  

에이전트가 지금까지 어떤 일을 해왔는지, 어떤 상태인지, 그리고 다음에 어떤 결정을 내려야 할지를 계속 **추적하고 조율하는 역할**을 한다.  
> that layer is responsible for keeping track of everything that's happened, the current state of the task, and then managing the reasoning and planning.

즉, **모델이 판단**하고, **도구가 행동**한다면,  
오케스트레이션 레이어는 **에이전트의 '흐름'을 책임지는 컨트롤 타워** 역할이라고 볼 수 있다.  

<br>

---
### ✍🏻 Prompt Engineering의 역할도 중요하다
또 하나 중요한 포인트는 바로 **Prompt Engineering**이다.  

에이전트의 사고 과정을 어떻게 유도하느냐에 따라 전체 행동의 흐림이 달라지기 때문이다.  
> prompt engineering is all about crafting those really effective instructions for language models and that's crucial for guiding the agent's thought process.

단순히 명령을 주는 수준을 넘어서, **언어 모델이 어떤 식으로 사고하고 행동할지 방향을 제시해주는 설계**가 필요하다. 

<br>

---
### 🛠 에이전트의 사고력을 키우는 추론 프레임워크들
에이전트가 정보를 받아들이고 행동하는 데는 모델과 도구만으로는 부족하다. **어떻게 사고할지 유도하는 '프레임워크'**가 함께 작동해야 한다.  

여기서는 대표적인 세 가지 프레임워크를 소개한다.  
<small style="color:gray">이전 Unit 1-Prompt Engineering에서 이미 한 번 다뤘던 개념들이다. (각 프레임워크 이름을 누르면 이전 게시물로 이동!)</small>

- [ReAct](https://kbr1218.github.io/kaggle-gen-ai-podcast_13/)
- [Chain of Thoughts (CoT)](https://kbr1218.github.io/kaggle-gen-ai-podcast_12/)
- [Tree of Thoughts (ToT)](https://kbr1218.github.io/kaggle-gen-ai-podcast_13/)  


<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  💡 <strong>프레임워크? 프롬프트 엔지니어링 기법?</strong><br><br>
  ReAct, CoT, ToT는 단순한 기능이 아니라, 에이전트가 <strong>어떻게 사고하고 행동할지 정해주는 틀</strong>이다.<br>
  그래서 보통은 '추론 프레임워크'라고 부르지만, 실제 구현은 대부분 <strong>프롬프트를 통해</strong> 이루어지기 때문에 
  <strong>프롬프트 엔지니어링 전략</strong>으로도 볼 수 있다고 한다!<br>
  👉🏻 결국 <strong>"프레임워크이자 프롬프트 설계 기법"인 셈</strong>
</div>

<div style="margin-top: 3.3em;"></div>

##### 1️⃣ ReAct - Reason + Act
ReAct는 이름 그대로 **Reason(추론)하고 Act(행동)**하는 두 단계를 명시적으로 분리해 사고 과정을 유도하는 프레임워크이다.  
> so it's not just about getting a final answer, it's about showing the user how the agent got there

- 모델이 먼저 **생각을 정리**하고 <small style="color:gray">(think about what needs to be done)</small> 
- **필요한 도구를 호출**해서 행동하며 <small style="color:gray">(use a tool and see what happens)</small>  
- 그 결과에 따라 다시 **추론을 이어가는 방식**이다. <small style="color:gray">(reason based on that result)</small>  

이런 흐름은 결과의 **신뢰성과 투명성**<small style="color:gray">(transparent and trustworthy)</small>을 높이는 데에도 효과적이다.  

사용자는 단순한 답이 아니라, **어떻게 그 답에 도달했는지의 사고 과정**을 함께 볼 수 있기 때문이다. 

<br>

##### 2️⃣ Chain of Thoughts (CoT) - 논리적 단계를 따라 사고하기
CoT는 문제 해결 과정을 **단계별로 글로 풀어내는 방식**이다.  
답을 바로 내기보단, 모델이 **논리적인 사고 흐름을 서술**하도록 유도한다.  
> it's like showing your work in a math problem

"답이 뭐야?"라고 직접적으로 묻기 보다는 **"왜 그게 답인지 단계별로 설명해줘"**에 가깝다.  

여기서도 여러 변형이 등장한다:
- [**Self-Consistency**](https://kbr1218.github.io/kaggle-gen-ai-podcast_12/)  
    <small style="color:gray">(얘도 Unit 1-Prompt Engineering에서 간단하게 다룬 기법이다.)</small>
- **Active Prompting**
- **Multimodal CoT**, etc.

> so different flavors of CoT for different tasks

태스트에 따라 다양한 버전의 CoT를 사용할 수 있다는 것도 장점이다.  

<br>

##### 3️⃣Tree of Thoughts (ToT) — 사고의 확장을 위한 브랜칭
ToT는 CoT를 더 확장한 형태로, CoT같이 한 줄기 사고가 아니라 **여러 가능성을 탐색**하는 방식이다.  
> so instead of just a single chain of thought, it's like branching out, exploring multiple paths.

예를 들어, 체스를 둘 때, 한 수 앞만 보는 게 아니라 여러 수를 시뮬레이션하며 가능한 전략을 평가하듯, ToT는 **복잡한 문제를 구조적으로 탐색**하는 데 효과적이다.  

<br>

---
### ✈️ ReAct 예시 — 비행기 예약하기
ReAct가 실제로 어떻게 작동하는지도 간단한 예시를 통해 알려주고 있다.  

우선 사용자가 원하는 태스크가 **비행기 예약**이라면:  

0. **🧾 사용자: 비행기 예약을 도와줘!**  

1. 에이전트는 먼저 **명확히 이해하려고** 질문한다:
    > "어디에서 출발하시나요?" <small>(where are you flying from?)</small>

2. 그 다음, **어떤 방식으로 정보를 찾을지 판단**한다:
    > 예를 들어) 항공 API 같은 **외부 도구를 호출**해 정보를 얻기로 결정 <small>(decides to use a flight API)</small>

3. API 호출 결과를 받아오고 → 이를 **관찰**<small style="color:gray">(Observation)</small>한다:

4. 그 정보를 기반으로 다시 **추론 → 추가 질문**한다:
    > 예를 들어) "원하는 날짜가 있으신가요?"  <small>(ask the user about their preferred travel dates)</small>

이 과정은 여러 차례 반복되며, **질문 → 사고 → 도구 사용 → 관찰 → 다시 추론**의 루프로 이어진다. 
> so it's like a back and forth conversation between the agent and the user with the agent using tools and reasoning to gradually gather all the necessary information

<div style="flex: 3; min-width: 120px;">
    <img src="/assets/images/20250427/aiagent2.png" 
        alt="agent with ReAct" 
        style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
    <small style="color:gray; display: block; margin-top: 8px;">
      Example agent with ReAct reasoning in the orchestration layer
    </small>
</div>
<div style="margin-top: 1.3em;"></div>

이 전 과정을 시각적으로 정리한 플로우 다이어그램을 보면 ReAct의 작동 흐름을 쉽게 이해할 수 있다.

<div style="margin-top: 3.3em;"></div>
<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 결국, 에이전트가 얼마나 정확한 답을 낼 수 있느냐는 <strong>모델이 얼마나 잘 추론하고, 자신이 사용할 수 있는 도구를 얼마나 효과적으로 사용하는가</strong><small style="color:gray">(how well the model can reason and how effectively it can use the tools it has available)</small>에 달려있다.
</div>
<div style="margin-top: 3.3em;"></div>

**도구**에 대한 이야기가 나온 만큼, 다음 글에서는 에이전트가 어떤 도구를 활용하고, 그 도구들이 어떤 역할을 하는지 정리해볼 예정이다. 🔧

<br>

---
### 💭 오늘 챙겨간 것들
이번 글에서는 에이전트가 어떻게 사고하고, 계획하고, 실행하는지 그 과정을 구성하는 핵심 개념인 **Cognitive Architecture**에 대해 정리했다

✔️ 셰프처럼 계획 → 행동 → 관찰 → 조정을 반복하며  
✔️ 정보를 받아들이고, 추론하고, 결정을 내리고, 도구를 활용하고, 그 결과를 학습하는  
지속적인 사고 루프를 수행하는 구조!

또한 이 사고 과정을 유도하는
- ReAct,
- Chain of Thought,
- Tree of Thoughts  

같은 프레임워크들이 어떻게 작동하고, 얘네들이 왜 전략적 추론 방식을 만드는 설계 방식인지도 함께 살펴봤다.

<div style="margin-top: 3.3em;"></div>

다음 글에서는 에이전트가 사고력을 현실 세계와 연결해주는 핵심 요소인 **“도구(Tools)”**에 대해 다룰 예정이다. 바이바이

<br><br>