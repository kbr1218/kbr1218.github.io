---
layout: post
title:  "[Kaggle Gen AI] Day 2 - 임베딩의 종류: 텍스트부터 그래프까지, 데이터별 표현 방식 알아보기 🚀"
author: me
categories: [ Kaggle Gen-AI]
date: 2025-04-23 09:00:00
image: assets/images/20250402/day2_1.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_19/)에서는 임베딩의 개념과 평가 방법, 그리고 RAG 구조처럼 실전에서 어떻게 활용되는지를 살펴봤다.  

이번 글에서는 임베딩이 **어떤 방식으로 구성되고**,  **어떤 데이터에 어떻게 적용되는지** 정리해보려고 한다. 
텍스트, 이미지, 구조화된 표 형식의 데이터, 네트워크 그래프까지—  데이터의 형태에 따라 **임베딩 방식도 달라진다.**  

핵심은 **중요한 정보를 담고 있는 저차원 표현**을 만드는 것이다. 
> it's all about creating low-dimensional representations that capture the essential information.

이번 글에서는 각 데이터 타입별로 어떤 임베딩 기법들이 사용되는지, 그리고 그 기술들이 **어떻게 발전해왔는지**를 차근차근 살펴보겠다.

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=xCAVsst6WJ8&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=4)

<br>

---
### 📝 텍스트 임베딩 – 단어부터 문서까지, 의미를 숫자로 바꾸는 법
팟캐스트에서는 데이터의 종류에 따라 임베딩을 나누고 있고, 그중에서도 가장 먼저 소개된 건 **텍스트 임베딩**이다.  

텍스트 데이터를 벡터로 표현하는 방식은 자연어 처리의 거의 모든 작업에서 중심이 되는 기술이다. 
> which are kind of the foundation for a lot of NLP applications.

단어, 문장, 문단, 그리고 전체 문서를 **의미를 담은 숫자 벡터로 바꾸는 것**이 핵심이다.   
벡터에서는 이를 다시 두 가지로 구분하고 있다:  
- **단어 기반 임베딩 (Token/Word)**
- **문서 기반 임베딩 (Document)**

<br>

##### 🧩 토크나이징
임베딩 전처리 과정에서는 먼저 텍스트를 **토큰(token)**이라는 작은 단위로 쪼개고, 각 토큰에 **고유한 숫자 ID를 부여**해서 숫자 벡터로 바꾼다.  

가장 단순한 방식은 one-hot encoding이지만, 이건 단어 간 의미 관계를 전혀 반영하지 못하기 때문에, 실제로는 임베딩을 사용하는 경우가 많다. 

<br>

##### 🧠 Word Embedding - 단어의 의미를 벡터로
단어를 고정된 길이의 벡터로 표현하면서 **단어 간 의미 관계**를 파악할 수 있게 해주는 방식이다.  

대표적인 단어 임베딩 모델들은 아래와 같다:  
- **Word2Vec**
- **GloVe**
- **FastText**
- **Swivel**  

이 중에서도 Word2Vec은 "문맥을 보면 단어의 의미를 알 수 있다"는 아이디어에서 출발했다.  
즉, **어떤 단어가 주로 어떤 단어들과 함께 등장하느냐**가 그 단어의 의미를 결정한다.  
> so it's all about context.

Word2Vec에는 두 가지 학습 구조가 있다:  
- **CBOW (continuous Bag of Words)**: 주변 단어들로 중심 단어를 예측
- **Skip-gram**: 중심 단어로 주변 단어들을 예측  

CBOW는 **학습 속도가 빠르고 자주 등장하는 단어에 잘 작동**하지만, Skip-gram은 **드물게 등장하는 단어나 작은 데이터셋에 더 적합**하다. 

<br>

##### 🛠️ Word2Vec의 확장과 대안들
**FastText**는 Word2Vec에서 더 발전되어, 단어 내부의 접두사·접미사 단위같은 서브워드<small style="color:gray">(subword)</small> 구조도 함께 학습하여 그 안에서 의미를 포착할 수 있다.  
> so it's like it's getting even more granular with the meaning. 

**GloVe**는 전체 코퍼스<small style="color:gray">(corpus)</small>에서 단어들이 얼마나 함께 등장하는지를 세어, 단어들 사이의 **전반적인 관계**<small style="color:gray">(global co-occurrence)</small>를 학습힌다. 
> so like it's looking at the big picture.

