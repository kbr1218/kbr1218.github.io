---
layout: post
title:  "[Kaggle Gen AI] Day 3 - 똑똑한 검색: Agentic RAG & AI 리서처 팀 🚀"
author: me
categories: [ Kaggle Gen-AI]
date: 2025-05-07 09:00:00
image: assets/images/20250402/day3_2.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_27/)에서는 멀티 에이전트 시스템의 개념과 장점, 그리고 다양한 설계 패턴과 평가 방식까지 정리해봤다.  
에이전트를 팀처럼 구성해 문제를 나누고, 서로 협업하거나 검증하는 방식이 어떻게 **신뢰성과 효율성**을 동시에 가져올 수 있는지!

이번 글에서는 그 연장선에서, **RAG(Retrieval-Augmented Generation)** 구조에 에이전트의 지능을 더한 진화된 형태인 **Agentic RAG**에 대해 정리해보려 한다.
> it's like giving the LLM a brain boost.

단순히 외부 문서를 찾아서 넣어주는 게 아닌, **질문을 분해하고, 검색을 정교화하며, 결과를 평가·종합하는 AI 리서처 팀이 LLM 뒤에 붙는 느낌**이다.

그리고 이걸 제대로 활용하려면, 검색 시스템을 얼마나 잘 설계하고 최적화하는지가 굉장히 중요하다고 한다!

<div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  ⚠️ <strong>주의사항</strong><br>
  이번 팟캐스트에는 중간에 내용이 반복되거나 말이 끊기는 부분이 있다. <small style="color:gray">(AI로 만든 팟캐스트라더니 검토하는 걸 잊으셨나 봄)</small>
</div>
<div style="margin-top: 2.3em;"></div>

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=7rbSwt-7odQ&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=5)

<div style="margin-top: 2.3em;"></div>

---
### ✨ Agentic RAG란? — RAG + 지능
**Agentic RAG**는 말 그대로 **에이전트를 결합한 RAG 시스템**이다. **Retrieval-Augmented Generation with Agents**.  
> is this like RAG but on steroids? - you could say that.

기존의 RAG는 **지식 기반에서 관련 정보를 검색하고, LLM이 이를 바탕으로 응답을 생성**하는 구조였지만, 복잡한 쿼리나 **추론이 필요한 멀티스텝 문제**에는 한계가 있었다.  
> it's essentially performing a sophisticated lookup, but it's not really understanding what it's retrieving.

Agentic RAG는 이 한계를 **에이전트를 투입해 보완**한다.  
에이전트들은 이제 검색을 넘어서서

- 쿼리를 **능동적으로 재구성**하고, <small style="color:gray">(actively refine the search queries)</small>  
- 검색 결과를 **평가하고 선별**하며, <small style="color:gray">(evaluate the retrieved information)</small>  
- 상황에 따라 **새로운 지식에도 적용**할 수 있다. <small style="color:gray">(adapt to new knowledge)</small>  

> so it's like having a team of AI research assistants. it's like giving the LLM a brain boost.

<div style="margin-top: 3.3em;"></div>

그 결과:  
- **정확도** 향상 <small style="color:gray">(improved accuracy)</small>  
- **문맥 이해력** 강화 <small style="color:gray">(enhanced contextual understanding)</small>  
- **적응력** 개선 <small style="color:gray">(increased adaptability)</small>  

같은 기존 RAG보다 한층 진화된 성능을 기대할 수 있다. 

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
💬 특히, 방대한 양의 문서 속에서 핵심을 찾아야 하는 과제 (e.g. 법률 리서치, 과학적 발견, 기술 매뉴얼 설계)에 굉장히 유용하다고 한다.  <em style="color:gray">(anywhere you need to find the needle in the haystack!)</em>
</div><br>

---
### 🚗 예시로 보는 Agentic RAG — 단순 키워드 검색을 넘어
팟캐스트에서는 실제 적용 예시로 **자동차 매뉴얼 시스템**을 소개한다.

우리가 자동차 회사를 위한 시스템을 구축하고 있다고 상상해보자. 그리고 사용자가 하는 **모든 질문에 답할 수 있는 정말 지능적인 자동차 매뉴얼**을 만들고자 한다. 보통의 자동차 매뉴얼은 사용자 비친화적이면서 복잡하고 보기 불편한 걸로 유명하다.  

하지만 **Agentic RAG**를 사용한다면, 이걸 완전히 바꿀 수 있다. 훨씬 더 **직관적인 방식으로 정보를 제공**할 수 있게 되는 것이다. 

만약 사용자가:  
> "How do I pair my phone with the Bluetooth system?" ("차량 블루투스에 휴대폰을 어떻게 연결하나요?")  

라고 질문한다면, 전통적인 RAG는 **Bluetooth / phone / pair** 등의 키워드가 들어간 텍스트 조각들을 그냥 찾아올 뿐이다.  
→ 하지만 이건 검색 결과가 너무 **부정확하거나, 핵심을 놓치기 쉬운 방식**이다.

