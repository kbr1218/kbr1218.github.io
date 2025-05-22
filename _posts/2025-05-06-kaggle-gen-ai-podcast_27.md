---
layout: post
title:  "[Kaggle Gen AI] Day 3 - 협업하는 AI들: 멀티 에이전트의 설계, 과제, 그리고 평가까지 🚀"
author: me
categories: [ Kaggle Gen-AI]
date: 2025-05-06 09:00:00
image: assets/images/20250402/day3_2.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_26/)에서는 실제 환경에서 에이전트를 안정적으로 운영하기 위해 필요한 **AgentOps, KPI 지표 설계, 자동화된 평가 시스템**에 대해 정리해봤다.  

간단한 데모 수준을 넘어서 **운영 가능한 agent 시스템을 만들기 위해 고려해야 할 요소들**을 함께 확인해봤고,

이번 글에서는 **멀티 에이전트 시스템**에 대해 정리해보겠다.  
> it's like assembling a team of experts, each with their own unique skill set.

여러 명의 에이전트를 조합해 문제를 나눠서 해결하고, 서로의 작업을 검증하거나 협력하거나, 때로는 경쟁도 하는 구조이다. 멋지군  

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  ⚠️ <strong>주의사항</strong><br>
  이번 팟캐스트에는 중간에 내용이 반복되거나 말이 끊기는 부분이 있다. <small style="color:gray">(AI로 만든 팟캐스트라더니 검토하는 걸 잊으셨나 봄)</small>
</div>
<div style="margin-top: 2.3em;"></div>

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=7rbSwt-7odQ&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=5)

<div style="margin-top: 2.3em;"></div>

---
### 🔧 멀티 에이전트 시스템이란?
**멀티 에이전트 시스템** <small style="color:gray">(Multi-Agent Systems)</small>의 기본 아이디어는 사실 단순하다고 한다.  

**한 명의 에이전트가 모든 걸 다 하려는 대신, 문제를 쪼개고 각각을 전담할 전문가들을 배치하는 것.**
> so it's like assembling a team of experts, each with their own unique skill set.

복잡한 문제를 더 작고 관리 가능한<small style="color:gray">(smaller, more manageable)</small> 태스크로 나누고, 각각의 에이전트가 **자신의 전문 역량을 살려** 역할을 맡는 구조이다. 

마치 현실 세계의 팀처럼, 잘 조율된 협업을 이루는 방식이다. 
> leveraging specialization much like you would with, a real world team, like a well-coordinated team.

<br>

---
### 👥 왜 하나보다 여럿이 나은가? 멀티 에이전트 시스템의 이점
그렇다면 이런 구조가 주는 이점은 무엇일까?

- ✅ **정확도 향상** <small style="color:gray">(better accuracy)</small>  
  **서로의 결과를 검증하고 보완**하므로 오류 가능성이 줄어든다.

<div style="margin-top: 2.3em;"></div>

- ✅ **병렬 처리로 효율 극대화** <small style="color:gray">(more efficient)</small>  
  여러 에이전트가 **동시에 작업**하면 속도는 훨씬 빨라진다.  
  > it's like having a team that can multitask on a whole new level. The AI multitasking.

<div style="margin-top: 2.3em;"></div>

- ✅ **확장성** <small style="color:gray">(very flexible)</small>  
  더 많은 계산이 필요하면 에이전트를 **추가하기만 하면 된다.**

<div style="margin-top: 2.3em;"></div>

- ✅ **내결함성** <small style="color:gray">(fault tolerance)</small>  
  어떤 에이전트 하나가 실패해도 시스템 전체가 멈추지 않는다.  
  > it's like a self-healing AI system.

이렇게 **정확성, 속도, 유연성, 안정성**까지 모두 챙길 수 있는 게 멀티 에이전트 구조의 강점이다.

<br>

---
### 🔍 신뢰할 수 있는 AI를 위한 구조: 체크 앤 밸런스
AI 시스템에서 자주 제기되는 문제 중 하나가 **hallucination** <small style="color:gray">(환각 현상)</small>이나 **편향** <small style="color:gray">(bias)</small>이다. 이러한 문제는 하나의 에이전트가 모든 판단을 내릴 때 특히 치명적이다. 

하지만 멀티 에이전트 구조에서는 서로의 판단을 **교차 검증**하고, 다양한 시각이 함께 작동하기 때문에 더 안전하다.

에이전트 여러 명의 관점을 결합하면,  **한 에이전트의 편향이나 오류가 전체 결과에 미치는 영향을 줄일 수 있다.**  
마치 의료에서 **두 번째, 세 번째, 네 번째 의견을 듣는 것처럼.**

