---
layout: post
title:  "[Kaggle Gen AI] Day 2 - 임베딩의 다음 단계: 학습 구조와 벡터 검색 시스템 이해하기 🚀"
author: me
categories: [ Kaggle Gen-AI]
date: 2025-04-24 09:00:00
image: assets/images/20250402/day2_1.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_20/)에서는 텍스트, 이미지, 그래프 등 다양한 데이터 타입에 따라 **임베딩이 어떻게 달라지는지**, 그리고 각 방식이 어떤 아이디어를 바탕으로 만들어졌는지를 정리했다.  

이번 글에서는 임베딩의 **실전 활용**에 대해서,  
✔️ **임베딩 모델은 어떻게 훈련되고**,  
✔️ **어떻게 검색되고**,  
✔️ **어떤 시스템 위에서 돌아가는지**까지 알아보겠다.  

<div style="margin-top: 3.3em;"></div>

더 구체적으로, 이 글에서는 다음과 같은 내용을 정리해보겠다:  
- Dual Encoder 구조와 Contrastive 학습 방식  
- Pretraining vs Finetuning의 차이  
- 벡터 검색(Vector Search)과 ANN 기법들 (LSH, HNSW, SCaNN 등)  
- 벡터 데이터베이스와 운영 상의 고려사항들  
- 그리고 실제 응용 사례까지!

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=xCAVsst6WJ8&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=4)

<br>

---
### ⚙️ 임베딩 모델의 훈련 구조와 과정 살펴보기
요즘 임베딩 모델들은 대부분 **Dual Encoder 구조**를 사용한다.  

하나는 **쿼리 (query)**를 인코딩하는 인코더, 다른 하나는 **문서나 이미지 등 검색 대상(data)**을 인코딩하는 인코더.
이 두 네트워크가 함께 학습된다.  

이 두 인코더는 보통 **비슷한 데이터는 가까운 벡터로, 다른 데이터는 멀어진 벡터로** 학습되도록 만드는 구조인 **Contrastive Loss (대조 학습 손실 함수)**를 사용해서 훈련된다.  
좋은 건 끌어당기고, 나쁜 건 밀어내는 느낌이라고 보면 된다. 
> it's like attracting the good stuff and repelling the bad stuff.

<br>

