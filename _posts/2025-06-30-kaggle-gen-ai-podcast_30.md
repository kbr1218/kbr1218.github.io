---
layout: post
title:  "[Kaggle Gen AI] Day 4 - 도메인 특화 LLM: 사이버 보안에 뛰어들기 🚀"
author: me
categories: [ Kaggle Gen-AI]
date: 2025-06-30 09:00:00
image: assets/images/20250402/day4_1.png
---
[지난 글](https://kbr1218.github.io/kaggle-gen-ai-podcast_29/)까지는 AI 계약에 대해 다루었다.  
Day 4에서는 조금 더 "실전"에 가까운 이야기로 넘어가서, LLM이 **의료(MedLM)나 사이버보안(SecLM)처럼 고도로 특화된 분야**에서는 어떻게 쓰이고 있는지를 살펴보려고 한다.
글이 길어지기 때문에 이번 글에서는 **SecLM, 사이버보안 특화 모델**에 대해서만 작성해 보겠다.  

> what's really got me excited is seeing them move beyond those General application and actually tackling like really specialized fields like cyber security and healthcare.

이번 세션에서는,  
✔️ 범용 모델을 넘어 특정 영역에 딱 맞게 모델을 튜닝하는 이유,  
✔️ 어떤 도전과제가 있었고 어떤 돌파구가 열렸는지,  
✔️ 그리고 이게 실제로 어떤 **새로운 가능성들을 만들어내고 있는지**를  
함께 살펴본다.

> "it's not just some cool trend — it's a real strategy"

<div style="margin-top: 2.3em;"></div>

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=MWqspvVvNzA&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=7)

<div style="margin-top: 3.3em;"></div>

---
### 🛡️ SecLM: 사이버보안을 위한 AI 어시스턴트
영화 속 해커들은 항상 어두운 방에 앉아서 혼자 키보드를 두드리지만, 실제 보안 전문가의 일상은 다르다.  
끝도 없이 쏟아지는 새로운 위협에 맞서면서, 바위를 밀어 올리는 것처럼 단조로운 반복 작업에 시간을 쏟아야 한다.  

여기서는 **사이버보안이 직면한 세 가지 주요 압박**을 강조한다:  
1. 새롭고 정교해지는 공격 기법 <small style="color:gray">(constant emergence of sophisticated attacks)</small>  
2. 운영상의 반복 업무 <small style="color:gray">(operational toil and repetitive grunt work)</small>  
3. 숙련된 인력의 부족 <small style="color:gray">(persistent shortage of skilled professionals)</small>  

이런 환경에서 LLM은 단순히 "멋진 기술"이 아니라 보안 전문가들의 부담을 덜어주고 **전략적 업무에 집중하게 만드는 조력자**로 떠오르고 있다.  

> it's not about replacing these Security Experts but giving them almost like an AI assistant that can take care of a lot of that tedious work freeing them up to focus on the really complex strategic stuff.

<div style="margin-top: 3.3em;"></div>

SecLM을 활용하면 가능한 일들의 예시도 몇 가지 제시했는데:
- **자연어 요청을 복잡한 쿼리로 자동 변환**  
  <small style="color:gray">translate natural language into complex query syntax</small>    
  : 예전 같으면 보안 분석가가 몇 시간에 걸쳐 작성하던 쿼리를 몇 초 만에 작성할 수 있다. 

- **경보 확인/분석과 분류 자동화**  
  <small style="color:gray">automate the investigation and categorization of alerts</small>  
  : 중요도에 따라서 대응 우선순위를 지정할 수 있다.  

- **맞춤형 대응 플랜 생성**  
  <small style="color:gray">generate personalized remediation plans</small>  
  : 특정 보안 사고에 맞춘 액션 플랜을 추천할 수 있다.  

- **악성코드 리버스 엔지니어링**  
  <small style="color:gray">reverse engineer obfuscated malicious software</small>  
  : 난독화된 코드의 해석을 도울 수 있따.

- **조직 맞춤형 위협 요약 보고서 생성**  
  <small style="color:gray">generate organization-specific threat summaries</small>  

