---
layout: post
title:  "[Kaggle Gen AI] Day 3 - 에이전트를 더 똑똑하게 만드는 법: 학습 전략과 실전 예제 🚀"
author: me
categories: [ Kaggle Gen-AI]
date: 2025-04-29 09:00:00
image: assets/images/20250402/day3_1.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_24/)에서는 에이전트가 **도구(tools)**를 사용해 실제 세계와 어떻게 연결되는지를 살펴봤다.  

**🧩 Extension, 🧪 Function, 📚 Data Store** - 이 세 가지 도구 유형을 중심으로 에이전트가 외부 시스템과 상호작용하고, 새로운 정보를 얻고, 작업을 실행하는 방식들을 정리했다.  

이번 글은 Unit 3 - Generative AI Agent 팟캐스트의 마지막 파트를 정리할거고, 마무리답게 에이전트를 **더 똑똑하게 만드는 방법**인  
✔️ 모델이 어떤 도구를 언제 어떻게 써야 할지를 더 잘 판단하게 만드는 학습 전략들과  
✔️ 실제로 이런 에이전트를 LangChain과 Vertex AI를 활용해 구현하는 예제를 함께 정리해보려고 한다.  

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=D3Kaqz7VW28&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=5)

<div style="margin-top: 2.3em;"></div>

---
### 🏫 모델을 더 똑똑하게 만들기 — 세 가지 학습 전략
에이전트 시스템이 점점 복잡해질수록 중요한 과제가 하나 생긴다: 모델이 **"어떤 도구를 언제, 어떻게 써야 할지"제대로 판단하는 능력**

이를 위해 팟캐스트에서는 모델 성능을 높이는 **세 가지 학습 접근 방식**을 소개한다.  
- in context learning
- retrieval based in context learning
- fine-tuning based learning

그리고 다시 등장하는 요리사 비유!  
> just like a chef needs to know more than just the basics of cooking.

요리사가 단순히 칼질만 잘한다고 훌륭한 셰프가 될 수 없는 것처럼, **언어 모델도 도구를 잘 쓰려면 그에 맞는 전문적인 지식과 맥락 학습이 필요**하다.  

<br>

##### 1️⃣ In-context Learning - 요리사에게 레시피를 직접 보여주기
> think of it like this: you give the chef (which is the model) a specific recipe (which is the prompt), you give them the necessary ingredients (which are the relevant tools) and maybe you even show them a few examples of what the finished dish should look like.

가장 직관적인 방법은 **프롬프트 안에 필요한 정보들을 다 넣어주는 방식**이다. 
- 모델에게 적절한 도구를 알려주고,
- 몇 가지 사용 예시도 함께 보여주고,
- 목표가 뭔지도 명확히 전달해주면,

모델은 **프롬프트 안의 정보만으로 추론하고 실행**할 수 있게 된다.  
→ 요리사에게 레시피와 재료를 건네주고 “이대로 만들어 봐”라고 하는 느낌!

이 방식은 **속도가 빠르고 구현이 간편**하다는 장점이 있다.

<br>

##### 2️⃣ Retrieval-based In-context Learning — 커다란 팬트리와 요리책 라이브러리
> this is like giving the chef access to a huge pantry full of ingredients and a whole library of cookbooks.

이번엔 요리사에게 더 많은 재료와 참고자료를 주는 방식이다.  
→ 프롬프트에는 없지만 **외부 데이터베이스에서 관련 정보를 검색해서 모델에게 함께 제공**하는 방식!

- 프롬프트를 임베딩하여 의미 기반 검색을 수행하고
- 관련도 높은 문서나 예시를 찾아
- 그것을 함께 넣어 다시 모델에 전달한다. 

바로 이게 앞서 다뤘던 **RAG(Retrieval-Augmented Generation)** 방식이다.  
Vertex AI의 Example Store 기능도 이 방식과 유사하다.
> so it's like augmenting the model's knowledge with an external database of information and examples.

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 <strong>모델이 이미 알고 있는 것 + 외부에서 가져온 정보 = 더 풍부한 추론 가능!</strong>
</div><br>

<br>

