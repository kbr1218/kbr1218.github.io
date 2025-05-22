---
layout: post
title:  "[Kaggle Gen AI] Day 1 - LLM은 지금 어디에 쓰이고 있을까? 실전 적용 사례 🚀"
author: me
categories: [ Kaggle Gen-AI ]
date: 2025-04-12 09:00:00
image: assets/images/20250402/day1_1.png
---
[지난 게시물](https://kbr1218.github.io/kaggle-gen-ai-podcast_9/)에서는 LLM을 더 빠르고 효율적으로 만드는 **추론 가속 기법들**에 대해 살펴봤다. 

이번 글에서는 **인트로 유닛의 마지막 주제!**
바로 **LLM의 실제 적용 사례들**을 정리해보려고 한다.  

✔️ 코드 생성이나 수학 연구처럼 **전문적인 영역부터**,  
✔️ 번역, 요약, 질의응답, 콘텐츠 제작 같은 **일상적인 활용**,  
✔️ 그리고 최근 주목받는 **멀티모달 모델들까지**  
LLM이 어떤 분야에서 **어떻게 쓰이고 있고**, 앞으로는 **어디까지 확장될 수 있는지** 들어보기~  


[👉 팟캐스트 원본 링크 바로가기](https://www.youtube.com/watch?v=Na3O4Pkbp-U&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=1)

<br>

---
### 🌍 LLM 응용 분야, 실제로 어디에 쓰이고 있을까?
요즘 LLM은 단순한 챗봇을 넘어서 **코딩, 수학, 번역, 요약, 콘텐츠 제작, 의료, 법률** 등 정말 다양한 분야에 활용되고 있다.

<div style="margin-top: 3.3em;"></div>

##### 1️⃣ 코드 & 수학 분야
LLM은 이제 **코드 생성**<small style="color:gray">(code generation)</small>이나 **자동완성**<small style="color:gray">(completion)</small>,
**리팩토링**<small style="color:gray">(refactoring)</small>, **디버깅**<small style="color:gray">(debugging)</small>,
**언어 간 코드 번역**<small style="color:gray">(traslating code between languages)</small>, **문서 작성**<small style="color:gray">(writing documentation)</small>, 그리고 **방대한 코드베이스 이해**<small style="color:gray">(helping to understand large code bases)</small>를 돕는 데까지 활용되고 있다.
- 대표적인 예시로는 **AlphaCode2** 같은 모델이 있는데, 실제 프로그래밍 대회에서도 뛰어난 성과를 보이고 있다고 한다.
- 심지어 **Fund Search**나 **AlphaGeometry**같은 프로젝트는 수학자들이 **새로운 발견을 할 수 있도록 도와주고** 있다.  

<br>

##### 2️⃣ 기계 번역 & 언어 응용
LLM은 전통적인 기계 번역보다 **더 유창하고 정확하며 자연스러운 번역**<small style="color:gray">(more fluent, accurate and natural sounding translations)</small>을 가능하게 만들고 있다.   

**텍스트 요약**<small style="color:gray">(text summarization)</small>도 점점 더 좋아지고 있고, 핵심만 잘 추려주는 방향으로 발전 중이다.   

또한, **질의응답 시스템**<small style="color:gray">(Question-Answering)</small>은 더 똑똑하고 정밀해졌고, **RAG**<small style="color:gray">(Retrieval-Augmented Generation)</small>같은 기술 덕분에 정확하고 맥락이 있는 답변을 제공할 수 있게 되었다.  

챗봇은 더 사람 같아지고, 다이나믹하고 흥미로운 대화가 가능해졌다는 평가도 많아졌다.  

<br>

##### 3️⃣ 콘텐츠 생성 (Creative Content)
콘텐츠 제작도 LLM이 완전히 바꿔놓고 있다. 
**광고 문구, 스크립트 작성, 기획서** 등 다양한 형식의 창의적인 텍스트를 생성하는 데 활용되고 있다.  

<br>

##### 4️⃣ 자연어 이해 (Natural Language Inference)
LLM은 **감정 분석**<small style="color:gray">(Sentiment Analysis)</small>,  **법률 문서 분석**, **의료 진단 보조** 등 다양한 텍스트 기반 이해 작업에도 쓰이고 있다. 

<br>

##### 5️⃣ 텍스트 분류 (Text Classification)
텍스트 분류도 점점 더 정밀해지고 있다. 예를 들어
- 스팸 탐지 <small style="color:gray">(spam detection)</small>
- 뉴스 카테고리 분류 <small style="color:gray">(news categorization)</small>
- 고객 피드백 분석  <small style="color:gray">(understanding customer feedback)</small>   

등에서 LLM의 성능이 크게 향상되었다.  

<br>

##### 6️⃣ LLM 스스로 평가자 역할 <small style="color:gray">(LLM evaluation)</small>
LLM이 **다른 모델을 평가하는 평가자 역할**로도 쓰이고 있다는 건 [이전](https://kbr1218.github.io/kaggle-gen-ai-podcast_8/)에 LLM 평가 방법인 AI Evaluator를 정리하면서 언급했었다.   
이것도 **실제 활용 사례 중 하나**!

<br>

#####  7️⃣ 텍스트 분석 (Text Analysis)
방대한 데이터셋에서 **인사이트를 뽑아내고, 트렌드를 파악하는 데**에도 LLM이 적극 활용되고 있다.  
> it’s really an incredible range of applications and we’re only scratching the surface.

<br>

---
### 🧠 멀티모달 LLM: 텍스트를 넘어서
요즘 주목받는 흐름 중 하나는 바로 **멀티모달 LLM**<small style="color:gray">(multimodal LLM)</small>이다.  
이제 LLM은 텍스트만 다루는 걸 넘어서 **텍스트 + 이미지 + 오디오 + 비디오**까지 **여러 형태의 데이터를 동시에 이해하고 생성**할 수 있게 되었다.

> Multimodal LLMs - they're enabling entirely new categories of applications. 

이런 멀티모달 모델들은 **창작 분야**<small style="color:gray">( creative content creation)</small>, **교육**<small style="color:gray">(education LLM)</small>, **보조 기술**<small style="color:gray">(assistive technologies)</small>, **비즈니스**<small style="color:gray">(business)</small>, **과학 연구**<small style="color:gray">(scientific research)</small> 등 정말 다양한 영역에서 새로운 가능성을 열고 있다.   

**완전히 새로운 형태의 애플리케이션**이 열리고 있는 중이면서, 이 기술은 앞으로의 흐름을 바꿀 수 있다고 강조하며 말했다.

> it's truly a transformative technology.

<br>

---
###  🧭 인트로 유닛 Recap
지금까지 인트로 유닛에서는  
✔️ **Transformer 구조의 기본 빌딩 블록**부터 시작해서,  
✔️ 다양한 LLM 모델들의 진화,  
✔️ **파인튜닝** 방식,  
✔️ **LLM 평가** 방법,  
✔️ 그리고 속도와 효율을 높이는 **추론 가속 기법들**까지—  

30분도 안 되는 시간동안 LLM의 핵심 개념과 작동 원리를 엄청나게 빠르게 훑었다.

> the progress has been remarkable and it seems like things are only accelerating who knows what amzaing things we’ll see in the next few years.

다음 유닛에서는 LLM을 제대로 다루기 위한 **프롬프트 엔지니어링**<small style="color:gray">(Prompt Engineering)</small>에 대해 이야기한다. 

<br><br>