Agentic RAG에서는 다르다:
- 먼저, **전문 에이전트** <small style="color:gray">(e.g. Car Manual Agent)</small>가 사용자의 질문을 **세부 단계로 분해**한다.  
  e.g. 블루투스 켜기 → 기기 검색 → 핀 코드 입력... <small style="color:gray">(요런식으로)</small>  
  해결해야 할 과제를 더 작고 관리 가능한 과제로 쪼개는 것이다. <small style="color:gray">(divide and conquer 같이??)</small>   
  > so it's creating like a mental checklist.

- 단계를 쪼갰으면, 각 단계별로 **더 구체적인 쿼리를 생성**한다.  
  e.g. “블루투스 활성화 방법”, “자동차 핀코드 위치”

- 검색된 결과 중에서 **가장 관련성 높은 조각을 평가 및 선별** <small style="color:gray">(evaluate and select)</small>하고, 여러 출처에서 얻은 정보를 **종합하여 완성도 높은 응답**을 만들어낸다. 
  > it can synthesize information from multiple sources.

몇 번이나 반복하는 말이지만, Agentic RAG는 단순 검색 기능이 아니라 **LLM 뒤에 붙은 전문 리서처 팀**처럼 작동하면서,
- 더 나은 정보
- 더 정밀한 응답
- 더 쉬운 이해

를 만들어 낼 수 있다. 
> it's like having an AI expert in your pocket.

이 구조는 단지 멋진 데모용 기술이 아니라, **실제로 실생활의 복잡한 문제를 해결할 수 있는 기반 기술**이라는 점에서 주목할 만하다. 

<br>

---
### 🛠️ Agentic RAG의 성능을 끌어올리는 핵심: 검색 최적화
Agentic RAG가 아무리 똑똑해도, **엉터리 데이터를 검색해오면 무용지물**이다. 
> even the smartest agents, they're only as good as the information they have access to. garbage in, garbage out, as they say.

그래서 Agent RAG를 설계하기 전에 **검색 시스템을 먼저 제대로 최적화**해야 한다

> before we get off all excited and start building these super fancy systems, we need to make sure our basic search engine is up to snuff

<br>

##### 1️⃣ 문서 구조화: Chunking
먼저 해야 할 일은 문서를 **적절한 단위로 분할** <small style="color:gray">(chunking)</small>하는 것이다.  
사용자의 쿼리에 대응할 수 있는 **의미 있는 최소 단위**로 쪼개야 한다.  

너무 작으면 정보가 부족하고, 너무 크면 검색 정확도가 떨어진다.  

> breaking them down into meaningful units that align with the types of queries you're expecting.

<br>

##### 2️⃣ 메타데이터 추가: 문맥을 풍부하게
각 청크에 **적절한 메타데이터**를 덧붙이는 것도 중요하다.  
<small style="color:gray">e.g. 키워드, 동의어, 작성자, 날짜, 카테고리, 태그 등</small>

> it's all about giving the search engine as much information as possible. the more it knows, the better it can perform.

→ 검색의 정확도와 범위를 모두 끌어올릴 수 있다.

<br>

##### 3️⃣ 임베딩 품질 향상: 파인튜닝 or 서치 어댑터
특화 도메인에서는 **임베딩 모델을 파인튜닝**하거나, **Search Adapter**를 적용하는 것도 효과적이다. 

> you're making the embedding model an expert in your data.

→ 내 데이터에 최적화된 검색 품질 확보.  

<br>

##### 4️⃣ 속도: 빠른 벡터 DB로 유저 경험 개선
사용자는 **기다리지 않고** 답을 받기를 원하기 때문에, 벡터 검색은 **빠를수록 좋다**.
> speed matters, even in the AI world. milliseconds count.

<br>

##### 5️⃣ Ranker 적용: 최종 정렬
벡터 검색은 빠르지만 **대략적인 유사도 기반**이라, 최종 결과가 **가장 관련 있는 순서로 정렬되지 않을 수**도 있다.  
그래서, 별도의 **정교한 랭커**<small style="color:gray">(rankers)</small>로 **재정렬** <small style="color:gray">(reranking)</small>하는 과정이 필요하다.

> you want the cream of the crop.

<br>

##### 6️⃣ Check Grounding: 팩트 검증 단계
Check Grounding은 말 그대로 **검색 결과 기반의 팩트체크 시스템**이다.  
에이전트가 생성한 응답이 **실제 검색된 문서에 기반한 것인지** 검증하기 때문에 **hallucination(환각)** 방지에 효과적

> it's like a safety net. it makes sure that any claims made by the AI are actually supported by the retrieved information.

<br>

---
### 🧰 Google Cloud가 제공하는 검색 도구들
이 모든 과정을 직접 구현하기는 쉽지 않지만, Google Cloud에서는 이를 위한 다양한 도구를 제공하고 있다고 한다. 