**Swivel**은 GloVe와 유가하게 단어 간 **동시 출현 행렬**<small style="color:gray">(co-occurrence matrix)</small>을 사용하는 방식이지만, **더 큰 규모의 대규모 데이터셋**에서도 잘 잘동하도록 설계되어 있다.  
그래서 수백만 개의 단어가 있는 상황에서도 효율적으로 **분산 학습**<small style="color:gray">(distributed processing)</small>이 가능하다.  

이렇게 학습된 임베딩 벡터는 2차원 공간에 시각화하면, 의미가 유사한 단어들이 서로 가까운 위치에 모이는 특성을 보인다.  
→ 그래서 시각적으로도 **단어 간의 의미 관계를 확인**할 수 있다.

<br>

---
### 📄 Document Embedding – 문단과 문서 전체를 표현하는 방식
이제는 단어가 아니라, **더 큰 텍스트 단위인 문단이나 전체 문서**를 어떻게 벡터로 표현하는지에 관한 것이다.  
초기 방식부터 최신 LLM 기반 임베딩까지, 문서 임베딩의 발전은 크게 세 단계로 나눠볼 수 있다:  

<br>

##### 1️⃣ Bag-of-Words 기반 임베딩
문서 임베딩의 가장 초창기 접근법은 단어의 단순 빈도 기반 통계 기법에서 시작했다.  

대표적인 방법 두 가지:
1.  **LSA (Latent Semantic Analysis)**
    단어-문서 행렬을 만든 다음, 차원 축소<small style="color:gray">(dimensionality reduction)</small>를 적용하여  **단어와 문서 간 숨겨진 의미 관계**를 찾아내는 방식이다.  
    > like it's looking for the underlying themes.

    <div style="margin-top: 3.3em;"></div>

2. **LDA (Latent Dirichlet Allocation)**
    확률 기반 접근법으로, 문서를 **여러 주제**<small style="color:gray">(topic)</small>**의 혼합물**로 보고 각 단어가 어떤 주제에 속할 확률을 계산하는 모델이다.  
    > it's like each document is a recipe and the words are the ingredients.

    <div style="margin-top: 3.3em;"></div>

3. **TF-IDF / BM25**  
    문서에서 자주 등장하지만 전체 코퍼스에서는 드문 단어일수록 **더 중요한 단어로 간주**해 가중치를 부여하는 통계적 기법이다.    
    BM25는 이 TF-IDF 계열 중에서 특히 **강력한 베이스라인으로 자주 쓰이는 방식**이다.  
    > BM25 is highlighted as a really strong baseline.

하지만 이 방식들은 모두 **단어의 순서를 전혀 고려하지 않는다**    
> all these bag of words models ignore the order of words. they just treat each document as like a bag of words.

단어의 순서를 고려하지 않기 때문에 **문맥이나 의미를 온전히 반영하기 어렵다**는 단점이 있다  
→ 그래서 **Doc2Vec** 같은 더 진보된 방식들이 등장하게 된다.  

<br>

---
##### 2️⃣ Doc2Vec – 문서 전체를 하나의 벡터로
**Word2Vec**에서 영감을 받아, 문서 단위를 하나의 벡터로 표현하는 방식이다.  
> so it's like Word2Vec but for whole document.

Doc2Vec에서는 각 문서에 고유한 **Paragraph Vector**를 부여하고, 단어 벡터와 함께 학습시켜 문서 전체를 벡터로 표현한다. 

이 방식은 **문서 구조 전체를 고려한 임베딩**을 만들 수 있다는 점에서 기존 BoW보다 훨씬 강력하다.  

<br>

---
##### 3️⃣ Pre-trained LLM 기반 문서 임베딩 – BERT부터 Gemini까지
최근 몇 년 사이, 문서 임베딩의 판도를 완전히 바꾼 건 **사전 학습된 대형 언어 모델 (LLM)**의 등장이었다.  

이전의 통계 기법들과 달리,  
이 모델들은 **딥러닝 기반의 복잡한 신경망 구조** <small style="color:gray">(deep neural networks)</small>, **막대한 학습 데이터** <small style="color:gray">(massive amounts of training data)</small>, 그리고 **파인튜닝** <small style="color:gray">(idea of fine-tuning)</small> 기법을 활용한다.  

<div style="margin-top: 3.3em;"></div>

