---
layout: post
title:  "[Kaggle Gen AI] Day 3 - 실전 에이전트 구축과 운영: AgentOps, KPI, 평가 자동화 🚀"
author: me
categories: [ Kaggle Gen-AI]
date: 2025-05-01 09:00:00
image: assets/images/20250402/day3_2.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_25/)에서는 에이전트가 외부 도구와 상호작용하며 실제 세계에서 작업을 수행하는 방식과, 그 도구 사용 능력을 향상시키기 위한 몇 가지 **학습 전략들** <small style="color:gray">(in-context, retrieval, fine-tuning)</small>을 정리해봤다.  

LangGraph와 ReAct 프레임워크를 사용한 실전 예제를 통해, 멀티스텝 질문을 처리하는 간단한 에이전트도 만들어봄!  

Unit 3의 두 번째 심화 팟캐스트에서는, **에이전트를 실전 환경에서 운영하는 데 필요한 요소들**에 대해 정리해보려고 한다. 심화 유닛인만큼 기본적인 agent 구조를 넘어서 실제 배포, 운영, 평가까지 다루는 개발자 중심의 실전 가이드라고 보면 된다.

실제로 운영 가능한 agent를 만들기 위해선
- **운영을 위한 시스템 구성** <small style="color:gray">(AgentOps)</small>  
- **비즈니스 KPI와의 연결**
- **사람 + 자동화된 평가 전략**  

등 현실적인 고민들이 필수적으로 따라온다.


<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  ⚠️ <strong>주의사항</strong><br>
  이번 팟캐스트에는 중간에 내용이 반복되거나 말이 끊기는 부분이 있다. <small style="color:gray">(AI로 만든 팟캐스트라더니 검토하는 걸 잊으셨나 봄)</small>
</div>
<div style="margin-top: 2.3em;"></div>

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=7rbSwt-7odQ&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=5)

<div style="margin-top: 2.3em;"></div>

---
### 🌎 현실에서 돌아가는 에이전트를 만든다는 것
이 팟캐스트와 백서에서는 Proof of Concept (PoC) 수준의 데모와 **실제 환경에서 안정적으로 동작하는 에이전트**를 만드는 건 완전히 다른 이야기라는 걸 알려준다.

그냥 "우리 에이전트는 이런 것도 할 수 있어요" 수준을 넘어서, "이걸 실제로 **규모 있게** 운영하려면 어떻게 해야 하지?"라는 질문으로 넘어가는 시점에 주목한다.

> it's not just about building a cool demo anymore. It's like going from, hey, look what my agent can do. now how do I actually make this work, at scale without, needing a whole team of engineers to babysit it? 24/7.

바로 여기서 **AgentOps**라는 개념이 등장한다.  

<br>

---
### 🏭 AgentOps: 에이전트를 위한 DevOps + MLOps
백서에서는 이를 **DevOps와 MLOps의 하이브리드**로 설명하는데, 에이전트 운영에만 나타나는 고유한 과제들을 다루기 위한 방식이라고 보면 된다. 

예를 들어:  
- 에이전트가 사용할 **도구들을 안정적으로 관리하는 프로세스** <small style="color:gray">(managing the tools that your agents are going to use)</small>
- 매우 복잡해질 수 있는 **워크플로우를 오케스트레이션**하는 방식 <small style="color:gray">(ways to orchestrate these like sometimes very complex workflows)</small>
- **메모리 관리**를 효율적으로 하는 방법 <small style="color:gray">(figuring out how to handle memory efficiently)</small>
- 하나의 거대한 작업을 **더 작고 처리 가능한 단위로 나누는 전략** <small style="color:gray">(breaking down these massive tasks into smaller, more manageable chunks)</small> 

이런 것들을 잘 설계하고 구성해야 한다.  

> it's like treating your agents as like a well-oiled machine where each part has its role. it's functioning smoothly and then there's a system in place to kind of monitor and maintain the whole operation percisely.