##### 3️⃣ Fine-tuning — 셰프를 요리 학교에 다시 보내기
> this is where you actually send the chef back to culinary school to specialize in a particular type of cuisine.

마지막 방식은 **모델 자체를 다시 훈련시키는 방법**이다. 
- 모델이 특정 도구나 도메인에서 전문성을 갖도록
- 대규모 예제 데이터로 **전용 파인튜닝**을 진행하는 것!

> so you're essentially teaching the model to be an expert in using certain tools or in a particular domain.

→ 예를 들어, “항공 예약 도우미 에이전트”를 만들려면 항공 관련 API, 도메인 용어, 흐름에 특화된 데이터로 모델을 다시 학습시키는 것.

당근 이 방식은 **시간과 자원이 많이 든다는 단점**이 있다.

<br>

##### 🔄 각 방법의 트레이드오프
이 세가지 방식은 각자 장단점이 있따.  
- **In-context Learning**
  - 빠르고 유연하다.
  - 하지만 복잡한 작업에는 한계가 있을 수 있다. <small style="color:gray">(might not be as effective for really complex tasks)</small>  

- **Retrieval 기반 학습**
  - 외부 정보로 확장성을 확보하면서도 모델 구조는 유지할 수 있다.

- **Fine-tuning**
  - 특정 작업에 완전히 최적화된 모델을 만들 수 있다.
  - 하지만 시간이 오래 걸리고 리소스가 많이 든다. <small style="color:gray">(takes more time and resources)</small>  

실제로는 이 방법들을 **적절히 조합**할 수 있으니 서비스 목적에 맞게 **속도 vs. 성능 vs. 유연성**의 균형을 맞추는 게 핵심이다.  
> but the good news is you can often combine these techniques to get the best of all worlds.

<br>

---
### ⚙️ LangChain으로 만드는 실전 에이전트 — Quick Start 예제
**실제로 에이전트를 어떻게 만들 수 있는지**를 알아본다. 
여기서는 오픈소스 프레임워크인 **LangChain과 LangGraph**를 사용해 간단하면서도 동작하는 에이전트를 만드는 과정을 보여준다.  

<div style="margin-top: 3.3em;"></div>

##### 🧪 예제에 사용된 구성 요소들
- 모델: Gemini 2.0 Flash 001
- 도구 1: searching Google
- 도구 2: Google Places API  

→ 이걸 가지고 하나의 멀티스텝 질문에 답하도록 에이전트를 만든다.  

백서에서는 이 질문에 대답을 하는 에이전트를 만들었는데:
> who did the Texas Longhorns play in their last game and what's the address of the stadium?

이건 두 단계를 거쳐야 하는 질문이다. 
1. 어떤 팀과 경기했는지 찾고  
2. 그 경기장 주소를 알아내야 한다. 

하지만 저 질문은 재미 없으니까 저거 말고 다른 질문을 해보기로!
> What city did Taylor Swift most recently perform in during the The Eras Tour, and what is the address of the stadium?

이 질문 또한, 두 단계를 거쳐야 한다.  
1. **어디서 마지막으로 공연했는지 찾고**
2. **그 경기장 주소를 알아야 한다.** 

<div style="margin-top: 3.3em;"></div>

코드는 아래와 같은 순서로 구성한다.  
1. API 키 설정 <small style="color:gray">(set up the API Keys)</small>
2. **도구** 정의 <small style="color:gray">(define the tools)</small>
3. **모델** 초기화 <small style="color:gray">(initialize the model)</small>
4. 그리고 **ReAct 기반 에이전트** 구성 <small style="color:gray">(create a react agent that can answer this multi-part question)</small>

<br>

##### 🔧 코드 수정 및 튜닝 포인트 정리
이번 실습을 진행하면서 에러가 몇 가지 발생했고, 라이브러리가 맞지 않는 문제도 있어서 몇 가지 수정을 했고 아래 그 수정사항들을 간단히 정리해봤다.  

