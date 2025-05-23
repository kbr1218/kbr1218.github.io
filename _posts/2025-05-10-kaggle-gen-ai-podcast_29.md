---
layout: post
title:  "[Kaggle Gen AI] Day 3 - 에이전트를 개발한다는 것: Agent Builder와 'AI 계약'의 미래 🚀"
author: me
categories: [ Kaggle Gen-AI]
date: 2025-05-10 09:00:00
image: assets/images/20250402/day3_2.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_27/)에서는 **Agentic RAG**라는 고도화된 검색 구조와 함께, 멀티 에이전트 시스템이 실제로 어떻게 적용되고 있는지 몇 가지 사례들을 통해 살펴봤다.  

이번 글에서는, **에이전트를 직접 개발하고, 관리하고, 평가하는 도구**인 **Agent Builder**는 어떤 모습일지, 그리고 **"AI와 계약을 맺는다"**는 건 무엇인지 알아보기로~

팟캐스트에서는 **Agent Builder 도구들**과 함께, AI에게 책임과 신뢰를 요구하게 되는 **'에이전트 계약' 개념**을 이야기하며 **AI가 인간과 어떻게 더 깊이 협력할 수 있는가**에 대한 새로운 패러다임을 제시하고 있다. 

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  ⚠️ <strong>주의사항</strong><br>
  이번 팟캐스트에는 중간에 내용이 반복되거나 말이 끊기는 부분이 있다. <small style="color:gray">(AI로 만든 팟캐스트라더니 검토하는 걸 잊으셨나 봄)</small>
</div>
<div style="margin-top: 2.3em;"></div>

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=7rbSwt-7odQ&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=5)

<div style="margin-top: 3.3em;"></div>

---
### 📄 Agents as Contractors — AI와 계약을 맺는다는 것
이번 파트는 기술적인 부분보단 **철학적인 이야기**가 나오는데,  
에이전트가 점점 더 **자율적이고 복잡한 역할**을 수행하게 되면서, 단순한 지시만으로는 부족하다는 주장이다. 

예를 들면, 그냥 집 청소하라고 말하는 것과, 청소 범위·기한·기준을 모두 상세히 정리한 계약서를 주는 것과는 차원이 다르다.
> it's like the difference between telling someone clean the house vs. giving them a detailed contract that outlines exactly what needs to be done, by when, and to what standard.

이제는 "이렇게 해줘" 수준의 명령이 아니라, **명확한 기대치와 평가 기준**, 실패 시 **책임 규정까지 포함된 '계약적 구조'**가 필요하다는 것이다.

<br>

##### 🤔 ‘AI 계약’이란?
팟캐스트에서는 이런 질문을 던진다:
> what would an AI contract even look like?

그냥 API를 호출하거나 프롬프트를 넘기는 게 아닌,  
- 무엇을 해야 하고, <small style="color:gray">(what you need)</small>  
- 어떤 기준을 만족해야 하며 <small style="color:gray">(setting clear performance benchmarks)</small>  
- 실패 시 어떤 책임이 따르는지 <small style="color:gray">(specifying penalties for failing)</small>  

이 모든 것이 명시된 **'계약' 같은 인터페이스**가 필요하다는 생각이다.  
→ **신뢰성과 책임을 담보**하기 위한 구조인 셈

<br>

##### ⚖️ 왜 이 개념이 중요한가?
에이전트가 단순 추천 시스템이 아니라 'controlling a self-driving car', 'managing critical infrastructure'같은  
**고위험 시스템에 통합되기 시작**하면서, 우리는 그 결과에 **책임을 물을 수 있어야 한다.  

그래서 이 개념은 **AI 시스템과 신뢰를 구축하기 위한 핵심 패러다임**으로 다뤄지고 있다. 

<br>

---
### 🧠 새로운 패러다임: 계약, 투명성, 그리고 신뢰
"에이전트를 계약자로 본다"는 아이디어는 **AI가 우리 삶 깊숙이 들어오고 있다는 사실**에서 출발한, **근본적인 패러다임의 전환**이라고 강조한다.
> it's a whole new paradiam and this shift is driven by this need for greater accountability and transparency.

에이전트가 점점 더 많은 책임을 맡게 될수록, 우리는 다음 질문에 답해야 한다:  
- 이 에이전트를 **어떻게 신뢰**할 수 있을까?
- **무슨 기준으로 평가**하고, **문제가 생기면 어떻게 대응**할 것인가?

