---
layout: post
title:  "[Kaggle Gen AI] Day 2 - 임베딩을 더 잘 쓰기 위한 평가 기준과 RAG 활용법 🚀"
author: me
categories: [ Kaggle Gen-AI]
date: 2025-04-22 09:00:00
image: assets/images/20250402/day2_1.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_18/)에서는 임베딩이 어떤 개념인지, 그리고 그걸 어떻게 활용해서 **의미 기반의 검색이나 추천 시스템을 만들 수 있는지** 알아봤다.  

이번 글에서는 임베딩을 **어떻게 평가하고 선택할 수 있는지**, 그리고 이 임베딩을 활용해서 LLM을 더 똑똑하게 만드는 **RAG (Retrieval-Augmented Generation)** 구조까지 함께 정리해보겠다!

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=xCAVsst6WJ8&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=4)

<br>

---
### 🎯 임베딩 성능, 어떻게 평가할까?
임베딩이 **"진짜 잘 작동하고 있는지"**는 어떻게 판단할 수 있을까?  

중요한 건, 임베딩이
- **관련이 있는 데이터를 잘 찾아주고** <small style="color:gray">(retrieve relevant items)</small>, 
- **불필요한 것들은 잘 걸러내는가** <small style="color:gray">(filter out irrelevant ones)</small>이다. 

> it's kind of like separating the signal from the noise.

<br>

---
### 📈 대표적인 평가 지표들
1. **Precision (정밀도)**  
    가져온 결과 중에 **실제로 관련 있는 게 얼마나 되는지**를 본다.  
    > it's like are you hitting the target.

    <div style="margin-top: 3.3em;"></div>

2. **Recall (재현율)**  
    전체 관련 항목 중에서 **내가 실제로 몇 개나 찾아냈는지**를 본다.  
    > are you casting a wide enough net? are you getting all good stuff?

    이 두 가지 지표는 항상 **trade-off** 관계라서, 둘 다 높이는 게 쉽진 않지만 **상황에 따라 균형적으로 맞추는 게 중요하다.  

    <div style="margin-top: 3.3em;"></div>

3.  **Precision@K / Recall@K**  
    실제 서비스에서는 검색 결과 상위 몇 개만 보는 경우가 많다.  
    그래서 **'Top-K 결과가 얼마나 괜찮았는지'**를 따로 보는 지표도 많이 쓰인다.  

    예를 들어, 'Precision@5'는 "내가 보여준 상위 5개 결과 중 몇 개가 실제로 정답이었는지"를 의미하고,  
    실무에서는 이게 더 현실적이다. 구글에서 검색할 때 첫 번째 페이지를 넘어가는 경우는 많지 않기 때문이다.  

    <div style="margin-top: 3.3em;"></div>

4. **NDCG (Normalized Discounted Cumulative Gain)**
    이건 결과의 **순서까지 고려**하는 지표이다. 
    > finding a highly relevant document on page two isn't as good as finding it on page one.

    정확한 값도 중요하지만, **가장 중요한 정보가 상위에 있는지**가 더 중요할 때 유용하다.  

    그래서 'NDCG' 점수가 더 높다는 건, 사용자에게 실제로 도움이 되는 방식으로 결과를 잘 정렬하고 있다는 뜻이다.  
    > you're doing a better job of ranking things in a way that's actually useful to user.

<br>

---
### 📊 모델 비교를 위한 벤치마크와 도구들
백서에서는 모델을 공정하게 비교할 수 있는 **표준화된 벤치마크**들도 소개하고 있다.  (e.g. **BEIR, MTEB**)  
> it's like having a level playing field.

같은 기준 위에서 평가해야 모델 간 비교가 제대로 되니까, **'사과 vs. 오렌지' 비교를 피할 수 있는 도구들**이라고 보면 된다.  

이런 벤치마크를 기반으로 평가할 때는 **`Tevatron`, `pyserini`, `BEIR library`** 같은 오픈소스 도구들도 함께 활용하면 결과의 **재현성과 일관성**을 높일 수 있다. 
>  it's all about having a standardized way to measure things.

<br><div style="background: #fff9e7; border-left: 4px solid #fdc82a; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
  💡 <strong>BEIR란?</strong><br>
  BEIR는 Benchmarking Information Retrieval의 약자로, <strong>정보 검색 성능을 평가하기 위한 표준 벤치마크 데이터셋 모음</strong>이다. 
  총 19개 이상의 데이터셋으로 구성되어 있어서, 모델이 "다양한 상황에서 얼마나 잘 검색하는지"를 종합적으로 평가할 수 있게 해준다. 
</div><br>

---
### ⚖️ 성능 지표만으론 부족할 때 – 현실적인 선택 기준들
정확도 높은 임베딩 모델이라고 무조건 좋은 것은 아니다.  
**실제로 쓰려면 고려해야 할 현실적인 요소들**도 꽤 많다.  

백서에서는 아래 네 가지를 중요한 기준으로 소개했다:
- **모델 크기** <small style="color:gray">(model size)</small>
- **임베딩 차원 수** <small style="color:gray">(dimensionality)</small>
- **지연 시간** <small style="color:gray">(latency)</small>
- **비용** <small style="color:gray">(cost)</small>  

예를 들어, 아무리 정확도가 높아도 모델이 너무 커서 **현실적인 환경에 배포할 수 없다면** 무용지물이다.  

또한, 임베딩을 생성하는 데 너무 오래 걸리면, **실시간 서비스에서는 사용할 수 없다.**  