이 흐름의 시작을 연 대표적인 모델이 바로 **BERT** (2018)이다.  
- Transformer 아키텍처 기반 모델
- Masked Language Modeling & Next Sentence Prediction 방식으로 학습하고
- 수많은 문장들로부터 문맥을 학습해, **문맥에 따라 변하는 임베딩** <small style="color:gray">(contextualized embedding)</small>을 생성한다.  

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 같은 단어라도 <strong>어떤 문장에 있느냐에 따라 벡터가 달라진다.</strong> <small style="color:gray"><em>(it's not just a static representation of the word itself, it takes into account the context)</em></small>
</div><br>

또한, 입력 전체를 대표하는 **CLS 토큰**이 자주 사용되며, 이 벡터를 문서 임베딩으로 활용하는 경우가 많다. 
BERT 이후에는 다양한 파생 모델들이 등장했는데, 예를 들면:  
- **Sentence-BERT (SBERT)**
- **SimCSE**
- **E5**  
→ 이 모델들은 특히 **문장 단위의 임베딩**을 고품질로 생성하는 데 최적화되어 있다.   

<div style="margin-top: 3.3em;"></div>

그리고 여기서 멈추지 않고, **더 크고 정교한 LLM들**이 계속해서 등장하고 있다:  
- **TS**, **PaLM**, **Gemini**, **GPT**, **LLaMA**, etc.

<div style="margin-top: 3.3em;"></div>

이러한 모델들은 강력한 문장·문서 임베딩을 만들어내는 데도 활용되고 있고,  대표적으로 **GTR, Sentence-T5**같은 임베딩 특화 모델들이 있다.  

최근에는 **Google의 Gemini 아키텍처 기반의 임베딩 모델**이 Vertex AI 플랫폼에 공개되어, 다양한 벤치마크에서 **인상적인 성능**을 보여주고 있다. 
> embeddings are like constantly evolving. it's a fast moving field.

<br>

---
##### 🧱 다양한 형태의 고급 문서 임베딩들
최근에는 문서 임베딩에서도 **유연하게 차원을 선택할 수 있는 구조**들이 등장하고 있다. 
> there's this idea of matrix embeddings which let you choose the dimensionality of the embeddings based on what you need for your specific task. 

문서에 **텍스트뿐 아니라 이미지까지 포함**되어있는 경우, 하나의 벡터로 표현하기보다는 **여러 개의 벡터로 나눠 표현하는 방식**이 등장하고 있다:
- **ColBERT**
- **xTR**
- **KPali**  

각각은 문서 안의 세부 요소들을 **다중 벡터**<small style="color:gray">(multi-vector)</small>로 나누어 표현하고, 검색/질문응답/분류 같은 태스크에 더 정교한 임베딩을 제공한다.  

또한, 팟캐스트에서는 기존의 얕은 모델들과 딥러닝 기반 모델들 사이의 차이도 강조하고 있다. 
- **학습에 훨씬 더 많은 데이터와 연산 자원이 필요**하지만,
- 그만큼 **문맥과 의미를 이해하는 능력도 훨씬 강력**하다.  

TensorFlow Hub와 Vertex AI에서는 이런 **사전 학습된 임베딩 모델들**을 직접 사용해볼 수 있고, LangChain이나 BigQuery 같은 도구와도 연동이 가능하다. 

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 도구는 점점 <strong>정교해지면서도 점점 더 접근 가능해지고 있는 중</strong>이다.  <small style="color:gray"><em>(the tools are getting more sophisticated but they're also becoming more accessible)</em></small>
</div>

그리고 이 개념은 **이미지에서도 동일하게 적용**된다.  

<br>

---
### 🖼️ Image Embedding – 이미지를 벡터로 표현하는 법
이미지 임베딩은 보통 **CNN (합성곱 신경망)**이나 **Vision Transformer**를 대규모 이미지 데이터셋으로 학습시켜 생성한다.  

주로 모델의 마지막 쪽 계층에서 나오는  **활성값**<small style="color:gray">(activation)</small>을 임베딩으로 사용하는데, 이 벡터 안에는 **이미지 안에서 모델이 뽑아낸 중요한 특징**들이 있다. 
> it's like the model is learning to see the important parts of the image. 

<br>

---
### 🔀 Multimodal Embedding – 텍스트와 이미지, 함께 다루기
**멀티모달 임베딩**<small style="color:gray">(multimodal embedding)</small>은 서로 다른 데이터 타입<small style="color:gray">(e.g. 텍스트와 이미지)</small>의 임베딩을 **하나의 통합된 표현으로 결합하는 방식**이다.  

예를 들어, 이미지와 텍스트를 모두 포함하는 문서를 대상으로, 텍스트 쿼리만으로 해당 문서를 **검색하거나 분류할 수 있는** 시스템도 등장하고 있다.  

대표적인 예시로, Google의 **Cali** 모델은 **OCR 없이도 이미지+텍스트 문서에서 직접 검색이 가능**하고, Vertex AI에서는 이런 이미지 및 멀티모달 임베딩도 직접 계산해볼 수 있는 API를 제공하고 있다. 

<br>

---
### 📊 Structured Data Embedding – 표 형식 데이터의 임베딩
그렇다면 **데이터베이스 테이블처럼 행과 열이 있는 데이터**같은 구조화된 데이터에도 임베딩을 적용할 수 있을까?  

가능하긴 하지만, 구조화된 데이터의 의미는 **스키마**<small style="color:gray">(schema)</small>와 **문맥** <small style="color:gray">(context)</small>에 따라 크게 달라지기 때문에 **텍스트나 이미지처럼 단순하게 적용되진 않는다.**

일반적인 표 구조(행-열 데이터)에는 **PCA** 같은 차원 축소 기법을 사용해 각 행을 임베딩 벡터로 변환할 수 있다.  
이런 벡터는 **이상치 탐지**<small style="color:gray">(schema)</small>, 또는 **다른 ML 모델의 입력 피처**로 활용된다. 

또한, **추천 시스템** <small style="color:gray">(recommendation systems)</small>처럼 **사용자(user)와 아이템(item)** 간의 관계가 중요한 경우에는 두 엔티티를 **같은 임베딩 공간에 매핑**해서 서로 유사한 사용자/아이템을 찾고, 적절한 추천을 제공할 수 있다.  
> so it's all about finding those hidden connections.

게다가 구조화된 임베딩에 **제품 설명, 사용자 리뷰 같은 비정형 텍스트 임베딩**을 결합하면 훨씬 더 **풍부한 표현**<small style="color:gray">(richer representations)</small>을 만들 수 있다. 

<br>

---
### 🕸️ Graph Embedding – 관계까지 담는 임베딩
마지막 임베딩은 **그래프 기반 데이터**에 관한 것이다.  

그래프 임베딩에서는 **개별 데이터 포인트만 표현하는 것이 아니라, 그들 사이의 관계까지 벡터에 담는 것**이 핵심이다. 
> you're not just representing individual data points, you're representing relationships

예를 들어, 소셜 네트워크처럼 사람(노드)과 관계(엣지)로 구성된 네트워크에서 각 사람을 벡터로 표현할 때, 단순히 **속성 정보**<small style="color:gray">(attributes)</small>만 담는 것이 아니라, **누구와 연결되어 있는지**<small style="color:gray">(position in the network)</small>, **네트워크 상에서의 위치**<small style="color:gray">(who they're connected to)</small>까지도 함께 반영된다. 
> it's like it's encoding the social fabric.

이런 방식은:
- 누가 누구를 알 거 같은지 예측하거나
- 사용자 분류
- 그래프 기반 추천 시스템 구축  
등과 같은 다양한 활용이 가능하다.  

<div style="margin-top: 3.3em;"></div>

그리고 대표적인 그래프 임베딩 알고리즘으로는 다음이 있다:  
- **DeepWalk**
- **Node2Vec**
- **LINE**
- **GraphSAGE**

<br>

---
### 💭 오늘 챙겨간 것들
이번 글에서는 임베딩이 **어떤 데이터 타입에 따라 달라지는지**, 그리고 **텍스트부터 이미지, 그래프까지** 어떻게 각각의 정보를 숫자 벡터로 표현하는지를 정리했다.  

✔️ 텍스트는 단어 수준부터 문서 수준까지 다양한 방법으로 임베딩할 수 있고, 초기의 Word2Vec이나 TF-IDF부터 BERT, Gemini 같은 LLM 기반 모델들까지 기술이 급격히 발전해왔다.  
✔️ 이미지의 경우 CNN이나 Vision Transformer를 활용하고, 그 중간 레이어의 **활성값(activation)**을 임베딩으로 활용해 시각적인 특징을 추출한다.  
✔️ 멀티모달 임베딩은 텍스트와 이미지처럼 **서로 다른 데이터 타입을 하나의 공간에서 표현**하며, 문서 내 다양한 정보를 종합적으로 이해할 수 있게 한다.  
✔️ 구조화된 데이터 역시 PCA나 사용자-아이템 임베딩을 통해 추천 시스템이나 이상치 탐지에 활용되며, **비정형 데이터와 결합해 더 풍부한 표현**을 만드는 것도 가능하다.  
✔️ 그래프 임베딩은 관계성까지 함께 벡터에 담는 방식으로, 노드 간 연결성과 네트워크 상 위치까지 반영한다.  

다음 글에서는, **임베딩이 실제로 어떻게 훈련되고**, **Vector Search**와 **ANN 검색**, **벡터 데이터베이스**까지 임베딩을 실전에서 활용하는 방법을 정리해보겠다! 

<br><br>