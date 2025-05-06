---
layout: post
title:  "[Kaggle Gen AI] Day 3 - 에이전트가 세상과 연결되는 방법: Tools 🚀"
author: me
categories: [ Kaggle Gen-AI]
date: 2025-04-28 09:00:00
image: assets/images/20250402/day3_1.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_23/)에서는 에이전트가 **목표를 세우고 계획하고 실행하는 시스템**이라는 점을 중심으로 그 작동 원리인 **Cognitive Architecture**를 살펴봤다.  

셰프의 비유처럼,  **생각하고 → 행동하고 → 관찰하고 → 조정하는** 사고 루프를 반복하며 ReAct, CoT, ToT 같은 프레임워크를 통해 어떻게 추론하고 결정하는지를 정리했는데~

이번 글에서는 그 흐름을 이어서 에이전트가 **실제 세상과 상호작용하는 방법**, **외부 정보를 얻고, 작업을 실행하고, 지식을 확장하는 수단인 '도구(Tools)'**에 대해 정리해보겠다. 

✔️ Google 모델 생태계 기준으로 분류한  
- **Extensions**,  
- **Functions**,  
- **Data Stores**  

요 세 가지를 중심으로 에이전트 도구 시스템을 들여다본다.야호

<div style="margin-top: 3.3em;"></div>

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=D3Kaqz7VW28&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=5)

<div style="margin-top: 2.3em;"></div>

---
### 🧰 에이전트가 세상과 연결되는 열쇠 - Tools
팟캐스트에서 반복에서 계속 말하던 것이, 에이전트는 내부적으로 아무리 정교하게 추론하고 계획하더라도 **그 자체만으로는 외부 세계에 직접적인 영향을 미치지 못한다**는 것이다.  

언어 모델은  
- 정보를 처리하고 텍스트를 생성하는 데는 뛰어나지만,
- 로봇 팔을 움직이거나, 웹사이트에서 무언가를 구매하는 등 실제 작업을 수행하지는 못한다. 

> language models are great at processing information and generating text but they can't actually do things in the real world on their own like they can't control robot arm or buy something online 

그래서 필요한 것이 바로 **도구(tools)**이다.  

도구는 **모델과 외부 세계 사이의 다리 역할**을 하며, 에이전트가 외부 시스템, 데이터, API와 실제로 **상호작용**할 수 있게 만들어준다. 
> tools are keys to the outside world.

Kaggle gen-ai intensive course는 Google과 함께하는 콘텐츠인 만큼, 이 팟캐스트에서는 특히 **Google 모델들을 기준**으로, 에이전트가 활용하는 도구들을 세 가지 범주로 나누어 설명한다:
- **🧩 Extensions**
- **🧪 Functions**
- **📚 Data Stores**

각 도구는 **작동 방식, 실행 위치, 쓰임새**가 서로 다르며, 에이전트가 얼마나 유연하고 똑똒하게 행동할 수 있는지를 좌우하는 핵심 요소들이다.  

<br>

---
### 🧩 Extension — API와 연결하는 표준 인터페이스
Extension은 말 그대로, 에이전트가 **외부 API와 연결될 수 있도록 도와주는 표준화된 방식**이다. 
> extensions are essentially a standardized way to connect an agent to an API.