즉, 각각의 부품이 제 역할을 하고 전체가 매끄럽게 돌아갈 수 있도록 **정밀한 운영체계**를 갖추는 것. 이게 AgentOps가 지향하는 방향이다.  

<br>

---
### 📈 성능 측정은 결국 KPI와 연결된다 
그렇다면 **에이전트가 실제로 잘 작동하고 있는지**는 어떻게 판단할 수 있을까?  
여기서는 **비즈니스 KPI**를 중심에 둬야 한다고 답한다.  

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  💡 <strong>KPI(Key Performance Indicator)란?</strong><br>
  <strong>비즈니스 성과를 수치로 측정하는 지표</strong>를 의미한다. 에이전트의 경우, 목표 달성률, 사용자 참여도, 수익 기여도 등이 대표적인 KPI가 될 수 있다. <small style="color:gray"><em>(your business KPIs, those northstar metrics, goal, completion, user engagement, revenue, those should be front and center)</em></small>
</div>

에이전트는 결국 **특정한 비즈니스 목표를 달성하기 위해 만들어진 도구**이다.   
그렇다면 그 성능을 측정할 때도, **그 목표를 얼마나 잘 달성했는가**를 기준으로 판단하는 게 당연하다. 

<div style="margin-top: 2.3em;"></div>

이를 위해서는 **KPI와 연결될 수 있는 세부 지표들** <small style="color:gray">(granular metrics)</small>을 수집해야 한다:  
- 태스크 성공률 <small style="color:gray">(task success rate)</small>
- 사용자가 **에이전트와 어떻게 상호작용**했는지 <small style="color:gray">(how users are interacting with the agent)</small>
- 그리고 에이전트가 **어떤 경로로 작업을 수행했는지**에 대한 상세 로그 <small style="color:gray">(a detailed log of the agents actions)</small>  
    → 디버깅용 breadcrumb처럼

<div style="margin-top: 2.3em;"></div>

결과만 보는 것이 아니라, **그 결과에 도달하기까지의 과정을 파악**하는 것이 중요하다는 점을 강조하고 있다. 
> this level of tracking gives you insights not just into the outcome, but also th epath the agent took to get there. so it's not just about whether they succeeded or failed. it's about understanding how they got to that point. **the journey matters too.**

<br>

---
### 👀 사람의 피드백 vs. 자동화된 평가: 둘 다 필요하다
다음은 **에이전트에 대한 사람의 직접적인 피드백**<small style="color:gray">(human feedback)</small>**의 중요성**에 대한 내용이다.  

- 좋아요/싫어요 버튼 <small style="color:gray">(thumbs up or down systems)</small>
- 사용자 설문 <small style="color:gray">(user surveys)</small>
- 자유롭게 의견을 남길 수 있는 오픈형 피드백 등등 <small style="color:gray">(open-ended feedback)</small>

실제 사용자들이 **에이전트를 어떻게 느끼고 있는지**, **현장에서 어떻게 작동하는지에 대한 생생한 인사이트**를 줄 수 있는 모든 정보가 소중하다고 하며 강조한다. 

숫자로는 보이지 않은, 사용자 경험의 뉘앙스를 이해하려면 사람의 관찰이 반드시 필요하다. 
> the hard data might not tell the whole story.

<div style="margin-top: 3.3em;"></div>

하지만 모든 데이터를 사람이 일일이 들여다보기는 어렵다. 그래서 등장한 것이 **자동화된 평가** <small style="color:gray">(automated evaluation)</small>이다. 
> so it's like automating the quality control process for your agents.

<div style="margin-top: 3.3em;"></div>

자동화된 평가는 다음과 같은 것들을 할 수 있게 해준다:
- 에이전트가 **문제를 해결하는 방식** <small style="color:gray">(trajectory)</small> 분석
- 최종 응답의 품질 평가
- 전체 프로세스를 **수동 테스트 없이 진행**

<div style="margin-top: 3.3em;"></div>