1. **Vertex AI → Gemini API로 변경**  
    원래 예제는 ChatVertexAI를 사용하고 있었지만, Kaggle 환경에서는 GCP 프로젝트 인증이 어려워서 대신 **ChatGoogleGenerativeAI**를 사용하는 방식으로 바꿨다.  

    API 키만 있으면 되니까 Kaggle에서도 바로 쓸 수 있다는 장점!  

    ```python
    # from langchain_google_vertexai import ChatVertexAI
    from langchain_google_genai import ChatGoogleGenerativeAI
    
    # model = ChatVertexAI(model="gemini-2.0-flash-001")
    model = ChatGoogleGenerativeAI(model="gemini-2.0-flash")
    ```

    <div style="margin-top: 3.3em;"></div>

2. **GooglePlacesTool import 경로 변경**  
    최근 LangChain 구조가 바뀌면서, GooglePlacesTool이 `langchain_community`에서 deprecated 되었다고 한다.  
    → 그래서 권장되는 `langchain_google_community` 패키지로 import 경로를 변경했다.

    ```python
    # 참고로 패키지를 설치할 때 [places] 옵션을 붙여야 googlemaps 관련 의존성까지 설치된다.
    !pip install -qU "langchain-google-community[places]"

    # 기존 경로 >> 새로운 경로
    # from langchain_community.tools import GooglePlacesTool
    from langchain_google_community import GooglePlacesTool
    ```

    <div style="margin-top: 3.3em;"></div>

3. **주소 검색이 안 되는 문제 해결: 쿼리 표현 변경**  
      처음에는 국가를 찾고 → 바로 스타디움 이름을 검색하는 흐름으로 질문을 구성했는데, 다음 단계인 Google Places에서 주소를 찾을 수 없다는 에러가 계속 나왔다.  
      → 그래서 질문을 **"국가"가 아니라 "도시"를 먼저 찾도록 수정**하고,  

    모델이 도구에 넘겨주는 쿼리도 "BC Place address"처럼 모호한 표현 대신 **"BC Place Stadium, Vancouver"**처럼 좀 더 명확하게 바꿔줬다.
      ```python
      # 도구를 불러오는 함수 수정
      @tool
      def places(query: str):
          """Use the Google Places API to run a query for a specific venue. Prefer full names and locations."""
          # places = GooglePlacesTool()
          # return places.run(query)
          formatted_query = query.replace("address", "").strip()
          return GooglePlacesTool().run(formatted_query)
    ```
    장소명 + 도시명을 함께 명시해주는 게 훨씬 정확도가 높았다!

<br>

##### 💻 코드로 작성하기
수정 사항을 반영해서 전체 코드를 작성하면 이렇게 된다:  
```python
# (패키지 설치 및 API Key 불러오는 코드 생략...)
from langgraph.prebuilt import create_react_agent
from langchain_core.tools import tool
from langchain_community.utilities import SerpAPIWrapper
# from langchain_community.tools import GooglePlacesTool
from langchain_google_community import GooglePlacesTool
# from langchain_google_vertexai import ChatVertexAI
from langchain_google_genai import ChatGoogleGenerativeAI

@tool
def search(query: str):
    """Use the SerpAPI to run a Google Search."""
    search = SerpAPIWrapper()
    return search.run(query)

@tool
def places(query: str):
    """Use the Google Places API to run a query for a specific venue. Prefer full names and locations."""
    # places = GooglePlacesTool()
    # return places.run(query)
    formatted_query = query.replace("address", "").strip()
    return GooglePlacesTool().run(formatted_query)

model = ChatGoogleGenerativeAI(model="gemini-2.0-flash")
# model = ChatVertexAI(model="gemini-2.0-flash-001")
tools = [search, places]

query = "What city did Taylor Swift most recently perform in during the The Eras Tour, and what is the address of the stadium?"

agent = create_react_agent(model, tools)
input = {"messages": [("human", query)]}

for s in agent.stream(input, stream_mode="values"):
    message = s["messages"][-1]
    if isinstance(message, tuple):
        print(message)
    else:
        message.pretty_print()
```

<div style="margin-top: 3.3em;"></div>