> it's like getting a second opinion. Or a third or fourth opinion or a fifth.

즉, 멀티 에이전트 시스템은 단순히 빠르고 효율적인 것 이상의 의미를 넘어서, **더 견고하고, 더 신뢰할 수 있는 AI를 만드는 기반**이 된다. 

> they're also about, making the system as a whole more robust and reliable. it's about building trust.

<br>

---
### 🧱 멀티 에이전트 시스템은 어떻게 구성될까?
다음으로는 **이 에이전트들이 실제로 어떻게 함께 작동하는지**, 그러니까 **시스템 구조**를 살펴볼 차례이다.  
> so that's where design patterns come into play.

팟캐스트에서는 이 구조를 설명할 때, **디자인 패턴** <small style="color:gray">(Design Patterns)</small>이라는 개념을 언급한다.  
⇒ 멀티 에이전트 시스템을 설계하고 구성할 때 사용할 수 있는 **청사진 또는 모범 사례들**이라는 것!

> think of them as like blueprints for building and organizing your multi-agent team.

오랜 실험을 거쳐 효과가 검증된 대표적인 구조들을 하나씩 살펴보자면: 

<br>

##### 1️⃣ Sequential Pattern — AI 조립 라인
Sequential Pattern은 말그대로 **에이전트들이 순서대로 작동하는 구조**이다.  
앞 단계의 에이전트가 결과를 만들어내면, 그걸 다음 에이전트가 받아서 작업을 이어간다. 
> so like an assembly line but for AI.

- 각 에이전트는 **고유한 역할**을 담당하며,
- 복잡한 문제를 **단계별로 분해해서 처리하는 데 적합하다. 

<br>

##### 2️⃣ Hierarchical Pattern — 매니저 + 팀원 구조
**관리자 역할을 하는 에이전트**가 전체 워크플로우를 조율하는 구조이다.  
매니저 에이전트가 하위 에이전트들에게 **태스크를 분배**하고 **작업을 조정**한다. 
> it's a top-down approach where the manager agent delegates takes, and coordinates the overall workflow.

- 큰 문제를 **전략적으로 쪼개서 배분**해야 할 때 효과적이다.
- 현실 세계의 팀장과 팀원 관계처럼 동작!

<br>

##### 3️⃣ Collaborative Pattern — 평등한 팀워크
이 구조에서는 에이전트들이 **동등한 관계**로 협력한다.  
연구 프로젝트를 진행하는 과학자들처럼 정보와 리소스를 공유하면서 **공동의 목표를 함께 달성**하려고 한다. 
> like a team of scientists working on a research project. they're all in it together.

- 역할 구분 없이, 서로 **지속적으로 소통**하면서 문제를 해결한다. 
- **복잡한 탐색 문제나 창의적 과제**에 적합하다. 

<br>

##### 4️⃣ Competitive Pattern — AI끼리 경쟁하기
이 구조는 **여러 에이전트가 경쟁하면서 더 나은 해결책을 찾는 방식**이다.  
각자 독립적으로 문제를 풀고, 그 중 최고의 결과를 선택하는 구조이다. 

- 다양한 후보를 실험해보고 **최적의 해답**을 찾는 데 유리하다
  > this pattern can be really effective for optimization problems.
- e.g. 경로 탐색, 추천 시스템 등에서 활용 가능

<br>

##### 🛠️ 문제에 따라 구조도 달라진다
여러가지 종류의 디자인 패턴을 보면 알 수 있듯이, 멀티 에이전트 시스템은 **하나의 정답이 있는 게 아니라**, **문제의 성격과 목표에 따라 가장 적합한 패턴**을 선택하는 것이 중요하다.  

너무나도 당연한 말!
> different tools for different jobs.

어떤 경우에는 순차적으로, 어떤 경우에는 협력적으로, 때로는 계층 구조가, 때로는 경쟁 구도가 더 나은 해답이 될 수도 있따. 

<br>

---
### ⚠️ 멀티 에이전트 시스템의 현실적인 과제들
멀티 에이전트 시스템이 짱인 만큼, **새로운 어려움**도 함께 생긴다.  
팟캐스트에선 4가지 어려운 점을 짚어줬다:  

<div style="margin-top: 3.3em;"></div>

