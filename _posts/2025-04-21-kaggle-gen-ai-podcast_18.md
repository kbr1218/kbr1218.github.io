---
layout: post
title:  "[Kaggle Gen AI] Day 2 - Embedding의 모든 것 – 의미를 이해하고, 검색하고, 추천하는 법 🚀"
author: me
categories: [ Kaggle Gen-AI]
date: 2025-04-21 09:00:00
image: assets/images/20250402/day2_1.png
---
이번 글에서는 생성형 AI의 핵심 기술 중 하나인 **임베딩 (Embedding)**에 대해 알아보려고 한다.  

LLM이 똑똑해지는 데 있어 가장 중요한 요소 중 하나는 단어, 문장, 이미지, 심지어 표 같은 구조화된 데이터까지 모두를 **숫자로 표현하면서도 '의미'를 잃지 않는 방식**이 있다는 점이다.  

> it's like giving machines a cheat sheet for understanding the meaning of text, images, all kinds of data.

임베딩의 개념과, 왜 중요한지, 검색이나 추천 시스템에서 어떻게 활용되는지, **의미를 숫자로 바꾸는 과정과 그 활용법**을 정리해보겠다.   

[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=xCAVsst6WJ8&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=4)

<br>

---
### 🧩 임베딩이란? – AI가 '의미'를 이해하는 방식
생성형 AI가 '텍스트의 의미'를 이해하는 데 있어 가장 핵심적인 기술 중 하나가 바로 **임베딩 (Embedding)**이다.  

텍스트, 이미지, 표와 같은 복잡한 데이터를 **낮은 차원의 숫자 벡터로 바꿔서** 그 안에 담긴 **의미나 관계**를 표현할 수 있게 해준다.  

> it's like speaking the language of AI.

사실 우리가 다루는 데이터는 너무 많고 복잡하다.  
LLM과 같은 거대한 모델들이 생겨나면서 이 데이터를 제대로 정리하고 처리하는 일이 점점 더 중요해졌고, 여기서 임베딩이야말로 **수많은 데이터 속에서 질서를 잡아주는 비밀 소스 역할**을 하고 있다.  

> they're helping us make sense of this massive amount of data that we're dealing with now.

<br>

---
### 🧪 임베딩은 어떻게 작동할까?
임베딩의 핵심은 **복잡한 데이터를 간단한 벡터로 바꾸되, 의미는 최대한 유지하는 것**이다.  

텍스트를 예로 들면, 단어나 문장을 그 자체로 다루는 게 아니라 그걸 **"의미적으로 가까운 벡터"**로 바꿔서 AI가 처리할 수 있게 만드는 방식이다.  

> so instead of dealing with raw text or pixels, you're working with these compact vectors that preserve the essential semantic information. 

결과적으로 이런 벡터는  
- 저장 공간도 적게 차지하고,
- 서로 다른 데이터 간의 **유사성을 비교**하거나, 
- **대규모 처리**를 훨씬 더 효율적으로 할 수 있게 도와준다. 

백서에서는 이걸 **위도와 경도에 비유**한다.  
> just like you can pinpoint any location on Earth with latitude and longitude.

지구상의 어느 위치든 숫자 두 개(위도와 경도)로 정확히 표시할 수 있는 것처럼, 임베딩도 텍스트나 이미지 같은 복잡한 데이터를 **숫자 벡터 공간의 한 점으로 표현**하는 방식이다.  
→ 이렇게 표현해도 **원래 의미를 잃지 않는다!**  

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 복잡한 원본을 간단한 벡터로 바꾸면서도, 그 안의 <strong>의미 구조는 그대로 보존</strong>된다는 게 임베딩의 핵심이다. 
</div><br>

---
### 💡 왜 임베딩이 중요한가?
임베딩이 중요한 이유는 단순히 데이터를 '압축'하기 때문이 아니다.  
진짜 핵심은, **'의미'를 보존한 채로 데이터를 수치화**할 수 있다는 데 있다.  

텍스트뿐 아니라 오디오, 이미지, 비디오, 그리고 데이터베이스에 있는 구조화된 데이터까지도 전부 임베딩을 통해 **하나의 공통된 벡터 공간** 안에서 다룰 수 있게 된다.  

그러니까 데이터의 형식과 상관없이 **"비슷한 의미를 가진 것들은 가까운 위치에, 다른 것들은 멀리 있게"** 표현할 수 있다.  
> that's where the idea of semantic relationships comes in.

<br>

---
### 📝 의미 기반 유사성, 어떻게 표현될까?
임베딩으로 표현하면, **왕(king)은 여왕(queen)과 가깝고, 자전거(bicycle)와는 멀다.**   
> in an embedding space, the word 'king' would be closer to 'queen' than it would be to say 'bicycle'  

왜냐하면 '왕'과 '여왕'은 의미상 비슷하지만, '자전거'는 완전히 다른 개념이기 때문이다.  