---
##### 🧪 사전학습과 파인튜닝 – 두 단계로 나뉘는 학습
대부분의 모델 훈련은 두 가지 단계로 구성된다:
1. **Pre-training (사전학습)**
    대규모 데이터셋에서 **일반적인 표현 능력을 학습**하는 단계이다.  
    요즘은 BERT, T5, GPT, Gemini, CoCa 같은 **거대한 사전학습 모델의 가중치로 초기화**하는 방식이 흔하다.  

    이 방식은 임베딩 모델이 **사전학습 과정에서 이미 학습된 지식들을 그대로 활용**할 수 있게 해준다.  

    <div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 마치 한 발 더 출발하는 것처럼, 더 빠르게 좋은 성능을 낼 수 있다는 의미이다.  <small style="color:gray"><em>(it's like it’s getting a head start)</em></small>
    </div><br>

2. **Fine-tuning (파인튜닝)**  
    사전 학습된 모델을 **특정 태스크에 맞게 조정**하는 단계이다.  
    소규모 데이터셋을 활용해서, 우리가 원하는 도메인이나 작업에 맞게 **모델을 정밀하게 튜닝**한다. 

    파인튜닝에 사용할 데이터는 다양한 방식으로 만들 수 있다:  
    - 사람이 직접 라벨링한 데이터 <small style="color:gray">(manual labeling)</small>
    - 모델이 생성한 **synthetic data (합성 데이터)**
    - **model distillation**이나 **hard negative mining**같은 기술도 사용 가능하다. 

<br>

---
##### 📌 학습된 임베딩 활용하기
임베딩을 학습시킨 이후에는 다양한 **다운스트림 작업** <small style="color:gray">(downstream task)</small>에 활용할 수 있다.  

예를 들어, **분류기**<small style="color:gray">(classifier)</small>를 만들고 싶다면, 임베딩 위에 분류 레이어를 추가해서 함께 학습시키면 된다.  

이때 두 가지 선택지가 있다:  
- 임베딩 모델을 **고정 (freeze)**하고 분류기만 학습시키거나,
- 임베딩 모델까지 **함께 미세조정 (fine-tune)**하는 것  

→ 선택은 데이터 양이나 태스크의 특수성에 따라 달라진다.  

Vertex AI에서는 **텍스트 임베딩 모델을 커스터마이징하고 파인튜닝하는 도구들**도 잘 제공하고 있다.  

<br>

---
### 🧭 벡터 검색(Vector Search) – 임베딩에 꼭 필요한 기술
임베딩을 만들었다면, 이제 중요한 건 **이걸 어떻게 효율적으로 검색하느냐**다.  
단순 키워드 매칭이 아닌, **의미 기반 검색**을 가능하게 해주는 것이 벡터 검색이다.  

검색을 위해서는:  
- 먼저 전체 데이터에 대해 임베딩을 계산하고, 
- 그 벡터들을 **벡터 데이터베이스 (vectorDB)**에 저장한다. 
- 그리고 쿼리도 같은 방식으로 임베딩한 뒤,
- **쿼리 임베딩과 가장 가까운 벡터**를 찾아서 결과를 반환한다.  

<div style="margin-top: 3.3em;"></div>

하지만 데이터가 수백만, 수억 개가 넘어갈 때, 일일이 모든 벡터와 비교하는 건 **너무 느리고 비효율적**이다. 

그래서 사용하는 것이 **Approximate Nearest Neighbor (ANN)** 기법들이다. 

<br>

---
##### ⚙️ 대표적인 ANN 알고리즘들
ANN 기법을 사용하면 **정확한 결과를 훨씬 빠르게 찾을 수 있다.**  

1. **LSH (Locality Sensitive Hashing)**  
    비슷한 벡터들을 **같은 해시 버킷**<small style="color:gray">(hashing buckets)</small>에 넣어, 그 안에서만 빠르게 검색할 수 있게 만든다.  

    <div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 마치 <strong>우편 번호로 구역을 나누고</strong> 그 안에서만 찾는 것처럼 동작한다.   <small style="color:gray"><em>(the paper uses this analogy of postal codes which is pretty helpful)</em></small>
    </div>
    <div style="margin-top: 3.3em;"></div>

2. **Tree 기반 기법 (KD Tree, Ball Tree)**  
    벡터 공간을 반복적으로 분할하면서 검색 속도를 높이는 방식이다.  

    <div style="margin-top: 3.3em;"></div>

3. **FAISS 기반의 HNSW (Hierarchical Navigable Small World)**  
    HNSW는 **계층적 그래프 구조**를 만들어서 검색한다. 
    - 먼저 **멀리 떨어진 노드들로 빠르게 전체 영역 중 대략적인 위치를 찾고**
    - 그 다음엔 **가까운 연결들을 따라 정확한 이웃을 찾는 방식**이다.

    > it starts with long-range connections to quickly get in the right neighborhood.

    <div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 HNSW는 요즘 여러 서비스에서 <strong>가장 많이 쓰이는 알고리즘</strong> 중 하나다. <small style="color:gray"><em>(so HNSW is like the go-to algorithm for a lot of applications. it's a really solid performer.)</em></small>
    </div>
    <div style="margin-top: 3.3em;"></div>

4. **SCaNN (Scalable Nearest Neighbor)**  
    Google이 만든 ANN 기법으로, **매우 큰 데이터셋**에서도 빠르게 작동하도록 공간을 먼저 **작게 쪼개고**, 여러 스코어링 기법을 조합해서 최종 결과를 찾는다. 
    > so it's like it's breaking down the problem into smaller, more manageable chunks.

<br>

---
### 🗃️ 벡터 데이터베이스 – 벡터를 저장하고 검색하는 기반 시스템
기존 관계형 데이터베이스는 **고차원 벡터나 유사도 기반 검색**에 적합하지 않기 때문에, 이 모든 걸 가능하게 하려면 **벡터 전용 데이터베이스**가 필요하다.  
> so it's like their purpose is built for the world of embeddings.

최근에는 Postgres나 Elastic 같은 전통적EB들도 **Vector Search 기능을 추가**하는 경우가 늘고 있는데, 이런 방식을 **하이브리드 검색**<small style="color:gray">(hybrid search)</small>라고 부른다.  

<br>

##### 💡 벡터 검색 워크플로우
1. 데이터에 대해 임베딩을 만든다. 
2. 임베딩 벡터를 인덱싱한다. 
3. 쿼리를 임베딩한다.
4. 가장 유사한 벡터를 찾아 결과로 반환한다.

<div style="margin-top: 3.3em;"></div>

그리고 현재 사용 가능한 벡터DB 시스템들:
- Vertex AI Vector Search
- FAISS
- Pinecone
- Weaviate (wv8)
- ChromaDB
- Cloud SQL for Postgres <small style="color:gray">(vector extension 포함)</small>, etc.  

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
💬 각기 다른 성능과 특징을 가지므로, 사용 목적에 따라 선택이 필요하다. <small style="color:gray"><em>(they all have different strengths and weaknesses so you have to choose the right one for your needs. it's a growing ecosystem.)</em></small>
</div><br>

---
### 🧩 운영할 때 고려해야 할 점들
알고리즘만 중요한 게 아니라, **전체 infrastructure 관점에서 운영을 잘 해야 한다.**  
- **확장성(Scalability)**
- **가용성(Availability)**
- **일관성(Consistency)**
- **업데이트 / 백업 / 보안** 등  
도 중요한 고려 요소이다.  

또한 **임베딩 자체도 시간이 지나면 바뀔 수 있다.** 
사전 학습된 모델이 업데이트되면, 기존 벡터를 **다시 임베딩하고, 인덱스도 재구성**해야 할 수 있다.

그리고 모든 문제를 임베딩만으로 해결할 수 없다.  
경우에 따라선 **기존의 전통적인 풀텍스트 검색**<small style="color:gray">(full text search)</small>과 **벡터 검색을 병행**하는 하이브리드 방식이 더 적합할 수 있다. 
> so it's about choosing the right tool for the job.

<br>

---
### 🎨 임베딩 응용 사례
지금까지 임베딩의 기본 개념부터, 데이터 타입별 임베딩, 훈련 방식, 검색 알고리즘, 그리고 데이터베이스까지 정리해봤다.  
day2의 마지막으로, 임베딩을 가지고 **실제로 무엇을 할 수 있을지** 알아보겠다.  

<div style="margin-top: 3.3em;"></div>

##### 🔍 임베딩의 주요 활용 분야
팟캐스트에서는 아래와 같은 다양한 응용 분야를 소개하고 있다:  
- **정보 검색** <small style="color:gray">(Information Retrieval)</small>
- **추천 시스템** <small style="color:gray">(Recommendation Systems)</small>
- **의미 기반 유사도 계산** <small style="color:gray">(Semantic Similarity)</small>
- **분류** <small style="color:gray">(Classification)</small>
- **클러스터링** <small style="color:gray">(Clustering)</small>
- **재정렬** <small style="color:gray">(Re-ranking)</small>  

그리고 여기에 **벡터 저장소**와 **ANN 검색 기법**을 결합하면 훨씬 더 강력한 시스템을 만들 수 있다.  

예를 들면:  
- **RAG** <small style="color:gray">(Retrieval-Augmented Generation)</small>
- **대규모 검색 엔진** <small style="color:gray">(large scale search engines)</small>
- **개인화 추천 시스템** <small style="color:gray">(personalized recommendation systems)</small>
- **이상 탐지 시스템** <small style="color:gray">(anomaly detection)</small>
- **Few-shot 분류기** <small style="color:gray">(Few-shot classification)</small>

<div style="margin-top: 3.3em;"></div>

##### 🏗️ 임베딩은 랭킹 시스템의 시작점이기도 하다
대규모 시스템에서는 랭킹<small style="color:gray">(ranking)</small> 과정이 여러 단계로 나뉘는데, **임베딩은 그 첫 번째 단계에서 아주 자주 사용**된다.  
→ 전체 후보군을 빠르게 좁히기 위한 필터링 역할이다.  
→ 특히 SCaNN 같은 ANN 기법들이 이 역할에 특화되어 있다.  

<div style="margin-top: 3.3em;"></div>

##### 📚 game changer RAG
이전에 다뤘던 **RAG 구조**는, 임베딩 기반 검색을 언어 모델과 결합해 **정확도를 높이고 환각**<small style="color:gray">(hallucination)</small>**을 줄이는 데 효과적**이다. 

<div style="margin-top: 3.3em;"></div>

##### ✅ 정보의 신뢰성을 높이기 위해
모델이 어떤 정보를 기반으로 응답하는지를 **출처로 함께 제공**하면, 사용자가 결과를 검증할 수 있어  **신뢰성 있는 시스템**이 될 수 있다.  

<br>

---
### 💭 오늘 챙겨간 것들
이번 글에서는 임베딩 모델이  
✔️ 어떻게 훈련되고 (Dual Encoder, Contrastive Loss),  
✔️ 어떻게 활용되고 (Downstream Task),  
✔️ 어떻게 검색되는지 (Vector Search & ANN),  
✔️ 그리고 어떤 시스템 위에서 운영되는지 (Vector DB & Infrastructure)  
전반적인 워크플로우를 정리해봤다.  

→ 임베딩과 벡터 검색은 **의미 기반의 데이터를 다루는 데 매우 강력한 도구**이며, **각 기술의 장단점을 잘 이해하고, 내 문제에 맞게 조합하는 것이 핵심**이다.

Gemini 기반 임베딩 모델이나 SCaNN처럼, 새로운 기술들이 계속 등장하고 있고, **그 가능성은 이제 막 시작된 수준**이라고 한다. 

<div style="background: #f0f8ff; border-left: 4px solid #0277bd; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  🧭 중요한 건, <strong>논문을 읽고</strong>, <strong>도구를 써보고</strong>, <strong>작게라도 실험해보면서</strong>,
  직접 써봐야 내가 쓸 수 있는 기술이 된다.
</div>

<br><br>