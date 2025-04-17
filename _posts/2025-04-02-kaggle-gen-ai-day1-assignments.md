---
layout: post
title:  "[Kaggle Gen AI] Day 1 과제 소개 - LLM & Prompt Engineering 🚀"
author: me
categories: [ Kaggle Gen-AI ]
date: 2025-04-02 09:00:00
image: assets/images/20250402/day1.png
---

첫 번째 날은 **LLM의 기본 개념**부터 시작해서, **프롬프트 엔지니어링**의 기초를 배우고 실습해보는 흐름으로 구성되어 있다.  

진행 순서는 아래 정리한 목록대로 따라가면 되는데, 각 항목에는 관련된 팟캐스트나 백서 링크도 함께 적어놨으니 바로 확인 가능하다. 
개인적으로는 팟캐스트가 백서 내용을 아주 잘 요약해주고 있고, 설명 흐름도 매끄러워서 호다닥 공부해야 한다면 **팟캐스트를 굉장히 추천!**  

<br>

---

### 🎒 Today’s Assignments
1. Complete the Intro Unit – “Foundational Large Language Models & Text Generation”:  
   <small style="color:gray">인트로 유닛 – “기초 대형 언어 모델(LLM) & Text Generation”:</small>
   - Listen to the [summary podcast episode](https://www.youtube.com/watch?v=Na3O4Pkbp-U&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=1) for this unit.  
   <small style="color:gray">인트로 유닛 요약 팟캐스트 듣기</small>
   * To complement the podcast, read the “[Foundational Large Language Models & Text Generation](https://www.kaggle.com/whitepaper-foundational-llm-and-text-generation)” whitepaper.  
   <small style="color:gray">팟캐스트를 보완하기 위해, “기초 대형 언어 모델 & Text Generation” 백서 읽기</small>  

2. Complete Unit 1 – “Prompt Engineering”:  
   <small style="color:gray">유닛 1 – “프롬프트 엔지니어링”:</small>
   - Listen to the [summary podcast episode](https://www.youtube.com/watch?v=CFtX0ZyLSAY&list=PLqFaTIg4myu_yKJpvF8WE2JfaG5kGuvoE&index=2) for this unit.  
     <small style="color:gray">유닛 1의 요약 팟캐스트 듣기</small>  
   - To complement the podcast, read the “[Prompt Engineering](https://www.kaggle.com/whitepaper-prompt-engineering)” whitepaper.  
     <small style="color:gray">팟캐스트를 보완하기 위해, “prompt engineering” 백서 읽기</small>  
   - Complete these codelabs on Kaggle:  
     <small style="color:gray">kaggle의 코드랩 실습 진행하기</small>  
     - [Prompting fundamentals](https://www.kaggle.com/code/markishere/day-1-prompting)  
      <small style="color:gray">프롬프트의 기본 원리</small>  
     - [Evaluation and structured data](https://www.kaggle.com/code/markishere/day-1-evaluation-and-structured-output)  
      <small style="color:gray">평가와 구조화된 데이터</small>  


<br>

---

### **💡 What You’ll Learn**
> Today you’ll explore the evolution of LLMs, from transformers to techniques like fine-tuning and inference acceleration. You’ll also get trained in the art of prompt engineering for optimal LLM interaction and evaluating LLMs.
The first codelab will walk you through getting started with the Gemini 2.0 API and cover several prompt techniques including how different parameters impact the prompts. In the second codelab, you will also learn how to evaluate the response of LLMs using autoraters and structured output.

🧠 Day 1에서는 **LLM의 발전 흐름**을 살펴본다! 트랜스포머 구조부터 시작해서, 파인튜닝(fine-tuning), 추론 최적화(inference acceleration) 같은 기법들이 어떻게 발전해왔는지를 간단히 정리해본다.

🛠️ 이어서 **프롬프트 엔지니어링(Prompt Engineering)**의 기본 개념도 배우고, LLM과 효과적으로 상호작용하기 위해 어떤 방식으로 질문(프롬프트)을 작성해야 하는지, 그리고 그 구조와 스타일을 다루는 법을 익힌다.

🧪 첫 번째 코드랩에서는 **Gemini 2.0 API**를 사용해 실습을 진행한다.  
프롬프트에 어떤 파라미터를 주는지에 따라 출력 결과가 어떻게 달라지는지를 직접 실험해볼 수 있다.


🧾 두 번째 코드랩에서는 **LLM 응답 평가**와 **구조화된 출력 만들기**에 대해 다룬다.  
자동 평가(auto-rating) 방법을 배우고, 모델의 응답을 좀 더 구조화된 형태로 받는 연습도 해본다.

<br>

---
<ul>
 Day-1
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_1/">[Kaggle Gen AI] Day 1 - Transformer 구조 한 눈에 보기🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_2/">[Kaggle Gen AI] Day 1 - Decoder-Only 구조, 왜 LLM은 디코더만 쓸까? 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_3/">[Kaggle Gen AI] Day 1 - Transformer 이후, LLM 진화 타임라인 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_4/">[Kaggle Gen AI] Day 1 - 오픈소스 LLM 생태계, 한눈에 보기 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_5/">[Kaggle Gen AI] Day 1 - LLM 파인튜닝 A to Z 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_6/">[Kaggle Gen AI] Day 1 - 프롬프트 엔지니어링, LLM을 잘 쓰는 기술 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_7/">[Kaggle Gen AI] Day 1 - 샘플링으로 바꾸는 LLM의 말하기 스타일 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_8/">[Kaggle Gen AI] Day 1 - 정답이 없어서 더 중요한 LLM 평가법 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_9/">[Kaggle Gen AI] Day 1 - 품질, 속도, 비용 사이에서 균형 잡기: LLM 추론 가속 기법들 🚀</a></small></li>
  <li><small><a href="https://kbr1218.github.io/kaggle-gen-ai-podcast_10/">[Kaggle Gen AI] Day 1 - LLM은 지금 어디에 쓰이고 있을까? 실전 적용 사례 🚀</a></small></li>
</ul>