- **보안 취약점 진단과 안전한 코드 스니펫 제공**  
  <small style="color:gray">identify critical areas for security testing and generate secure code snippets</small>  

<div style="margin-top: 3.3em;"></div>

이 모든 걸 하나의 솔루션이 해결하는 것은 아니기 때문에 여기서는 **계층적 접근 방식**<small style="color:gray">(layered approach)</small>을 제시한다.  
- 최상단: **기존 보안 툴**이 데이터와 컨텍스트를 공급
- 중간층: **특화 모델 API (여기서 SecLM 사용)**
- 하단: **보안 인텔리전스와 인간 전문가의 판단**

<br>

---
### 🔍 SecLM의 목표: 단일 창구에서 모든 보안 데이터 통합
SecLM은 **조각나 있는 다양한 보안 데이터를 한데 모으고**, 누구나 자연어로 질문해도 바로 통합된 답변을 제공할 수 있는 **One-Stop Shop**을 목표로 한다.  

>  you could ask questions in plain language point it to your internal data sources and it gives you an answer that pulls from all that information.

이러한 목표를 위해서는, 생각보다 높은 기준을 충족해야 한다:
- **최신성** <small style="color:gray">(timeliness)</small>    
  : 위협 환경은 순식간에 바뀌고, 모델이 뒤쳐지면 쓸모 없어진다.

- **보안과 개인정보 보호** <small style="color:gray">(analyze sensitive user specific data)</small>  
  : 민감한 내부 데이터를 안전하게 분석하면서도 외부에 노출되면 안 된다.

- **깊이 있는 보안 지식** <small style="color:gray">(deep security knowledge)</small>  
  : 전문 용어와 컨셉을 제대로 이해하고, 그 “언어”로 대화할 수 있어야 한다.

- **다단계 추론** <small style="color:gray">(capable of multi-step reasoning)</small>  
  : 서로 다른 데이터 소스와 모델을 연결해 복잡한 질문에도 답할 수 있어야 한다.

<div style="margin-top: 3.3em;"></div>

하지만 위 기준들을 만족시키는 데에는 큰 장벽이 있다.  

먼저, **일반 LLM이 보안 분야에 적합하지 않는 이유** 중 하나는 **고품질 학습 데이터가 거의 공개되어 있지 않다는 점**이다.  
실제 보안 사고의 세부 정보들은 대부분 기밀이라, 모델 훈련이 어렵다.  

그리고 운좋게 구할 수 있는 데이터도 대부분 특정 제품이나 표준 개념에 치우쳐 있어서, **현실적인 맥락과 세부 디테일이 부족**하다.  

마지막으로, 보안 도메인은 **너무 방대하고 복잡**하다는 점이다.  
- 낮은 수준의 시스템 아키텍처부터
- 높은 수준의 정책과 위협 인텔리전스까지  
모델이 모두 "유창하게 <small style="color:gray">(be fluent)</small>" 다룰 수 있어야 한다. 

> it's a lot for a model to grasp.

특히 악성코드 분석이나 피싱 탐지같은 **민감 영역**은 오용 가능성 때문에 범용 모델이 기피하는 경우도 많다.  

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
💬 이 모든 점이, <strong>왜 사이버보안을 위한 특화 LLM이 따로 필요</strong>한지를 잘 보여준다!
</div>
<br>

---
### 🏭SecLM이 만들어지는 과정
마지막으로, 이렇게 고도로 특화된 **SecLM**은 어떻게 개발될까?  

먼저 **강력한 범용 기반 모델**로부터 시작한다.  
위협 인텔리전스는 전 세계에서 쏟아지기 때문에 초기 단계부터 다국어 처리가 중요하다.  

이후에는 **사이버보안에 특화된 pre-training**을 거친다.  
- 블로그, 위협 리포트, 탐지 룰, 보안 교재까지 활용하며
- 모델이 **보안의 언어를 자연스럽게 이해**하도록 학습한다.  