여기서 trajectory란, **에이전트가 문제를 해결하기 위해 선택한 경로**를 의미한다:
- 적절한 도구를 사용했는가? <small style="color:gray">(did it pick the right tools?)</small>
- 효율적인 접근 방식을 사용했는가? <small style="color:gray">(was its approach efficient?)</small>
- 불필요한 경로로 시간을 낭비하진 않았는가? <small style="color:gray">(did it waste time exploring, dead ends?)</small>

> it's not just about the destination, it's also about the journey.

<div style="margin-top: 3.3em;"></div>

이러한 trajectory를 평가하는 방법으로는 여러 가지가 있다:
- **exact match**: 사전에 정의된 이상적인 순서와 정확히 일치
- **in-order match**: 핵심 단계만 순서대로 맞는지 확인
- **any-order match**: 순서와 관계없이 단계만 맞는지 확인

→ 과제 성격에 따라 유연하게 평가 기준을 조정할 수 있다.

<div style="margin-top: 3.3em;"></div>

그리고 최종 응답의 품질을 평가하는 방식 중 흥미로운 개념이 하나 나온다: **autorator**<small style="color:gray">(자동화된 평가 시스템)</small>  
> it's basically using one LLM to judge the output of another LLM

하나의 LLM이 기준을 설정하고, 다른 LLM의 응답이 그 기준에 맞는지를 평가하는 구조이다. 
마치 AI로 구성된 QA(품질 보증) 팀이 24시간 쉬지 않고 작동하는 것과 같다.
> it's like having an AI quality assurance team working around the clock 24/7 quality control.

<div style="margin-top: 3.3em;"></div>

하지만 이렇게 자동화가 잘 되어 있어도, **사람의 평가가 여전히 중요**한 부분도 있다.  
예를 들어:
- 창의성 <small style="color:gray">(creativity)</small>
- 상식 <small style="color:gray">(common sense)</small>
- 미묘한 맥락 이해 <small style="color:gray">(understanding nuance)</small>

이런 것들은 아직까지**사람이 더 잘 판단할 수 있는 영역**이다.  

왜냐하면 autorater가 "기술적으로 맞고, 규칙도 모두 지켰다"라고 판단하더라도, 사람이 보기엔 말투가 부자연스럽거나 맥락에 어울리지 않을 수 있다는 것이다.
> an autorater might say, "Okay, this response is technically correct. it follows all the rules." but it completely misses the mark in terms of like tone or context.

특히 실제 사용자와 상호작용하는 에이전트라면, **현실적인 감각** <small style="color:gray">(human touch)</small>을 유지하는 것이 필수적이다. 
> it's like this vital check to make sure that those automated evaluation methods are staying aligned with what users actually need and expect.

<br>

---
### 💭 오늘 챙겨간 것들
이번 글에서는 에이전트를 실제 서비스에 적용하려면 어떤 준비가 필요한지 살펴봤다.   
- **AgentOps**라는 개념을 통해, 운영 관점에서 고려해야 할 요소들 — 도구 관리, 워크플로우 설계, 메모리 최적화, etc — 을 확인했고,
- 비즈니스 KPI와 연결된 지표들로 에이전트를 평가하는 방법이랑,
- **자동화된 평가 시스템(autorater)**과 사람의 직관적인 피드백이 각각 어떤 역할을 할 수 있는지도 함께 살펴보았다.

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💡 중요한 건 <strong>결과만 보는 것이 아니라</strong>, 그 결과에 이르는 <strong>과정(trajectory)</strong>을 함께 살펴보는 관점이다.
</div>

다음 글에서는 **멀티 에이전트 시스템**<small style="color:gray">(Multi-agent Systems)</small>을 다룰 예정이다.  
✔️ 여러 에이전트를 조합해 문제를 나눠서 풀고  
✔️ 서로의 작업을 검증하고  
✔️ 때론 협력하고, 때론 경쟁하는 구조까지!  

**팀처럼 움직이는 AI 시스템**은 어떻게 설계되고 평가되는가?에 대한 답을 알아보기로~

<br><br>