<br>

##### 🧾 계약은 명확한 기대와 책임을 위한 도구
에이전트에게 점점 더 많은 의사결정을 맡기게 된다면,  
- 그 결정이 **어떻게 만들어졌는지**를 투명하게 볼 수 있어야 하고,
- **그 결과에 대해 누가 책임지는가**도 명확해야 한다.

기대치를 정하고, 성능을 모니터링하고, 문제가 생기면 해결할 수 있는 장치들이 필요하다. 
> we need mechanisms to define expectations, monitor performance, and resolve issues. it's about setting clear boundaries.

이때, '계약'은 **기대치를 형식화**하고, 에이전트가 **우리의 목표와 일치하도록 만드는 장치**가 될 수 있다.  

<br>

##### 🤝🏻 현실 세계와의 평행이론
생각해보면, 우리가 **사람 간의 복잡한 관계**에서 계약을 쓰는 이유도 같다
- 누가 뭘 해야 하는지
- 언제까지 해야 하는지
- 실패했을 때 어떻게 책임지는지

→ AI에게도 이 원리를 그대로 적용해보자는 발상이다.

> we're taking the principles where contracts govern human relationships, and applying them to the world of AI.

에이전트가 점점 자율성과 권한을 갖게 되는 지금, **신뢰와 책임을 구조적으로 확보할 수 있는 프레임워크**가 필요하며,  
그 시작이 바로 **'계약적인 사고방식'**이라는 것이다.  

<br>

---
### 🤹🏻‍♀️ 신뢰할 수 있는 에이전트를 만들기 위한 4가지
이제 마지막 주제로 넘어간다: 우리가 점점 더 많은 책임을 AI에게 맡기려고 할 때, **어떻게 하면 AI를 신뢰할 수 있을까?**

> it's one thing to trust an AI to you know recommend a movie but it's quite another to trust it to make life or death decisions.

팟캐스트에서는 이를 위해 꼭 필요한 네 가지 핵심 요소를 소개한다:

<br>

##### 1️⃣ Transparency — 내부가 보여야 신뢰할 수 있다
> the more we understand how an agent works, the more likely we are to trust its decisions.

투명성은 말 그대로 **“에이전트가 왜 그런 결정을 내렸는지 알 수 있게 해주는 것”**이다.  
에이전트가 **어떻게 판단을 내렸는지**, **어떤 경로를 통해 결론에 도달**했는지,  
→ 이걸 **사용자가 확인할 수 있어야 한다.**

> it's like lifting the hood and letting users see how the sausage is made. creating a paper trail, a trail of breadcrumbs.

그 방식은 다양할 수 있다:  
- **결론 도출 과정에 대한 설명**을 제공하거나   
- 사용자가 직접 **에이전트의 행동을 감사하고, 결정 과정을 추적**할 수 있게 하거나  

> so it's like creating a paper trail. a trail of breadcrumbs for the AI so we can understand why it did what it did.

그렇게 되면, **AI가 왜 그렇게 행동했는지** 알 수 있고 “왜 그랬는지 모르겠음”이라는 **불신의 여지를 줄일 수 있다.**

특히 중요한 건, 이 투명성이 **문제가 생겼을 때 더더욱 제 역할을 한다**는 것.

> even the best AI systems are going to make mistakes. when that happens, we need to be able to understand what happened, and how to prevent it.

→ 문제의 원인을 추적하고, 다시는 같은 실수를 하지 않도록 개선할 수 있는 기반이 된다.  
**투명성은 결국, 지속적인 개선과 신뢰의 출발점**이다.

<br>

##### 2️⃣ Accountability — 결정엔 책임이 따른다
투명성은 **과정**을 보여주는 것이고, **책임성은 결과**에 책임지는 것이다.  
> just because a machine made the decision doesnt't mean that no one is responsible.

AI가 자율적으로 결정을 내리는 시대일수록, **결과에 대한 책임 소재**는 더더욱 분명해져야 한다.  
→ 누가 책임질 건지, **책임의 선을 명확히 긋는 구조**가 필요하다는 얘기.

<div style="margin-top: 3.3em;"></div>

여기서는 책임성을 확보하는 두 가지 현실적인 메커니즘을 소개한다:  
1. **모니터링 및 감사 시스템 구축** <small style="color:gray">(monitoring and auditing agent behavior)</small>   
    - 에이전트의 행동을 기록하고,
    - 누가 어떤 결정을 내렸는지 추적할 수 있게 만드는 것이다.