##### 📦 실제 출력 결과
```plaintext
================================ Human Message =================================

What city did Taylor Swift most recently perform in during the The Eras Tour, and what is the address of the stadium?
================================== Ai Message ==================================
Tool Calls:
  search (f8b66315-8661-445a-9a32-fbeb27955a05)
 Call ID: f8b66315-8661-445a-9a32-fbeb27955a05
  Args:
    query: Taylor Swift last performance Eras Tour
================================= Tool Message =================================
Name: search

Taylor Swift sang these words as the final performance of her globe-spanning, blockbuster-selling Eras tour came to a close on Sunday night (Dec. 8) at BC Place Stadium in Vancouver, tucking some fan service into a piano rendition of “Long Live” during her acoustic set.
================================== Ai Message ==================================
Tool Calls:
  places (4ee4d08a-a59c-4740-b964-ebd93f133266)
 Call ID: 4ee4d08a-a59c-4740-b964-ebd93f133266
  Args:
    query: BC Place Stadium Vancouver address
================================= Tool Message =================================
Name: places


================================== Ai Message ==================================
Tool Calls:
  search (b7c78a5a-0b21-4d69-9169-98303bf50db9)
 Call ID: b7c78a5a-0b21-4d69-9169-98303bf50db9
  Args:
    query: BC Place Stadium Vancouver address
================================= Tool Message =================================
Name: search

['BC Place is a multi-purpose stadium in Vancouver, British Columbia, Canada. Located at the north side of False Creek, it is owned and operated by the BC Pavilion Corporation, a Crown corporation of the province.', 'BC Place type: Multi-purpose stadium in Vancouver, British Columbia.', 'BC Place entity_type: local_nav.', 'BC Place kgmid: /m/03b4g3.', 'BC Place place_id: ChIJ1eC-_HxxhlQRuxiCkf1NQPk.', 'BC Place address: 777 Pacific Blvd, Vancouver, BC V6B 4Y8, Canada.', 'BC Place capacity: 54,500.', "BC Place teams: Vancouver Whitecaps FC, BC Lions, Canada men's national soccer team.", 'BC Place opened: June 19, 1983; 41 years ago.', 'BC Place owner: PavCo.', 'BC Place height: 204′.', 'BC Place surface: FieldTurf.', 'BC Place record_attendance: 65,061 (September 2, 2023, Ed Sheeran, +–=÷× Tour).', 'BC Place renovated: 2009 (interior); 2011 (exterior and interior).', 'BC Place architect: - Studio Phillips Barratt; Stantec Architecture (renovation);.', 'BC Place operator: BC Pavilion Corporation (PavCo).', 'BC Place executive_suites: 50.', 'BC Place phone: +1 604-669-2300.', "BC Place Stadium, Western Canada's premier venue for live events.", 'BC Place is a multi-purpose stadium located at the north side of False Creek, in Vancouver, British Columbia, Canada.', 'Get more information for B.C. Place Stadium in Vancouver, British Columbia. See reviews, map, get the address, and find directions.', 'BC Place · 777 Pacific Boulevard · Vancouver, British Columbia, Canada.', 'The area. Address. 777 Pacific Blvd, Vancouver, British Columbia V6B 4Y8 Canada.', '777 Pacific Boulevard. Vancouver, BC V6B 4Y8. Downtown ; (604) 669-2300 ; Visit Website. http://www.bcplacestadium.com ; More Info. Health Score ...', '777 Pacific Blvd, Vancouver, British Columbia, Canada. Open in Waze. +1-604-669-2300 · bcplace.com. Closed now. Monday08:30 - 12:00 13:00 - 17:00.']
================================== Ai Message ==================================

Taylor Swift's most recent performance during The Eras Tour was in Vancouver, BC, Canada at BC Place Stadium. The address of the stadium is 777 Pacific Blvd, Vancouver, BC V6B 4Y8, Canada.
```

실행 결과를 살펴보자면, 에이전트가 질문을 받고 아래 순서대로 도구를 사용했다:
1. 먼저 `search` 툴을 사용해
    → Taylor Swift가 The Eras Tour에서 가장 최근에 공연한 도시가 Vancouver, BC라는 정보를 가져옴.
2. 이어서 `places` 툴을 호출했지만, `query: "BC Place Stadium Vancouver address"`로는 결과가 없어서  
    → 다시 `search` 툴을 한 번 더 사용해 정확한 스타디움 주소를 가져옴.