일반적으로 API를 사용하려면  
- 사용자 요청을 해석하고 <small style="color:gray">(interpret the user's requests)</small>   
- 알맞은 API 호출을 구성하고 <small style="color:gray">(figure out the right API calls to make)</small>  
- 에러 처리를 따로 구현하는 등 <small style="color:gray">(handle all the errors that could pop up)</small>  

**복잡한 맞춤형 코드**가 필요하다.  

하지만 Extension을 사용하면 이 모든 과정을 **에이전트가 손쉽게 처리할 수 있도록 추상화**할 수 있다.  

<div style="flex: 3; min-width: 120px;">
    <img src="/assets/images/20250428/aiagent1.png" 
        alt="agent interact with external apis" 
        style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
    <small style="color:gray; display: block; margin-top: 8px;">
      How do Agents interact with External APIs?
    </small>
</div>
<div style="margin-top: 3.3em;"></div>

예를 들어, 여행 일정을 도와주는 에이전트를 만든다고 가정해보자.  
Google Flights API를 연결해야 한다면, Extension이 없을 경우 **직접 복잡한 코드를 짜야 한다**.  

하지만 Extension이 있다면, 에이전트는 단지 **몇 가지 사용 예시만 학습하면** 알아서 상황에 맞게 적절한 API 호출을 선택할 수 있게 된다. 
> so the agent figures out how to use the API just by seeing some examples

<div style="flex: 3; min-width: 120px;">
    <img src="/assets/images/20250428/aiagent2.png" 
        alt="agent interact with external apis" 
        style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
    <small style="color:gray; display: block; margin-top: 8px;">
      Extensions connect Agents to External APIs
    </small>
</div>
<div style="margin-top: 3.3em;"></div>

Extension은 **개별적으로 개발되지만**, 에이전트 구성에 포함되면 **실행 중에 상황에 맞게 선택적으로 활용**할 수 있다.  
> so at runtime, the agent can actually look at the user's query and choose the most appropriate extension based on those built-in examples.

<div style="flex: 3; min-width: 120px;">
    <img src="/assets/images/20250428/aiagent3.png" 
        alt="agent interact with external apis" 
        style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
    <small style="color:gray; display: block; margin-top: 8px;">
      1-to-many relationship between Agents, Extensions and APIs
    </small>
</div>
<div style="margin-top: 3.3em;"></div>

게다가 Google은 미리 만들어진 Extension들도 제공하고 있다. 예를 들어 **Code Interpreter**와 같은 도구는 모델이 파이썬 코드를 생성하고 **실제로 실행**할 수 있게 도와준다.  

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 예를 들어, “리스트를 정렬하는 함수를 만들어줘”라는 요청을 하면, 에이전트는 코드를 짜고 실행해 결과를 바로 보여줄 수 있다.
</div>

즉, Extension은 **API 사용을 위한 에이전트의 표준 인터페이스**로, 복잡한 내부 구현을 몰라도 다양한 외부 서비스와 연결할 수 있게 해준다.  
⇒ 복잡한 로직은 Extension이 처리하고, **에이전트는 그걸 마치 내장 기능처럼 "이용"할 수 있는 구조**이다.  

<br>

---
### 🧪 Function — 클라이언트 쪽에서 실행되는 맞춤형 기능 블록
Function은 우리가 일반적으로 프로그래밍에서 말하는 함수와 비슷하다.  
하나의 목적을 가진 **작고 독립적인 코드 조각**이며, 특정 작업을 수행하도록 설계되어 있다.  

하지만 에이전트 맥락에서 Function은 조금 다르다:  
에이전트가 **"언제 어떤 함수를 쓸지", "무슨 데이터를 넘길지"를 판단**하고, **실행은 클라이언트 쪽 시스템이 담당**하는 구조이다.  

<div style="margin-top: 3.3em;"></div>

##### 📌 Extension과 다른 두 가지 포인트

<div style="flex: 3; min-width: 120px;">
    <img src="/assets/images/20250428/aiagent4.png" 
        alt="agent interact with external apis" 
        style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
    <small style="color:gray; display: block; margin-top: 8px;">
      Delineating client vs. agent side control for extensions and function calling
    </small>
</div>
<div style="margin-top: 2.3em;"></div>

1. **API 호출을 직접 하지 않는다.**  
    ⇒ 언어 모델은 단지 **어떤 함수 + 어떤 인자(arguments)**를 쓸지 결정할 뿐이고, 실제 호출은 에이전트의 외부에서 이루어진다. 

    <div style="margin-top: 3.3em;"></div>

2. **함수는 클라이언트 측에서 실행된다.**
    ⇒ Extension이 에이전트 내부에서 실행되는 반면, Function은 사용자의 앱이나 서비스같은 **외부 시스템에서 실행**된다.  

> so it's like the agent is delegating that task to the client-side app

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  💡 <strong>핵심 차이 한 줄 요약:</strong><br>
  Extension은 <strong>Agent가 실행</strong><br>하고, Function은 <strong>Agent가 '지시'만 하고, 실행은 클라이언트</strong>가 한다. 
</div>

<br>

지난번과 같이 비행기 예약을 도와주는 에이전트를 만들 때, **Function 기반**이라면,  
- 에이전트는 **도시 이름만 추천**해주고, <small style="color:gray">(e.g. { "function": "display_cities", "args": ... } 이런 함수를 써~라는 JSON 응답을 만들어서 넘겨준다)</small>
- 실제 Google Flights API 호출은 사용자의 앱이 담당한다.  

이렇게 하면 개발자는
- 인증 문제를 직접 처리하거나
- API 응답을 가공하거나
- 아직 API가 준비되지 않은 상황에서도 테스트할 수 있어서

더 많은 **제어권과 유연성**을 갖게 된다.  
> it's a really clever way to split up the work and give the developer more flexibility.

백서에서는 travel concierge 예시를 통해 이를 잘 설명하고 있다.  
에이전트가 추천한 도시 이름을 클라이언트 앱이 받아서, Google Places API를 통해 이미지나 정보를 가져오는 방식이다.  

<div style="margin-top: 1.5em; margin-bottom: 1.2em;">
  <img src="/assets/images/20250428/aiagent5.png" 
       alt="Function Call - Travel Concierge Example" 
       style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
  <small style="color:gray; display: block; margin-top: 8px;">
    Sequence diagram showing the lifecycle of a Function Call
  </small>
</div>

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 에이전트는 그저 '빈칸만 채우는 역할'만 하고, 실제 실행은 개발자가 원하는 방식으로 처리한다. 
</div>

추가적으로, Functions는
- 민감한 인증 정보를 노출하지 않아도 되고,
- 비동기 작업이나 사용자 맞춤 실행해도 적합해서  

**보안과 사용자 경험 측면에서도 유리한 옵션**이 될 수 있다.

<br>

---
### 📚 Data Stores - 최신 정보를 가져오는 창구
에이전트가 아무리 똑똑하더라도, 기본적으로는 **훈련 데이터가 반영된 시점까지만** 세상을 알고 있다. 

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 팟캐스트에서는 이를 "언어모델 = 거대한 도서관"에 비유한다. <br>
    단, 그 도서관은 <strong>책이 업데이트되지 않는다는 치명적인 한계</strong>가 있다!
</div>

그 한계를 해결하는 것이 바로 **Data Store**다!  

<div style="margin-top: 1.5em; margin-bottom: 1.2em;">
  <img src="/assets/images/20250428/aiagent6.png" 
       alt="data stores" 
       style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
  <small style="color:gray; display: block; margin-top: 8px;">
    Data Stores connect Agents to new real-time data sources of various types.
  </small>
</div>

Data Store는 에이전트가
- 훈련 당시 데이터에 국한되지 않고,
- **현재 시점의 외부 정보**를
- **필요할 때마다 직접 검색하고 활용**할 수 있게 만들어준다.  

> so instead of being limited to its training data, the agent can access database, APIs, or any external source to get up-to-date and relevant information.

예를 들어, 
- 최신 뉴스, 환율 정보처럼 시시각각 바뀌는 데이터
- 회사 내부 문서, 논문, 슬랙 로그처럼 특정 조직에만 있는 **비공개 데이터**

이런 정보들을 Data Store에 저장해두면, **모델을 다시 훈련하지 않아도**, 에이전트가 직접 필요할 때 꺼내 쓸 수 있다. 

<div style="margin-top: 3.3em;"></div>

##### 어떻게 동작할까?
Data Store는 일반적으로 **벡터 데이터베이스 (VectorDB)**로 구성된다.  
    ⇒ 텍스트의 "의미"를 기준으로 검색할 수 있기 때문!

특히 이 팟캐스트에서 강조한 강력한 활용법이 바로 **RAG (Retrieval-Augmented Generation)**이다.  

<div style="margin-top: 3.3em;"></div>

##### RAG의 작동 흐름

<div style="margin-top: 1.5em; margin-bottom: 1.2em;">
  <img src="/assets/images/20250428/aiagent7.png" 
       alt="RAG" 
       style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
  <small style="color:gray; display: block; margin-top: 8px;">
    The lifecycle of a user request and agent response in a RAG based application
  </small>
</div>

1. 사용자 쿼리 입력 <small style="color:gray">(starts with the users query)</small>   
2. 모델이 이를 **벡터 임베딩**으로 변환 <small style="color:gray">(converted into a vector embedding)</small>  
3. Data Store에서 가장 **관련도 높은 문서**들을 검색 <small style="color:gray">(search through the data store to find the most relevant documents)</small>  
4. 해당 문서들과 쿼리를 함께 모델에 다시 전달 <small style="color:gray">(sent back to the agent along with the original query)</small>  
5. 모델이 그 정보를 바탕으로 **최종 응답 생성** <small style="color:gray">(uses all of that information to generate response)</small>  

> so it's combining its internal knowledge with all this external information to give the best possible answer

그리고 하나의 에이전트가 여러 개의 Data Store에 동시에 연결되어 **다양한 출처의 지식을 종합적으로 활용하는 구조**도 가능하다. 

<div style="margin-top: 3.3em;"></div>
<div style="margin-top: 1.5em; margin-bottom: 1.2em;">
  <img src="/assets/images/20250428/aiagent9.png" 
       alt="various types of pre-indexed data" 
       style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
  <small style="color:gray; display: block; margin-top: 8px;">
    1-to-many relationship between agents and data stores, which can represent various types of pre-indexed data
  </small>
</div>

요약하자면:  
- Data Store는 에이전트가 **실시간 정보에 접근할 수 있게 해주는 창구**이다.  
- 벡터 DB를 활용해 **의미 기반 검색**이 가능하다.  
- RAG 처럼 구조화된 워크플로우를 통해, 에이전트는 **기억 + 검색 + 생성**을 결합한 강력한 응답을 생성할 수 있다. 

<br>

---
### 📦 어떤 도구를 언제 쓰면 좋을까? – Tools 요약 정리
지금까지 살펴본 세 가지 도구 - **Extension, Function, Data Store**는 각각의 목적과 사용 맥락이 다르다.  

백서에서는 이 세가지를 비교한 걸 표로 제공하고 있으니 참고:

<div style="margin-top: 1.5em; margin-bottom: 1.2em;">
  <img src="/assets/images/20250428/aiagent10.png" 
       alt="Sample RAG based application w ReAct reasoning/planning" 
       style="max-width: 100%; display: block; margin-left: auto; border-radius: 6px;" />
</div>

- **🧩Extension**은
    - API 호출을 에이전트가 직접 처리해야 할 때
    - 특히 사전 구축된 API Extension이 있다면 빠르고 편리하게 사용이 가능하다  

- **🧪Function**은
    - 인증 문제나 세밀한 제어가 필요한 상황,
    - 또는 아직 API가 준비되지 않은 상태에서 에뮬레이션 목적으로도 유용하다  

- **📚 Data Store**
    - 에이전트가 최신 정보, 혹은 대규모 외부 지식을 활용해야 할 때 필수이며
    - 특히 RAG 기반 에이전트 구성의 핵심 요소이다  

이렇게 도구의 특성과 쓰임새를 명확히 구분하면, 서비스나 사용 목적에 따라 **에이전트 설계에 필요한 조합을 결정**하는 데 큰 도움이 된다.  

마지막 예시로, RAG 구조와 ReAct 추론 방식을 함께 사용하는 에이전트의 흐름 예시는 아래 그림과 같다:

<div style="margin-top: 1.5em; margin-bottom: 1.2em;">
  <img src="/assets/images/20250428/aiagent8.png" 
       alt="Sample RAG based application w ReAct reasoning/planning" 
       style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
  <small style="color:gray; display: block; margin-top: 8px;">
    Sample RAG based application w/ ReAct reasoning/planning
  </small>
</div><br>


---
### 💭 오늘 챙겨간 것들
에이전트가 외부 세계와 연결되려면 도구(Tools)가 꼭 필요하다.  
⇒ 모델 혼자서는 아무것도 못 한다. 실제 행동은 도구를 통해서만 가능하다는 것!  

🧩 **Extension** → 에이전트가 직접 API를 호출할 수 있게 해주는 표준화된 연결 방식  
🧪 **Function** → 에이전트는 함수를 ‘지시’만 하고, 실행은 클라이언트가 담당  
📚 **Data Store** → 훈련 데이터 외에도, 최신 정보나 비공개 데이터에 접근 가능하게 해주는 창구 (RAG 구조에서 필수이다)

<div style="margin-top: 3.3em;"></div>

다음 글에서는, 이렇게 도구를 잘 쓰기 위해 모델에게 필요한 **학습 전략** <small style="color:gray">(in-context learning, RAG 기반 학습, 파인튜닝, etc.)</small>과 직접 LangChain으로 에이전트를 만들어보는 예시 코드도 함께 정리하면서 Unit 3- AI Agent를 마무리해보겠다. 바이바이

<br><br>