이렇게 **단어, 문장, 이미지, 사용자 행동 데이터** 등 다양한 데이터를 **의미 중심으로 정렬**해주는 게 바로 임베딩의 힘이다.  

<br>

---
### ⚙️ 효율성과 확장성
또 하나의 강점은, 임베딩이 **원본보다 훨씬 낮은 차원**으로 데이터를 표현하기 때문에  저장 공간도 적게 들고, **대규모 데이터 처리나 검색**에도 매우 효율적이라는 점이다.

> because those embeddings are lower dimensional than the original data, they let you do large-scale processing and storage much more efficiently.

요약하자면,  임베딩은 단순한 숫자 덩어리가 아니라,  **"데이터의 의미를 보존하면서, 더 똑똑하고 빠르게 다룰 수 있게 해주는 압축 표현"**이다.

<br>

---
### 🔍 검색과 추천 시스템에서 임베딩이 쓰이는 법
임베딩이 실제로 어떻게 활용되는지를 보여주는 대표적인 예시는 **검색**<small style="color:gray">(search)</small>과 **추천**<small style="color:gray">(recommendation)</small> 시스템이다.  

구글 검색을 예로 들자면,  
우리가 쿼리를 입력하면, 수십억 개의 웹페이지 중에서 **의미적으로 가장 관련 있는 문서**들을 찾아주는 구조다.  

이게 가능한 건, 모든 문서와 쿼리를 **벡터 공간 상의 점으로 표현**해두었기 때문이다.  
- 먼저, 웹페이지마다 임베딩으로 미리 계산해두고,
- 사용자의 검색어도 실시간으로 임베딩으로 바꾼다. 
- 그리고, **가장 가까운 임베딩들**을 찾아서 결과로 보여주는 방식이다. 

> the closer the embeddings are in that space, the more semantically similar they are.

단어가 똑같지 않아도, **의미적으로 비슷하다면** 가까운 결과를 찾아줄 수 있는 이유가 바로 키워드 매칭이 아닌 **의미 기반 검색**<small style="color:gray">(semantic retrieval)</small>인 것이다.  

추천 시스템도 비슷한 구조를 가지고 있다. 
사용자가 이전에 좋아하거나 관심을 가졌던 항목들의 임베딩을 기준으로, **유사한 벡터를 가진 콘텐츠를 추천**해주는 방식이다.  

이 방식의 장점은 단순히 '비슷한 항목 찾기' 그 이상이다.  
**사용자의 행동 자체를 임베딩으로 수치화**하고, 그와 유사한 패턴을 가진 다른 아이템을 찾아낼 수 있기 때문에 **개인화된 추천**이 가능한 것이다.  

<br>

---
### 🔗 공동 임베딩 (Joint Embeddings)
또 하나의 흥미로운 개념은 **공동 임베딩 (joint embedding)**이다.  

이건 서로 다른 유형의 데이터를 **같은 임베딩 공간에 함께 매핑**하는 방식이다. 

예를 들어, 텍스트 설명으로 영상을 검색하거나, 이미지에 어울리는 캡션을 생성하고 싶을 때처럼 **텍스트와 이미지 같은 서로 다른 모달리티**를 다뤄야 하는 경우가 있다.  
> it's like it's breaking down the barriers between different types of data allowing for more nuanced understanding.

공동 임베딩을 사용하면 텍스트와 이미지 모두를 **하나의 임베딩 공간 안에서 비교**할 수 있게 된다.  
그래서 텍스트 기반의 검색어로도, 의미상 유사한 이미지를 찾을 수 있다. 

<div style="background: #f9f9f9; border-left: 4px solid #007bff; padding: 12px 16px; margin: 24px 0; border-radius: 4px;">
    💬 텍스트든 이미지는 <strong>같은 언어로 이야기하는 것처럼</strong> 만들어주는 기술이다.  
</div><br>


---
### 💭 오늘 챙겨간 것들
✔️ 임베딩은 텍스트, 이미지, 오디오, 구조화 데이터 등 **거의 모든 종류의 데이터를 숫자 벡터로 표현**할 수 있는 기술이다.  
✔️ 단순히 압축만 하는 것이 아니라, **'의미'를 보존하면서 표현**할 수 있다는 점이 중요하다.  

✔️ 임베딩을 활용하면 키워드가 아닌 **의미 기반의 검색(semantic search)**이 가능하다.  
✔️ 추천 시스템에서도 **사용자의 관심사를 임베딩화**해서 비슷한 항목을 찾아주는 데 사용된다.  

✔️ 서로 다른 데이터 유형(예: 텍스트와 이미지)을 **같은 임베딩 공간에 맵핑하는 공동 임베딩(joint embedding)** 기법도 존재한다.  

다음 글에서는, 임베딩이 진짜 '잘 작동하고 있는지'를 어떻게 평가하는지, **Precision, Recall, NDCG 같은 지표들과 모델 선택의 기준들**에 대해 정리해보겠다.  

<br><br>