그 다음 단계는 **지도 학습 파인튜닝**이다.  
여기서는 실제 보안 전문가들이 현실에서 실행하는 과제를 그대로 따라한다:  
- 악성 그룹 분석 <small style="color:gray">(analyzing potentially malicious groups)</small>  
- 명령어 해석 <small style="color:gray">(explaining command line instructions)</small>  
- 이벤트 로그 분석 <small style="color:gray">(interpreting security event logs)</small>  
- 복잡한 위협 리포트 요약 <small style="color:gray">(summarizing complex threat reports)</small>  
- 보안 관리 플랫폼용 쿼리 생성 <small style="color:gray">(generating queries for Security Management platforms)</small>  

> so they're really putting it through the paces.

이 모든 과정에서도 **프라이버시는 철저히 보호**된다.  
사용자 데이터는 특정 파인튜닝에서만 제한적으로 사용되고, 그 외에는 별도로 분리된다.  

<br>

---
### ⚖️ 성능 평가 방식
SecLM의 성능 평가는 여러 관점에서 이루어진다:
- **명확한 답이 있는 과제** <small style="color:gray">(clear right or wrong answer)</small>  
  :  right or wrong이 분명한 악성 코드 분류같은 과제는 표준 분류 메트릭스로 평가한다.

- **열린 질문(?)** <small style="color:gray">(open-ended tasks)</small>  
  : 전문가의 답변과 생성 결과를 Rouge, BERTScore 등으로 비교한다. 

- **더더 큰 LLM을 이용한 자동 비교 평가**

- 마지막에는 반드시 **인간 전문가의 정성 평가**가 필요하다. 

<br>

---
### ⚙️ 적응력과 최신성: In-Context Learning, PET, RAG
모델이 아무리 잘 학습해도 새로운 환경에 적응하는 능력 또한 중요하다.  
- **In-Context Learning**
  : 새로운 시스템 예제를 몇 개만 보여주는 즉시 적용하고
- **PET (Parameter-Efficient Tuning)**
  : 전체 모델의 재훈련 없이 가벼운 맞춤화가 가능하고
- **RAG (Retrieval-Augmented Generation)**
  : 최신 위협 정보를 실시간으로 검색해 반영할 수 있어야 한다. 

이러한 기법들 덕분에, SecLM은 언제나 최신 정보와 맥락을 유지할 수 있다. 

> it's always got the most current information

<br>

---
### 🌊 동적 워크플로우의 예시
팟캐스트에서는 **AP41 그룹**을 예로 들었는데,  

보안 분석가가 "AP41의 일반적인 공격 방법이 뭐야?"라고 물으면,  
SecLM은 단순히 한 번 답하는 게 아니라:  
1️⃣ 관련 정보를 검색  
2️⃣ 주요 TTPs와 IOCs를 추출  
3️⃣ 사용자 시스템용 쿼리를 생성  
4️⃣ 보안 이벤트를 조회  
5️⃣ 최종 요약을 작성 

요런 **유연한 추론/계획 프레임워크**가 SecLM의 핵심이다. 

> so it's kind of like a dynamic workflow.  

이 자동화 덕분에 분석가는 몇 시간의 반복 작업을 절약할 수 있으며,  
위협 인텔리전스 통합, 외부 보안 도구 연동, 맞춤형 분석까지 활용 범위는 훨씬 넓어진다.  

그리고, **장기 기억** <small style="color:gray">(long-term memory)</small>을 통해 사용자 선호화 맥락까지 함께 학습할 수 있다. 

<br>

---
### 💭 오늘 챙겨간 것들
SecLM은 그냥 "보안을 좀 더 편리하게" 만드는 수준이 아니라, **보안 전문가들의 일상적 부담을 줄이고, 새로운 수준의 자동화와 인사이트를 제공하는 플랫폼**이 되는 것이 목표이다  

> the goal is for it to become this central platform that can truly transform how cyber security is practiced.

이어지는 글에서는 **의료 도메인 특화 모델인 MedLM**에 대해서 알아보기로~

<br><br>