결국 중요한 건, **정확도, 속도, 비용 사이에서 균형을 잘 맞추는 것**이다. 
> you have to balance all of these factors to find the sweet spot for your particular application. 

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 즉, 임베딩 모델을 고를 때는 단순히 숫자 지표만 볼 게 아니라, <strong>이걸 우리 시스템에서 실제로 쓸 수 있는가?</strong>를 먼저 확인해야 한다.  
</div><br>

---
### 📚 RAG – 검색과 생성을 연결하는 구조
조금 구체적인 부분으로 넘어가서,  
팟캐스트에서는 **RAG (Retrieval-Augmented Generation)**이라는 구조를 소개하는데, 이건 임베딩을 활용해 **언어 모델을 더 똑똑하게 만드는 방법**이다.  

핵심 아이디어는 간단하다:  
- **"모르겠으면 찾아서 알려주자"**는 것
-  LLM이 모든 지식을 외울 필요 없이, 임베딩을 활용해 관련 정보를 찾아서 그걸 참고한 뒤 답변을 생성하는 방식이다. 

> it's like finding the needle in the haystack of your knowledge base.

<br>

---
### 🔄 RAG의 두 가지 단계 구성
1. **인덱스 생성 (create an index)**  
  - 문서를 **작은 청크**<small style="color:gray">(chunks)</small> 단위로 나눈다.  
  - 각 청크에 대해 **Document encoder**를 사용해 **임베딩을 생성**한다.
  - 이렇게 생성된 임베딩들을 **벡터 데이터베이스**에 저장한다. 

   > so it's like you're creating the searchable library of knowledge represented by these numerical embeddings.

   <div style="margin-top: 3.3em;"></div>

2. **쿼리 처리 (query processing)**  
  - 사용자의 질문을 받아서 query encoder를 사용해 **쿼리 임베딩**을 만든다.  
  - 벡터DB에서 **가장 유사한 문서 청크들을 검색**<small style="color:gray">(similarity search)</small> 한다. 
  - 그 결과를 LLM의 프롬프트에 넣어, **정확하고 관련성 높은 응답**<small style="color:gray">(accurate and relevant responses)</small> 을 생성한다. 

    <div style="margin-top: 3.3em;"></div>

이 과정에서 중요한 건 속도다.  LLM은 **실시가능로 정보를 검색하고 생성**해야 하기 때문에, 벡터 검색이 얼마나 빠르게 작동하느냐가 전체 성능에 큰 영향을 준다.  
> you're not just searching for keywords - you're searching for meaning in that embedding space.   

그래서 여기서도 **임베딩의 품질**과 **벡터DB의 효율성**이 아주 중요해진다.  

최근 몇 년 사이, 임베딩 모델의 성능은 정말 눈에 띄게 향상되었다.  
특히 Google의 최신 임베딩 모델은 **BEIR 기준 평균 점수가 10.6에서 55.7까지 대폭 상승**했다고 한다.  

<br>

---
### 🛠️ 시스템 설계와 실습 예제
다음으로, 여기서는 **임베딩 모델이 계속 발전하고 있다는 점**을 강조하며, 그에 맞춰 시스템도 **유연하게 설계**되어야 한다고 한다.  

- 새로운 모델이 나올 때마다 쉽게 교체할 수 있도록 구성하고
- 모델을 바꿨을 때 성능이 진짜 좋아졌는지 확인할 수 있는 **평가 파이프라인**도 함께 갖춰야 한다.  

<br>

---
### 🧩 고품질 라벨 데이터의 한계, 그리고 대안
임베딩 모델을 훈련할 때 생기는 대표적인 어려움은 **라벨이 붙은 고품질 데이터**<small style="color:gray">(getting highquality labeled data)</small>가 필요하다는 점이다.  

- 이건 시간이 오래 걸리고<small style="color:gray">(expensive)</small>,
- 비용도 많이 들고<small style="color:gray">(timec consuming)</small>,
- 데이터 수집 자체가 병목<small style="color:gray">(bottleneck)</small>이 될 수 있다.  

이 문제를 해결하기 위한 **흥미로운 접근법**도 함께 소개하는데:  
예를 들어, Google의 Get-Go 임베딩 모델을 개발할 때는 **대형 언어 모델을 활용해 synthetic question-document pairs를 생성**했다고 한다.  
> it's like AI helping to train better AI

이런 방식은 훈련 데이터를 빠르게 확장하고, **더 나은 성능으로 이어질 수 있는 가능성**을 열어준다.  

<br>

---
### 💭 오늘 챙겨간 것들
이번 게시물에서는, 임베딩이 실제로 얼마나 잘 작동하는지를 판단하는 기준들부터 RAG 구조처럼 임베딩을 활용한 실전 응용까지 한 번에 정리했다.  

✔️ Precision, Recall, NDCG 같은 평가 지표를 통해 **정확성과 유용성**을 어떻게 측정할 수 있는지 살펴봤고,  
✔️ 모델 선택 시에는 숫자 성능만 볼 게 아니라 **속도, 비용, 배포 가능성**까지 고려해야 한다는 점이랑,  
✔️ 임베딩을 활용해 LLM의 답변을 더 정확하게 만드는 **RAG 구조의 핵심 원리**도 함께 정리해봤다.  

다음 글에서는 **텍스트, 이미지, 구조화된 데이터, 그래프** 등 데이터의 형태에 따라 임베딩이 어떻게 달라지는지 정리해보기로  

<br><br>