2. **법적 프레임워크 마련** <small style="color:gray">(developing legal frameworks)</small>   
    - 에이전트가 잘못된 결정을 했을 때,
    - **개발자나 운영자가 법적으로 책임을 지도록 하는 구조**이다.

> it's like there are consequences for AI agents that behave badly,  just like there are consequences for humans who break the rules. because actions have consequences. They do for both humans and AI.

결국 AI도 **행동에는 결과가 따른다**는 원칙 위에서 움직여야 하며, 이것이 바로 **신뢰를 만들기 위한 최소 조건**이다.  

<br>

---
##### 3️⃣ Robustness — 예측불가 상황에서도 무너지지 않기
robustness, 강건성은 **예상치 못한 상황이나 입력이 들어왔을 때** 시스템이 오작동하거나 비정상적인 행동 없이 제대로 **대응할 수 있는 능력**을 말한다.

현실은 계획대로 흘러가지 않는다. 그래서 **강한 AI보다, 유연한 AI**가 필요하다.
> so it's like can it roll with the punches? (버티는 힘이 있는가?) can it adapt and respond appropriately to unforeseen circumstances? (예기치 못한 상황에서도 유연하게 반응할 수 있는가?)

에이전트가 **조금의 혼란과 불확실성 속**에서도, 당황하지 않고 **제대로 반응할 수 있어야** 한다. 

<div style="margin-top: 3.3em;"></div>

그렇다면 이 강건성을 키울 수 있는 방법은?
- **다양한 시나리오로 테스트** test them in a wide range of scenarios
- **failsafe(안전 장치)** 적용  Incorporate failsafe mechanisms
- 경험을 통해 **학습하고 적응**하는 구조 설계 design them with the ability to learn and adapt from their experiences

> it's about making them not only smart, but also resilient and adaptable. they need to be able to think on their feet.

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
💬 결국 중요한 건, 단순히 똑똑한 AI가 아니라, <strong>회복력 있고, 적응력 있으며, 순발력 있는 AI</strong> 를 만드는 것이며, 그 AI는 <strong>즉석에서 판단하고 행동</strong>할 수 있어야 한다.
</div>
<div style="margin-top: 3.3em;"></div>

---
##### 4️⃣ Fairness — AI는 세상을 더 나은 곳으로 만들어야 한다
공정성이란, AI 에이전트가 특정 집단을 차별하거나 기존의 편항을 그대로 재현하지 않도록 하는 것을 의미한다.  

AI가 **차별을 재생산하지 않고**, **특정 집단에 불공정하게 작동하지 않도록** 설계되어야 한다.  
→ 단순한 기술 문제가 아니라, **사회적 책임**의 문제다.

> we don't want AI to make the world a less equitable place. we want AI to be a force for good.

<div style="margin-top: 3.3em;"></div>

그럼 공정한 AI는 어떻게 만들 수 있을까?
- **훈련 데이터**에서부터 시작하고,   
- **판단 로직을 담은 알고리즘**,  
- **평가 기준이 되는 지표**까지  

→ **전 과정에서 잠재적인 편향을 인식하고 조치**해야 한다.  

공정한 AI를 만들려면 **개발의 모든 단계에서**, **잠재적인 편향과 위험 요소를 의식**하고 주의 깊게 살펴야 한다.  
> it's about being mindful of potential biases, being aware of those potential pitfalls at evey stage of the process from start to finish.

<br>

---
### 💭 오늘 챙겨간 것들
이번 게시물에서는  
✔️ 기술적인 도구를 넘어서, **에이전트와의 ‘계약’**이라는 새로운 사고방식   
✔️ 그리고 **신뢰할 수 있는 AI 에이전트를 만들기 위해 꼭 필요한 4가지 요건**  

  📌 **Transparency** — 에이전트의 판단 과정을 ‘속속들이’ 들여다볼 수 있어야 한다  
  📌 **Accountability** — 결과에 대해 누군가는 책임져야 한다  
  📌 **Robustness** — 현실의 예측 불가함을 버텨낼 수 있어야 한다  
  📌 **Fairness** — 편향 없이, 모두에게 공정한 AI를 만들어야 한다  

여기까지가 Unit 3였고, Unit 4에서는 **도메인 특화 LLM (Domain-Specific LLMs)**에 대해 다룰 예정!  
바이바이


<br><br>