| 목적 | 도구 | 설명 |
|------|------|------|
| 올인원 검색 솔루션 | **Vertex AI Search** | 구글 수준의 검색 기능을 갖춘 완성형 솔루션 |
| 커스텀 구축 | **Vertex AI Search Builder APIs** | DIY 스타일. 유연하게 원하는 구조로 검색 시스템 설계 가능 |
| RAG 파이프라인 자동화 | **Vertex AI RAG Engine** | Agentic RAG 전체 흐름을 관리하는 통합 오케스트레이터 |

<div style="margin-top: 2.3em;"></div>

> no matter your level of expertise or how much control you want, Google Cloud has a solution.

<br>

---
### 🧪 현실에 적용된 멀티 에이전트 시스템 — 과학과 자동차
멀티 에이전트 시스템은 이론 속 개념이 아니라, **이미 현실에서 작동 중인 기술**이다.  
팟캐스트에서는 두 가지 사례를 중심으로, 그 가능성과 적용 방식을 설명하고 있다. 

<br>

##### 1️⃣ Co-Scientist: AI가 하는 과학?
Google DeepMind가 개발 중인 **Co-Scientist**는 **과학적 발견을 가속화하기 위한 멀티 에이전트 시스템**이다.  
> so it's like having a team of AI powered scientists working tirelessly to make new discoveries. it's like AI doing science. 

- 에이전트들이 가설을 생성하고
- 실험 결과를 평가하고
- 새로운 매커니즘을 제안하며
- 가능한 약물 후보까지 도출하는 역할을 수행한다. 

<div style="margin-top: 3.3em;"></div>

예를 들어, 실제 연구에서는 **간 섬유증 치료제** 탐색에서, 이미 존재하는 약물뿐 아니라, **새로운 작용 메커니즘과 후보 물질**도 제안했다고 한다.  
> it's not just finding what's already known. it's pushing the boundaries of scientific knowledge. 

이 사례는 멀티 에이전트 시스템이 단순한 자동화를 넘어서, **인간의 지적 탐구를 보완하고 확장하는 수준**에 도달하고 있음을 보여준다. 
> it's about augmenting human intelligence and pushing the boundaries of what's possible.

<br>

#### 2️⃣ Automotive AI: 에이전트 팀으로 움직이는 차량 시스템
자율주행과 인포테인먼트 등, 현대의 자동차는 **수많은 AI 기능을 하나의 시스템에 집약**하고 있다.

하지만 이 모든 기능을 **단일 AI로 처리하는 것은 비현실적**이다. 
> it's like asking one person to be an expert, in everything. too much for one system to handle well.

<div style="margin-top: 3.3em;"></div>

그래서 등장한 방식이 바로 **멀티 에이전트 기반 차량 시스템**이다. 각 에이전트가 역할을 나눠 협업한다. 

예를 들면:  
- 🚘 내비게이션 안내는 → `Navigation Agent`  
- 🎵 음악·미디어 검색은 → `Media Agent`  
- 📖 차량 매뉴얼 응답은 → `Manual Agent`  
- 🌐 일반 질문은 → `General Knowledge Agent`

> it's like having a whole team of AI assistants in your car. your own personal AI pit crew.

<div style="margin-top: 3.3em;"></div>

이들이 협업하는 방식에도 여러 가지가 있다:  
- **계층 구조 (Hierarchical)**: 중앙 `오케스트레이터`가 요청을 분배  
- **다이아몬드 구조 (Diamond)**: `중재자 에이전트`를 통해 응답 스타일/톤을 조정  
- **P2P 구조 (Peer-to-peer)**: 에이전트들 간 **직접 소통**하여 유연한 협업  
- **협업 구조 (Collaborative)**: 복잡한 질문에 대해 **여러 에이전트가 함께 응답 구성**

<div style="margin-top: 3.3em;"></div>

그 결과 차량 시스템은 다음과 같은 장점을 갖는다:  
- **전문화**: 더 정교한 응답 제공  
- **속도와 효율**: 정확히 필요한 기능만 작동  
- **복원력**: 하나의 에이전트가 실패해도 시스템은 계속 작동   
> it's like having a backup plan built in.

<div style="margin-top: 3.3em;"></div>

즉, 이 구조는 **지능적이면서도 견고한 자동차 AI 시스템**을 만드는 핵심이다.
> multi-agent systems seem like the key to building truly intelligent and adaptable automotive AI.

<br>

---
### 💭 오늘 챙겨간 것들
이번엔 **Agentic RAG**이라는 강력한 구조를 다뤘다.  

✔️ **Agentic RAG = RAG + 에이전트 팀워크**  
✔️ 자동차 매뉴얼 예시: Agentic RAG를 사용하면 질문을 쪼개고, 단계별 검색하고, 종합까지 한다는 것  
✔️ **Agentic RAG도 검색이 전부다**: 청킹, 메타데이터, 임베딩 튜닝, 속도, 랭커, check grounding으로 검색 시스템을 먼저 다듬어야 한다는 것  
✔️ Google Cloud가 제공하는 유용한 검색도구들  

다음 게시물에서는 Agent 개발을 위한 실제 도구들, 그리고 "AI와 계약을 맺는다는 게 무슨 뜻일까?"라는 철학적으로 보이는 질문까지 이어진다.  

<br><br>