최종적으로, **공연 장소와 주소** 둘 다 정확하게 출력된 걸 확인할 수 있다!
> Taylor Swift's most recent performance during The Eras Tour was in Vancouver, BC, Canada at BC Place Stadium. The address of the stadium is 777 Pacific Blvd, Vancouver, BC V6B 4Y8, Canada.

<br>

---
### ☁️ 프로덕션 수준 에이전트를 위한 선택지 — Vertex AI Agent
LangChain과 Gemini API만으로 꽤 유용한 에이전트를 만들 수 있었지만, 백서에서는 **실제 프로덕션 환경에서 쓸 수 있는 에이전트 구성 방식**도 소개하고 있다.  

바로 **Vertex AI 기반 에이전트** 시스템!
> because when you're building someting for the real world, you need a lot more than just the basic agent logic.

<div style="margin-top: 3.3em;"></div>

##### 🏗️ 단순한 코드 이상의 것들이 필요하다
현실 세계에서 잘 동작하는 에이전트를 만들기 위해서는 단순히 ReAct 구조로 도구만 잘 부르면 되는 게 아니라, 아래와 같은 것들도 함께 고려해야 한다:
- UI나 챗 인터페이스 등 **사용자와 연결되는 진짜 인터페이스** <small style="color:gray">(user interfaces)</small>  
- **에이전트 성능을 측정하고 평가**하는 방법 <small style="color:gray">(ways to evaluate how well the agent is performing)</small>  
- 시간이 지남에 따라 성능을 개선하기 위한 지속적인 **업데이트 파이프라인** <small style="color:gray">(systems for continuously improving it)</small>  

이런 요소들을 모두 포함하고 있는게 Vertex AI Agent 플랫폼이라며 홍보하고 있다. 나중에 써봐야지
> it's a fully managed platform from Google that has everything you need to build, deploy and manage these sophisticated agent applications.

<div style="margin-top: 3.3em;"></div>

##### 🖥 자연어 기반 설정 인터페이스
그리고 Vertex AI의 장점 중 하나는 **복잡한 코드를 몰라도 자연어로 에이전트를 구성할 수 있는 인터페이스**를 제공한다는 점이다.  
- 어떤 역할을 수행할지
- 어떤 도구를 사용할지
- 어떤 식으로 응답해야 할지 예시까지 제공하면

→ 플랫폼이 이를 기반으로 에이전트를 구성해준다!
> so you don't have to be a coding wizard to use it. 

<div style="margin-top: 3.3em;"></div>

##### 🤖 에이전트 아키텍처 구성 예시
아래 그림에서 Vertex AI 기반 에이전트의 전체 구조를 확인할 수 있다.  

모델, 도구, 유저 인터페이스, 평가 및 개선 시스템까지 **각 구성요소가 어떻게 연결되는지 한눈에 파악**할 수 있게 정리되어 있다.  

<div style="flex: 3; min-width: 120px;">
    <img src="/assets/images/20250428/aiagent11.png" 
        alt="sample of agent architecture" 
        style="max-width: 100%; display: block; margin-left: auto; border: 1px solid #ddd; border-radius: 6px;" />
    <small style="color:gray; display: block; margin-top: 8px;">
      Sample end-to-end agent architecture built on Vertex AI platform
    </small>
</div><br>

---
### 💭 오늘 챙겨간 것들
여기까지 Unit 3의 Agent 정리였다.  

에이전트를 제대로 만들기 위해서는  
✔️ 생각의 구조를 짜는 오케스트레이션 레이어,  
✔️ 외부 세계와 상호작용하는 도구(Extensions, Functions, Data Stores),  
✔️ 그리고 그 도구들을 잘 쓰기 위한 학습 전략이 필요하다.  

<div style="margin-top: 3.3em;"></div>

다음 글에서는 Unit 3의 선택과제인 **고급 Unit 3b – “에이전트 심화”**를 정리해볼 예정!  
팟캐스트를 바당으로 더 복잡한 에이전트 구조나 활용법에 대해 깊이 들어가보겠습니다요. 바이바이  

<br><br>