1. **태스크 할당** <small style="color:gray">(task allocation)</small>  
    어떤 에이전트에게 어떤 일을 맡길지 결정하는 게 생각보다 어렵다. 

    잘못된 에이전트에게 잘못된 일을 주면 **효율이 떨어질 수 있어서**,  
    각각의 에이전트가 가진 **강점을 살리는 설계**가 중요하다
    > you got to play to their strengths.

     <br>

2. **추론의 조율 문제** <small style="color:gray">(coordinating the reasoning processes)</small>  
    각각의 에이전트가 똑똑한 것도 중요하지만,  
    그들이 **하나의 팀처럼 유기적으로 작동하도록 만드는 일**도 특히 더 중요하다.
    > teamwork makes the dream work.

     <br>

3. **문맥량 관리** <small style="color:gray">(context volume)</small>  
    에이전트 사이에 오가는 정보량이 많아지면, 그걸 감당하는 것도 어려워진다.  
    너무 적으면 판단의 근거가 부족하고, 너무 많으면 과부하가 발생하므로 이 둘 사이의 **적절한 균형**이 필요하다. 
    > information overload is a real concern. it is even for AI.

     <br>
     
4. **비용** <small style="color:gray">(time and cost)</small>  
    현실 세계에서 가장 무서운 요소이다.  
    멀티 에이전트는 계산량이 많고, 클라우드 자원을 많이 쓰면 **비용도 빠르게 증가**한다. 

     <div style="margin-top: 3.3em;"></div>

    <div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
        💬 결국 에이전트 시스템은 <strong>복잡성과 효율성 사이에서 끊임없이 줄다리기를 해야 하는 구조</strong>다. <em style="color:gray">(it's a delicate dance.)</em>
    </div><br>

---
### 🧪 멀티 에이전트 시스템의 평가: 팀 전체를 보기
이 글의 마지막 주제, 멀티 에이전트 시스템을 어떻게 평가할 수 있을까?

기본적으로는 이전에 다뤘던 기법들인: [**trajectory analysis**, **final response evaluation**](https://kbr1218.github.io/kaggle-gen-ai-podcast_26/) 등을 여전히 활용할 수 있다.

하지만 팟캐스트에서는 **싱글 에이전트 시스템과는 다른 관점이 필요**하다고 말한다. 

- 각 에이전트가 **개별적으로 얼마나 잘 작동했는가**
- 그리고 **서로 얼마나 잘 협업했는가**

> you got to look at both the individual and the team performance.

<div style="margin-top: 2.3em;"></div>

단순하게 "잘했다👍🏻" vs. "못했다👎🏻"가 아니라, 다음과 같은 질문을 던져야 한다:  

- 에이전트들 간에 **의사소통은 잘 되었는가?** <small style="color:gray">(are they communicating clearly?)</small>
- 적절한 에이전트에게 **적절한 태스크가 할당되었는가?** <small style="color:gray">(are we using the right agents for the right tasks?)</small>
- 전체 팀이 **목표를 향해 유기적으로 움직였는가?** <small style="color:gray">(how effectively are the agents cooperating?)</small>

> it's all about understanding the dynamic of the team.

이러한 **팀 차원의 평가 방식**이 멀티 에이전트 시스템을 이해하고 개선하는 데 핵심이 된다.
> it's the future of AI.

<br>

---
### 💭 오늘 챙겨간 것들
이번 글에서는 **멀티 에이전트 시스템**의 개념부터 구조, 설계 패턴, 그리고 현실적인 과제와 평가 방식까지 살펴보았다.

✔️ 하나의 거대한 문제를 잘게 쪼개고, 각 에이전트가 전문성을 살려 역할을 수행하는 구조  
✔️ **정확도**, **속도**, **확장성**, **안정성**, **신뢰성**의 이점을 가진 접근 방식이라는 점  
✔️ 시스템 설계 시에는 Sequential / Hierarchical / Collaborative / Competitive 같은 다양한 패턴이 있다는 것  
✔️ 하지만, 태스크 할당, 추론 조율, 문맥 관리, 비용 문제 등 **현실적인 운영 과제**도 당근 있다는 것  
✔️ 멀티 에이전트 시스템은 **개별 에이전트 성능뿐 아니라 팀 전체의 협업 역학(dynamics)**까지 함께 평가해야 한다는 것!

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  💡 <strong>멀티 에이전트 시스템은</strong> 단순한 기능의 조합이 아니라, <strong>잘 설계된 협업 구조</strong>가 핵심이다.
</div>

다음 글에서는, 단순 검색 기반 RAG에서 한 단계 더 나아간, **Agentic RAG** 구조를 살펴볼 예정이다.

<br><br>